---
title: MediaCrush for Nerds
layout: post
---

If you aren't familiar with [MediaCrush](https://mediacru.sh), a quick introduction: MediaCrush is a media
hosting website designed for speedy hosting, without sacrificing quality. We run uploaded static images
through various lossless compression tools, but the big draw is our GIF support. Whenever you upload a gif
to MediaCrush, we convert it to a video using h.264 and Theora to encode it, which gets us dramatically
smaller files. More details are available on the [about page](https://mediacru.sh/about). You might also
check out [this corresponding post](/2013/07/19/MediaCrush-for-users.html) aimed at non-technical users. In
addition to GIFs and static images, we also support video and audio uploads.

We're just tidying up around the place here before entering public beta. We're happy to have support
from Reddit Enhancement Suite on day one, which we think will really benefit us. All the features we wanted
to get in have gotten in, and we're pretty satisfied with the site. I just wanted to take a moment to ramble
about our thoughts behind the site, from a developer perspective.

### Privacy and Transparency

First, I want to talk a little bit about how much effort we put into making it perfect for our users. We
wanted to be sure it was everything you could ever want out of your image hosting site. Here's a few things
we made sure to do for the sake of our technically adept friends:

* All pages are served over https, all the time, from day one.
* MediaCrush respects your "Do Not Track" settings. We won't include Google Analytics on the page if you have
  [Do Not Track](http://donottrack.us/) set in your browser.
* We allow all users to opt-out of ads forever, without making a fuss about it. One click and they're gone.
  It's even possible to opt-out without ever seeing an ad at all - [click here](https://mediacru.sh/serious)
  and click the link in the relevant paragraph under "privacy".
* Transparency is key. We publish monthly reports on just about everything we can report on. You can browse
  them [here](https://mediacru.sh/transparency). That includes Google Analytics reports, server hosting
  costs and advertising revenue, donations we get, everything.
* The only thing we store about you is your hashed IP address when you upload a gif. It's impossible for us
  to get your original IP address from this. Our http logs don't show IPs, either. The only reason we store
  your hashed IP at all is to enable us to identify malicious users and prevent them from  uploading. If our
  database was stolen or we were pressured into releasing it, you'd remain completely anonymous.
* We accept bitcoin for [donations](https://mediacru.sh/donate)! Both of the guys here are big fans of
  bitcoin. You can use them to donate anonymously to our effort.

You can learn all the gory details about our stance on privacy and such on our
["serious" page](https://mediacru.sh/serious).

### Open Source Software

Everything about MediaCrush is completely free, as in freedom. The site is written in Python, runs on Linux,
and uses open-source tools for media processing. Not only that, the site itself is 
[completely open source](https://github.com/MediaCrush/MediaCrush). We welcome your contributions.
Additionally, we tried to be as encouraging of clones as we can. In the repository, you'll find instructions
on how to set up your own MediaCrush clone. We're not too scared of competetion - clones won't get our name,
server capacity, RES support, etc, but we still want to make it so that you're able to set up your own
version if you'd like. We selected the very liberal MIT license for MediaCrush, so feel free to do whatever
you want with it.

MediaCrush is also pretty dependent on open source software. The biggest of these is
[ffmpeg](http://ffmpeg.org), an amazing piece of software that can do anything with video files. It runs our
processing server and makes those gifs into videos for extra sexy load times. We also use software like
jhead to strip exif tags out of JPG files.

### Beta Time

Most of you will probably read this after we hit beta, but for us, we're still getting ready. We're pretty damn
excited about this software, and hope it'll go far. You can let us know your thoughts by commenting here.
You can also join us and listen in on (or contribute to) development chatter on our
[IRC channel](http://webchat.freenode.net/?channels=mediacrush&uio=d4) ([irc://](irc://irc.freenode.net/mediacrush)).
If we've missed anything at all, please let us know and we'll do our best to get it in. Thanks for using
MediaCrush!
