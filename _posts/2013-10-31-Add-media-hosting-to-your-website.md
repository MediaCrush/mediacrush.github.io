---
title: Effortlessly add media hosting to your site with MediaCrush
layout: hosting_post
---

Feel like your website could use some videos, or perhaps an audio player? Or maybe you're building some forum software and
you want to allow users to upload videos/audio/pictures without building out all the hosting and encoding in the backend.

<div class="mediacrush" data-media="fuMbnbF26cu9"></div>

We're happy to offer you the brand-new **mediacrush.js** JavaScript library for in-browser JavaScript. This easy-to-use
library will help you add MediaCrush integration into any website you please. Let us handle all the backend work. We'll
take your files and re-encode them for cross-browser support, we'll losslessly compress images, we'll do all the stuff
you should be doing but don't have time to. You'll all our great lossless media compression backend, plus fast and
reliable hosting, with all of our awesome [privacy benefits](https://blog.mediacru.sh/2013/07/19/MediaCrush-for-nerds.html)
thrown in.

## Embedding Media

Here's how easy it is to embed media in your site:

1. Add this: `<script type="text/javascript" src="https://mediacru.sh/static/mediacrush.js"></script>`
2. Add this: `<div class="mediacrush" data-media="wmoJGnBebB0L"></div>`

When the page loads, mediacrush.js discovers all of these divs and renders them into full-fledged media viewers. You can
also add these divs into your page later and use `MediaCrush.renderAll()` to rediscover them, or use
`MediaCrush.render(element)` to render a single one.

Want to host a video on your blog? Here's how:

1. Upload your video to [MediaCrush](https://mediacru.sh) and note the hash in the URL
2. Add the script to your blog
3. Add `<div class="mediacrush" data-media="..."></div>` to your post

Of course, you can still directly use the embed code provided on [every MediaCrush page](https://mediacru.sh/wmoJGnBebB0L).

We'll take a MP4, OGV, WEBM, MP3, OGG, PNG, JPG, GIF, or SVG file (up to 25 MB), do all the re-encoding for you, and spit
out a lovely HTML5 media player. We're adding support for new media formats all the time, too, so keep an eye out for new
kinds of supported media!

Here's two embedded MediaCrush files:

<div class="mediacrush" data-media="wmoJGnBebB0L"></div>
<p class="small">Source: Yoko Kanno feat. The Seatbelts - Tank!</p>

<div class="mediacrush" data-media="a6bivSCFLqNJ"></div>
<p class="small">Source: <a href="http://myanimelist.net/anime/11597/Nisemonogatari">Nisemonogatari</a></p>

## Uploading Media

Say you're working on some message board software. You don't want to deal with all the fuss of encoding videos right, or
hosting pictures, or anything else involved in letting your users upload media. The solution - MediaCrush. Add an input
field to your page like you usually would:

    <input type="file" id="mediacrush-upload" />

Then, you can hook up to a link or a button or anything else (see that mockup on top of the page for an example), then
simply do this:

    var file = document.getElementById('mediacrush-upload').files[0];
    MediaCrush.upload(file, function(media) {
        // Media is now uploaded, wait for processing to finish
        media.wait(function() {
            alert('Your file has been uploaded: ' + media.url);
        });
    });

There's a demo right on this page! Try it out:

<input type="file" id="mediacrush-file" style="display: none;" />
<div>
    <a href="#" id="mediacrush-upload"><img width=16 height=16 src="https://blog.mediacru.sh/static/logo.svg" style="display: inline; position: relative; margin-right: 2px; top: 2px;" />Upload to MediaCrush</a>
    <div class="progress"></div>
</div>
<div id="mediacrush-results" style="height: 300px; overflow: auto; border-radius: 5px; border: 1px solid #000; margin-top: 5px;">
</div>

The full source code of that demo is [here](https://gist.github.com/SirCmpwn/64235a3c5d104868c11b).

## Last Thoughts

We hope you find mediacrush.js useful. The full documentation is available [here](https://mediacru.sh/docs/mediacrush.js).
The library itself is MIT licensed, so feel free to modify and distribute it as you please (just like the
[rest of MediaCrush](https://github.com/MediaCrush/MediaCrush)).

Now, we should mention this: we don't get any ad revenue from people who use mediacrush.js to embed media or
offer a MediaCrush-based hosting service. Just like we ask of our other [API](https://mediacru.sh/docs/API)
users, please consider making a small [donation](https://mediacru.sh/donate) in exchange for our services. Of
course, this is completely optional - don't feel pressured to do so if you don't want to. You're also not
required to insert the MediaCrush branding into pages that use mediacrush.js, but we'd love it if you did! A
vectorized version of our logo can be found [here](https://blog.mediacru.sh/2013/07/27/MediaCrush-logo.html).

If you're having trouble, want to offer feedback, or just want to chat, you can find us in
[#mediacrush on irc.freenode.net](http://webchat.freenode.net/?channels=mediacrush&uio=d4).
You can also keep an eye on our [Twitter feed](https://twitter.com/mediacru_sh) for more updates.
You should drop us a line if you do anything neat with mediacrush.js! And of course, thanks for using MediaCrush!
