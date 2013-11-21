---
title: How we open-sourced our startup
layout: post
---

A little background: [MediaCrush](https://mediacru.sh) is an [open-source](https://github.com/MediaCrush/MediaCrush) website
for hosting media - images, video, and audio. I made this website with my friend [jdiez](https://github.com/jdiez17) over the course of the past few
months. Here's how it all started, four months ago:

    <SirCmpwn> I'm thinking of making a website for hosting gifs
    <SirCmpwn> but when you upload a gif, it converts it to ogv and renders it with HTML5 video
    <SirCmpwn> faster load time + better quality
    <jdiez> hmmmm
    <jdiez> sounds like a great idea
    <SirCmpwn> I bought gifquick.com
    <SirCmpwn> .net
    <jdiez> not one of your best names
    <SirCmpwn> you're a name
    <jdiez> I have one

We meant to start out with this site to convert GIFs to HTML5 video, which makes them load *way* faster. And we still do
this - every GIF uploaded to our site becomes an HTML5 video. We ditched the crappy "gifquick" name I came up with, added
support for more kinds of media, and we are proud to remove our "beta" notice on our
[911th commit](https://github.com/MediaCrush/MediaCrush/commit/0473bd5fb8164fa6c8318e5d219c9b532dda2d49), four months later. Here's what it's like to run an open-source website as a startup.

## Open source and MediaCrush

Us MediaCrush folk really like GitHub, and we use it for our [personal lives](https://github.com/SirCmpwn) [all the time](https://github.com/jdiez17).
We used it to collaborate on the site, seeing that we live in different countries. It was a private repo at first, but
after only a few dozen commits, we made it public, put up the work-in-progress site, and started talking about it here
and there.

The result has been overwhelmingly positive. We've got a good number of [collaborators](https://github.com/MediaCrush/MediaCrush/graphs/contributors), people are
[reporting bugs and requesting features](https://github.com/MediaCrush/MediaCrush/issues) all the time, and we get really good feedback from devs in our
[IRC channel](http://webchat.freenode.net/?channels=mediacrush&uio=d4) on Freenode.

In addition to getting contributors and bug reports, we also get a lot of good will from the hacker community. On Hacker
News, we've seen comments like [this one](https://news.ycombinator.com/item?id=6189664) praising our philosophies. People
talk about your stuff more if it's open source!

## The bottom line

Of course, all of this is meaningless if you can't be successful. There's the risk that someone will take your work,
build on top of it, and defeat you. However, we haven't seen that at all. Googling our name doesn't pull up any third-
party instances of MediaCrush, and no one seems to be stealing our secret sauce after we hit 100k uniques the other day.

Instead, we've gotten a great response from the community! We've also had lots of people [building](https://github.com/hypereddie10/jCrush)
and [open-sourcing](https://github.com/blha303/SnapCrush) neat [things](https://github.com/headdetect/SharpCrush) on top of MediaCrush.
The community that builds up around your work is a lot cooler when your work is open source.

A big part of what we do is taking care of our users. None of this works without love. We love them, and want them to love
using the site. You should follow our lead in defending user privacy - we serve all pages over HTTPS, we store nothing
about our users, and we are [completely transparent](https://mediacru.sh/transparency) about our operations.

## Roadmap

So how do you make money with an open source website like this? We don't make much money out of ads, and the (generous)
donations we receive probably won't last forever. We're working on a cool new thing that will let us manage and deploy
new instances of MediaCrush for you. You'll be able to rent a MediaCrush server from us and have a private place to
upload and share media. Of course, you can still set up a private instance on your own - we'll just make it nice and
simple for you. We also plan to offer support to those using our private instances, as well as handle the deployment
of updates and any other maintenance they might need. We aren't sure how this'll work out, so make sure you come back to
read the follow up a few months from now!

As always, thanks for using MediaCrush, and a big thanks to our awesome community!
