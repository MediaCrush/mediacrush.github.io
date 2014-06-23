---
layout: post
title: The right way to encode HTML5 video and audio
---

The title of this might be a little misleading, since I'm also going to go into detail on how MediaCrush
handles video, audio, and arbitrary image formats for display on the web. This is a technical article -
if you're not a hacker, don't feel bad for not understanding it.

The goal of this article is to clear up the misinformation around how video and audio should be encoded
for the web. We've personally spent a lot of time working on it, going through several revisions and hours
of hair-tearing effort to support nearly all browsers and devices. MediaCrush can now take just about any video, audio,
or image format, and turn it into something browser friendly. Feel free to
[watch this video](https://mediacru.sh/a6bivSCFLqNJ) on any device you can get your hands on to see that
we know what we're talking about.

By the way, if you want to skip all this crap, it is very easy to
[embed MediaCrush videos and audio](https://blog.mediacru.sh/2013/10/31/Add-media-hosting-to-your-website.html)
on your own website. You can just upload your media to [MediaCrush proper](https://mediacru.sh) and
let us do the hard work. MediaCrush is also [open-source](https://github.com/MediaCrush/MediaCrush), and we
encourage you to review our code (or even use it yourself) if you get stuck, and we'd love to get your
contribution if you think we can improve it.

Let's get started. You'll want the tools of the trade. We built our service on Linux, but you can probably
get it working on OSX or Windows with enough effort. You'll need to acquire and become familar with
[ffmpeg](http://ffmpeg.org) and [ImageMagick](http://www.imagemagick.org). The latter is optional if you
don't care about how we handle arbitrary image formats.

Please note that **you cannot use your distribution-provided ffmpeg package**. You'll need the
non-distributable libx264 to handle media on the web properly. [Read up](http://trac.ffmpeg.org/wiki/CompilationGuide)
on compiling ffmpeg yourself and make sure you include libx264, fdk-aac, libmp3lame, libvpx, and libopus.
Our servers and dev machines run Arch Linux and we just install the
[ffmpeg-full](https://aur.archlinux.org/packages/ffmpeg-full/) package from the AUR.

# Media Pipeline

Here's a simplified overview of how media goes through MediaCrush:

<div class="mediacrush" data-media="MOWXS5t0jeZK"></div>

First, we try to identify *what* the uploaded file is, and then we can determine how it should be
processed. We hand it off to the appropriate processor, which has an async and a sync step. The
processing is considered "complete" and ready for user consumption when sync finishes, and errors in
async fail silently, since the media is supposedly ready to go anyway.

The user isn't told that the file is done until "sync" finishes. This step is responsible for
anything crucial to displaying the file in the browser - most of the video/audio encoding happens
here. The "async" step is executed later and does things like optimizing PNG files and compressing
GIFs.

# Format Detection

When a file is uploaded to MediaCrush, we don't make any assumptions about what it is. We ignore the
user-supplied mimetype and file extension and instead make use of our tools to see what it is. We use
ffprobe (part of ffmpeg) to examine media files and determine if ffmpeg can handle them. If ffprobe
fails, we move onto ImageMagick identify, and finally onto file (though it's worth noting that we
don't actually care what file thinks because we only let things through if ffprobe or ImageMagick
can handle it).

If you want to do format detection for user-uploaded files, you'll want to flip through the man pages
for ffprobe and ImageMagick identify to see how to use them. You might also want to
[read or use our own detection code](https://github.com/MediaCrush/MediaCrush/blob/20242a8098b04e12011769c8fb54743603c9253e/mediacrush/processing/detect.py).

# Format Conversion

Once we know what a file is, we need to convert it to browser friendly formats. We take any input
file (that falls within our supported formats) and convert it to one or more of the following:

* PNG, JPG, BMP (Images)
* SVG (Vectors)
* MP4, WEBM, OGV (Videos)
* MP3, OGG (Audio)

I should take a quick second to mention that GIF files uploaded to MediaCrush are treated as videos
and are re-encoded as video for consumption as HTML5 video.

We choose the appropriate processor and move the file along. We have a generic image, video, and audio
processor, as well as a few extra processors that are designed for specific file types. You can read
MediaCrush's processing code [here](https://github.com/MediaCrush/MediaCrush/blob/93cdea29627393760acb01492de2ce39448ce40c/mediacrush/processing/processors.py).

## Image Conversion

For any image that doesn't fall within those three formats to begin with, it's very simple to convert
them with ImageMagick:

    convert input.ext output.png

We just convert any image file to a PNG for browser display and offer a download to the original file.

## Video Encoding

This is the one you're probably waiting to hear about. You'll need to re-encode the video to three
formats: mp4, webm, and ogv. You can probably get away without doing ogv, but we're still getting
several hundred hits on our ogv files per month, so we're going to keep doing it for the time being.
Web browsers are fickle little bastards and you can never be sure it'll work with mp4 and webm alone.

Specifically, we need the mp4 file to have h.264 and aac inside, the webm needs vp8 and vorbis, and
the ogv file needs theora and vorbis. If that didn't make sense: video files are "containers" that
have one or more video and audio streams inside. The terms before refer to exactly what kind of
streams we need in each file.

Our videos also need to be using yuv420p as the pixel format, and we need to use **8-bit** libx264. This is important, 10-bit libx264 will *not* work.
Additionally, h.264 does not allow for videos with an odd-numbered width or height, so our mp4 file
is scaled to compensate for this.

Note that you **must** re-encode any videos you want to use. If you already have, say, an mp4 file,
it is probably not good-to-go and you still need to process it.

Here's how you invoke ffmpeg for each video file (forgive me for the long commands):

### mp4

    ffmpeg -i input.ext -vcodec libx264 -pix_fmt yuv420p -profile:v baseline -preset slower -crf 18 -vf "scale=trunc(in_w/2)*2:trunc(in_h/2)*2" output.mp4

This takes `input.ext` and produces `output.mp4`. Here are each of the options used:

* `-vcodec libx264` specifies that we want to use libx264 as our video codec
* `-pix_fmt yuv420p` specifies the yuv420p pixel format, which works on more platforms than other formats
* `-profile:v baseline` uses the x264 'baseline' profile (makes Android happy)
* `-preset slower` tells ffmpeg to prefer slower encoding and better results over faster encoding time
* `-crf 18` sets the *constant rate factor* to 18, which affects quality. Lower numbers are better quality
* `-vf "scale=..."` uses the *scale* video filter to make sure that the video doesn't have an odd width/height

You'll want to modify the -crf value if you want to adjust the output quality. Further reading on the x264
encoding procedure is available on the [ffmpeg wiki](https://trac.ffmpeg.org/wiki/x264EncodingGuide), but note
that it's not geared towards HTML5 users.

### webm

    ffmpeg -i input.ext -c:v libvpx -c:a libvorbis -pix_fmt yuv420p -quality good -b:v 2M -crf 5 output.webm

This will turn `input.ext` into `output.webm`.

* `-c:v libvpx -c:a libvorbis` specifies the vp8 and vorbis encodings
* `-quality good` can be good, best, or fast. 'best' is not suggested.
* `-b:v 2M` sets the target bitrate to 2 MB/s. Adjust this to fit your bandwidth requirements (it will affect quality)
* `-crf 5` sets the constant rate factor. This works on a different scale than x264.

Modify -crf and -b:v to adjust the output quality. [Further reading on webm and ffmpeg](http://trac.ffmpeg.org/wiki/vpxEncodingGuide)

We're experimenting with vp9, stay tuned to hear our thoughts on it later on.

### ogv

    ffmpeg -i input.ext -q 5 -pix_fmt yuv420p -acodec libvorbis -vcodec libtheora output.ogv

`input.ext` becomes `output.ogv`.

* `-q 5` sets the desired quality.

[Futher reading on theora/vorbis and ffmpeg](http://trac.ffmpeg.org/wiki/TheoraVorbisEncodingGuide)

### A note on resolutions

These will use the input resolution as the output resolution. If you want to encode several videos
at different resolutions (like 420p/720p/1080p), you'll want to add -video_size right before the output
video when you invoke ffmpeg. You can also drop `-vf "scale..."` from the mp4 encoding if you do this.
Common sizes include:

* `-video_size hd480`
* `-video_size hd720`
* `-video_size hd1080`
* `-video_size 4k`

I suggest you make good use of ffprobe and don't encode the video any higher than the source resolution.
You can also specify the width/height manually with something like `1920x1080`.

### "Poster" image and thumbnail

The HTML5 video tag allows you to specify a "poster". This will take the first frame of the video:

    ffmpeg -i input.ext -vframes 1 -map 0:v:0 poster.png

Apply the scale filter to get a reasonably sized thumbnail:

    ffmpeg -i input.ext -vframes 1 -map 0:v:0 -vf "scale=100:100" thumbnail.png

### Subtitles

Note: This section was written some time after this article was first published.
We did not feel comfortable offering advice on subtitles when the article was
written and promised to update it later when we were more familiar with them.

You may extract a subtitle track from a video file like so:

    ffmpeg -y -i INPUT -map 0:s:0 OUTPUT.{srt,ass}

This only removes the first subtitle track from the file. A more sophisticated
solution would be required to detect and extract only the default track, or
perhaps to handle some more complex situations. When you do get a subtitle file
you wish to use, you will find it in one of two formats:

* SRT (SubRip Subtitles)
* ASS (Advanced Sub-Station)

SRT is very similar to VTT, and VTT has support natively via
[WebVTT](https://wiki.mozilla.org/WebVTT). MediaCrush has [a
procedure](https://github.com/MediaCrush/MediaCrush/blob/master/mediacrush/processing/processors.py#L182)
for converting SRT to VTT for use on the web. However, ASS is a much more
versatile subtitle format, with support for more interesting subtitles and
effects. VTT/SRT, on the other hand, is a glorified closed captioning format.

To use VTT subtitles, you must know that only Chrome currently has native support
for WebVTT. MediaCrush uses [captionator.js](http://captionatorjs.com/) to
polyfill VTT subtitles in other browsers. Please browse captionator's
documentation to learn how it may be used.

For Chrome, it's as simple as adding a track to your video tag:

    <track src="/example.vtt" kind="subtitles" data-format="vtt" default />

For ASS subtitles, there is no native support. However, the advantages they
present make it reasonable to wish for support. MediaCrush uses
[libjass](https://github.com/Arnavion/libjass) to provide support for ASS
subtitles. This is a bit too complex for this blog post, but I suggest you browse
our code to learn more about how we applied libjass to our workflow. It's much
more involved: we have to generate CSS files, extract fonts and subtitle tracks,
and generally go to a lot more trouble to support ASS. Join us on IRC if you run
into any problems and would like to hear more.

## Audio Encoding

Audio is a little easier. Here, we've given ffmpeg the choice on quality, and it'll tend towards the
highest quality possible. We need an mp3 file and an ogg file to support all browsers.

### mp3

    ffmpeg -i input.ext -acodec libmp3lame -q:a 0 -map 0:a:0 output.mp3

* `-acodec libmp3lame` chooses the lame mp3 codec
* `-q:a 0` sets the audio quality to zero, which will choose the best (for mp3, at least)
* `-map 0:a:0` ensures that only the first audio stream and no additional streams are used

To expand on that last one, if `input.ext` were a video, that argument would ensure that only the
audio track made it into the output file.

[Further reading](https://ffmpeg.org/trac/ffmpeg/wiki/Encoding%20VBR%20%28Variable%20Bit%20Rate%29%20mp3%20audio)

### ogg

    ffmpeg -i input.ext -acodec libvorbis -q:a 10 -map 0:a:0 output.ogg

* `-q:a 10` chooses the vorbis quality, which works differently than lame.

[Futher reading](https://trac.ffmpeg.org/wiki/TheoraVorbisEncodingGuide)

# HTML

Once you've encoded your videos/audio, you can render them like this:

    <video poster="poster.png" controls>
        <source src="video.mp4"></source>
        <source src="video.webm"></source>
        <source src="video.ogv"></source>
        <p>This is fallback content</p>
    </video>

    <audio controls>
        <source src="audio.mp3"></source>
        <source src="audio.ogg"></source>
        <p>This is fallback content</p>
    </audio>

Be sure to read [Mozilla's guide](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML5_audio_and_video)
on using HTML5 video and audio in your web pages. Leave a comment here or join our
[IRC channel](http://webchat.freenode.net/?channels=mediacrush&uio=d4) (#mediacrush on freenode) if you'd like
to ask us any questions. You can also reach out through our subreddit, [/r/MediaCrush](https://pay.reddit.com/r/MediaCrush).
You're also welcome to [browse our code](https://github.com/MediaCrush/MediaCrush) for a working demo. And of course,
if all of this seems like a pain in the ass, feel free to reference our
[developer docs](https://mediacru.sh/docs) to let us do the heavy lifting for you.

### Additional Reading

Here are some more resources:

* [ffmpeg docs](http://ffmpeg.org/documentation.html)
* [webm docs](http://www.webmproject.org/)
* [x264 docs](https://www.videolan.org/developers/x264.html)
* [HTML5 video tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video)
* [HTML5 audio tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio)
