---
title: Moving to a CDN
layout: post
---

So we're taking the plunge. MediaCrush is now backed by [Amazon's CloudFront Content Delivery Network](https://aws.amazon.com/cloudfront/).
This means that our media will load blazing fast from here on out.

There are some privacy implications here. There are a couple of things to note. First, your media is still stored on our Voxility servers in Germany.
Second, we only route requests for files (the actual video file, the actual image, etc) through CloudFront. This means that anything you upload will
still not be tied to you, since the uploading goes directly through our servers, and not through Amazon.

We understand that some of you still want to get away from that, so we're working on a means to opt-out of the CDN, which means your traffic will go
directly to our servers instead. The compromise here will be speed versus privacy, and we'll be happy to offer it to you as soon as possible. Our
speed was becoming a serious problem, so we wanted it fixed faster than we could implement a feature like that. And as always, you can run your own
instance of MediaCrush on your own servers for complete privacy, since we're open source and all.

Thanks for flying MediaCrush, and we hope you enjoy sharing your media even faster!
