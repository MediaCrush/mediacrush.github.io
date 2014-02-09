---
title: How we streamed HD video to 1600 people at once without trying
layout: post
---

Good websites are designed that way from the start. MediaCrush has been
built to scale since day one, and we'll tell you why our system survived
today's unexpected surge of traffic - without much effort on our parts.

MediaCrush hosts videos, audio, and images for users, super fast. One of
our users uploaded a video and shared it on Reddit, and it was quite
popular:

<div class="mediacrush" data-media="DeIQDSVYPW3X"></div>

As of writing, there are still ~400 people watching that video. The load
was way more than we've ever had before (compared to the previous record of
300 active uses), and our site performed great. Response times were fast,
the server's resource usage was doing fine, and everything was hunky dory
during the entire period of high traffic.

So here's our architecture:

* Actual video files are served through CloudFront (this saved us, I'm sure)
* HTML, CSS, JS, is served to our web server (we only have one), which is:
  * nginx proxying to gunicorn (gunicorn serves HTML, nginx serves static CSS and JS)
  * Python web (Flask) looks up media info from Redis (no caching) when serving view pages
  * Hosted on Voxility
  * 8 cores, 32GB RAM
  * Everything over HTTPS with SPDY v2

The web server also handles media transcoding and other processing
jobs, hence the 8 cores.

The reason we stayed alive is probably because we serve videos on
CloudFront, but our web server did its fair share, too. The web
server was handling a couple hundred requests per second at the peak
of this burst, and it was pushing about 100MB/s of network usage.

That being said, the CloudFront costs were notable:

<div class="mediacrush" data-media="O9KoA7HNo8F7"></div>

We've been designed to scale from the start. Here are our plans for
accomodating even higher loads in the future:

* Our processing servers are distributed, we can add new processing
  servers at any time
* Our web servers can be distributed and load-balanced if we use a shared Redis

We have identified a few places for improvement:

* Redis results and view pages can be cached
* We are currently using 6% of our 2.4 TB RAID array for storage,
  and we can expand that to 8 TB before we have to worry about new
  storage servers. We don't have a good way of expanding out to
  serveral storage servers when the time comes.

Since MediaCrush is open-source, you can read all the details in
our [GitHub repository](https://github.com/MediaCrush/MediaCrush),
or even set up your own instance of MediaCrush. You might be
particularly interested in our
[nginx config](https://github.com/MediaCrush/MediaCrush/blob/master/config/nginx.conf).

## Want in?

Our rock-solid services include a full
video, audio, and image processing stack that can take nearly
any media file and produce browser-friendly formats, ready to
be shared (on Reddit or anywhere else) or embedded in your
website. And we do it totally open-source, free, and without
ads. You should check out our
[excellent developer API](https://mediacru.sh/docs) if that
sort of thing interests you.

Thanks for using MediaCrush!
