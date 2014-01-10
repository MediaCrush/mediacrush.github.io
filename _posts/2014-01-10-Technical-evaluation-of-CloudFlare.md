---
layout: post
title: Technical Evaluation of CloudFlare
---

Hi there! We recently moved from AWS to Voxility, with great improvements in hosting costs,
processing times, media storage, and privacy. However, the site is noticably slower in many
regions and we need to address the issue. There are two real options:

1. Create and maintain our own Content Delivery Network
2. Use someone else's

Of course, solution #1 would be ideal and we hope that eventually, we're big enough to do
so. At the moment, though, we don't want to increase our operating costs tenfold to solve
this problem, so we've been evaluating alternatives. Specifically, we've put the most
effort into evaluating [CloudFlare](https://cloudflare.com/).

This blog post is the result of our research into CloudFlare, what we stand to gain by using
it, what we may lose, and the implications for user privacy. **Feedback from technical users
is strongly encouraged.** Leave a comment below or [email us](mailto:support@mediacru.sh) with
your thoughts.

## Performance Implications

If we switch to CloudFlare, we may be able to use their services to offer caching on
popular media, which will allow the site to perform faster from all regions via CloudFlare's
CDN. CloudFlare has good CDN coverage and our site will likely perform nice and fast for users
from nearly any location:

<img src="https://www.cloudflare.com/images/features/network-map-24.png" alt="CloudFlare Network Coverage" />

According to CloudFlare, requests will have an average latency of less than 30ms with
fewer than 10 hops. Latency is an important problem to solve with MediaCrush - our bandwidth
is great, but our isolated server has noticable latency problems from many locations.

## Security Implications

We currently serve MediaCrush over SSL. CloudFlare offers two kinds of SSL support, only to
paid users. The first offering consists of CloudFlare issuing their own certificate to
proxy our content through. Our current SSL would remain in place, and used between MediaCrush
and CloudFlare. Users connecting to MediaCrush would have their connections encrypted by
CloudFlare with the CF certificate.

The other offering, though too expensive for our use, consists of MediaCrush offering
CloudFlare our certificate for their use.

The implication of this is that we have to offer full trust to CloudFlare over connection
security and user privacy. CloudFlare would be able to read and understand any information
sent between users and MediaCrush, and would also be able to alter the content to inject
malicious content or additional tracking. This is not to fault CloudFlare - it is the
only feasible way to offer their services over SSL.

Importantly, routing all traffic through CloudFlare gives them an opportunity to record
information about visitors, and CloudFlare does so. CloudFlare will log HTTP activity
and claims to purge logs within 4 hours. Upon asking them for more details, CloudFlare
support confirmed that they *do not* respect Do Not Track settings for "malicious traffic",
and have yet to respond to my query about non-malcious traffic.

Any users who are using any kind of certificate pinning will notice a security error when
the switch is done.

For users who would rather not browse MediaCrush while routing all information through a
third party, we plan to offer `direct.mediacru.sh`, which will route traffic directly
to MediaCrush instead of through CloudFlare.

## Technical Implications

After asking CloudFlare about video streaming, they informed me that it's best to serve
videos directly through your website, and not through CloudFlare. This means that we
would not have performance gains on HTML5 video (and presumably not on audio, either).
To deal with the need to route video/audio around CloudFlare, we'll have to set up
some extra logic on the MediaCrush servers that will serve some content from a different
domain.

We won't have any problem with this, though it will increase development time and offer
some strange-looking configuration options to third party instances who are running off
of our [open source](https://github.com/MediaCrush/MediaCrush) site.

## Financial Implications

We'd be looking at $20/mo for the CloudFlare tier we're most seriously considering.
For reference, [here's our latest monthly report](https://blog.mediacru.sh/2014/01/09/Transparency-reports.html).

## Chats with CloudFlare

I've made some screenshots of my questions with CloudFlare available here if you'd like
to browse them:

TODO

## Concluding Thoughts

We're pretty confident about moving forward with CloudFlare, but we're still a bit
concerned about the privacy implications. We want to hear your thoughts on all of this.
