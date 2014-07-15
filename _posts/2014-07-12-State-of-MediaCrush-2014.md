---
# vim: tw=82
title: The State of MediaCrush 2014
layout: post
---

Today is MediaCrush's first birthday! We want to take this opportunity today and
every year going forward to give everyone some insight into how MediaCrush is
doing and where we've gone in the past year.

In upcoming years, we plan to include improvements we've made over the past year,
as well as all of our financial info, some interesting stats and raw analytics
data, a rundown of downtime events and how we learned from them, and any
interesting stuff to read on the net about us.

Additionally, we are going to post a lighter-weight version of this each quarter
with our finances and analytics, to replace the monthly reports we aren't very
good at publishing on time.

<div class="centered">
  <a href="#improvements">Improvements</a> &mdash;
  <a href="#finances">Finances</a> &mdash;
  <a href="#analytics">Analytics</a> &mdash;
  <a href="#downtime">Downtime</a> &mdash;
  <a href="#discussions">Discussions Online</a> &mdash;
  <a href="#press">Press Coverage</a>
</div>

<div id="improvements"></div>

## New Features and Improvements

We have improved MediaCrush quite a bit since our simple beginnings:

* Moved from Amazon Web Services to Voxility
  * VPS to dedicated hardware
  * Moved from US to Germany
  * Consistent pricing instead of pay-by-usage
* Support for all video, audio, and image formats
* Keep track of user history in their browser
* Allow users to delete their files
* Expose a public API
* Allow users to upload via URL or from their clipboard
* Support for albums of files
* Support for uploads from mobile devices
* Export albums as zip files
* Upload recordings from your microphone
* Subtitled videos

These changes are described in 1,797 individual changes to MediaCrush in the past
year, with the help of 18 contributors, 1,237 stars on GitHub, and 117 forks.

MediaCrush has also published 9 other open-source projects [on our
GitHub](https://github.com/MediaCrush), including this blog, our Firefox and
Chrome extensions, and more.  Lots of features have also been contributed to
[Reddit Enhancement
Suite](http://redditenhancementsuite.com/) on behalf of MediaCrush, the full list
of which you can [see
here](https://github.com/honestbleeps/Reddit-Enhancement-Suite/commits?author=SirCmpwn&page=1).
MediaCrush has also contributed to the [Prism Break](https://prism-break.org/) and
[Alternative Internet](https://github.com/redecentralize/alternative-internet)
projects to help fight for user privacy in a hostile internet. We have also
published [detailed
information](https://blog.mediacru.sh/2013/12/23/The-right-way-to-encode-HTML5-video.html)
for those who might want to build a similar service for their users.

<div id="finances"></div>

## Financial Information

Today, our costs are fairly constant (though this month we will be renewing our
domain and buying a new certificate). We cannot say the same of our costs in the
past, which have fluctuated as we experiemented with different hosts and CDN
providers. Our historical costs are reported here:

* July 2013
  * Hosting: $102.80
  * Domain names: $60
  * Certificate: $199
  * MediaCrush logo (free, but we offered a tip): 0.2 BTC or ~$20
  * Infographic: 0.7 BTC or ~$73
* August 2013
  * Hosting: $198.45
  * Reddit ads: $100
  * Reddit gold for /u/MediaCrusher: 0.0425 BTC or ~$5
* September 2013
  * Hosting: $216.13
  * Reddit ads: $100
  * Twitter ads: $88.36
* October 2013
  * Hosting: $274.25
* November 2013
  * Hosting (AWS): $423.61
  * Hosting (Vox): $138.53
* December 2013
  * Hosting: $199.00
* Janurary 2014
  * Hosting: $199.00
  * CDN: $56.62
* February 2014
  * Hosting: $199.00
  * CDN: $438.29
* March 2014
  * Hosting: $199.00
  * CDN: 235.45
* April 2014
  * Hosting: $199.00
* May 2014
  * Hosting: $199.00
* June 2014
  * Hosting: $199.00

Other sources of revenue such as donations and ads have helped make some of this
back:

* July 2013
  * Advertising: $0.23
* August 2013
  * Coinbase: 0.01 BTC or ~$1
  * Advertising: $13.94
* September 2013
  * Advertising: $3.53
* October 2013
  * Coinbase: 0.25 BTC or ~$50
  * Advertising: $18.04
* November 2013
  * Coinbase: 0.025 BTC or ~16
  * Dwolla: $1
  * Flattr: €4.50
  * Advertising: $47.21
* December 2013
  * Coinbase: 0.25 BTC or ~$191.33
  * Flattr: €6.73
* Janurary 2014
  * Coinbase: 0.0005 BTC or ~$0.30
  * Flattr: €6.24
* February 2014
  * Coinbase: 0.01 BTC or ~$3
  * Flattr: €6.24
* March 2014
  * No revenue
* April 2014
  * Advertising: $18.82
  * Flattr: €28.35
  * Gittip: $26
* May 2014
  * Advertising: $8.23
  * Coinbase: 0.02 BTC or ~$12
  * Flattr: €4.50
  * Gittip: $36
* June 2014
  * Advertising: $10.58
  * Coinbase: 0.005 BTC or ~3
  * Flattr: €7.50
  * Gittip: $30

Our overall profit is $-3061.33. Don't let this number worry you - our donations
and advertising revenue is on a steady trend upwards, and Jose and I have no
problem paying for the site ourselves. If you'd like to help, [you're welcome to
contribute](https://mediacru.sh/donate).

<div id="analytics"></div>

## Analytics and Stats

MediaCrush collects anonymous information on users who do not have [Do Not
Track](http://donottrack.us/) set on their web browsers. You may want to review
our [privacy policy](https://mediacru.sh/serious) for more information on this.

Over the past year, MediaCrush has been visited by 2,820,148 unique people, who
together amassed 5,815,361 page views. Each visitor spent an average of 33 seconds
on the site, and 36% of them came back again later. Not included in this number
are users who visited through the API, which includes users of Reddit Enhancement
Suite.

![](https://cdn.mediacru.sh/F0rD02O9QoK-.png)

These visitors uploaded a total of 154,821 files which occupy 544 GB of storage
space on our servers. Users have also created 3,451 albums. These files are:

* 92,971 JPG files
* 32,076 PNG files
* 574 files of other image formats
* 25,171 videos (includes GIFs)
* 2,281 audio files
* 987 SVG files

There are also a significant number of files in formats that are officially
unsupported, but that we currently allow users to upload anyway.

Our visitors mostly came from the United States and Canada, but we had visitors
from around the world. Only Chad, the Central African Republic, and North Korea
did not visit MediaCrush.

![](https://cdn.mediacru.sh/6KDbL2BIHYd4.png)

Our users were mostly Chrome users (52%). The runner up is Firefox, then the
Android Browser.

![](https://cdn.mediacru.sh/o5P2AmWDQboc.png)

Chrome users are the best at being up-to-date, and a clear trend between versions
is noticable on the site. In the past week, nearly 100% of Chrome users arrived
with Chrome 35. Opera users are the worst at being up-to-date, with almost 40% of
Opera users still on Opera 12. Give it up, guys. We can't give you the best
experience unless you update.

This data has shaped how we handle browser support for the many bleeding-edge web
technologies MediaCrush employs. If you'd like to help with our experiments, give
our [HTML5 video survey](https://survey.mediacru.sh) at try.

94% of our traffic comes from Reddit, followed by Facebook at 3%, and Twitter at
1%. Hacker News, Tumblr, Google+, and several others have also fed traffic into
MediaCrush.

![](https://cdn.mediacru.sh/mbmNdyZAAQF7.png)

There is also a rather even distribution of ISPs that serve MediaCrush to their
users:

![](https://cdn.mediacru.sh/dL6ci4eVJuX3.png)

ISPs, by the way, who are fiercely lobbying the FCC for permission to ruin the
internet. The rules they want to follow would ruin MediaCrush, who clearly cannot
afford to pay bribes to get content to you faster. You can reach a human at the
FCC by calling +1 (888) 225-5322, then pressing 1, 4, 0.

We won't tell you what the most-viewed pages are (privacy reasons), but we will
tell you the stats for the top ten pages:

1. 244,249 pageviews and 237,751 unique visitors
2. [Our home page](https://mediacru.sh): 125,879 pageviews and 85,681 unique visitors
3. 76,559 pageviews and 70,792 unique visitors
4. 68,331 pageviews and 61,492 unique visitors
5. 62,429 pageviews and 59,229 unique visitors
6. 57,652 pageviews and 50,127 unique visitors
7. 57,007 pageviews and 47,969 unique visitors
8. 54,773 pageviews and 50,984 unique visitors
9. 53,905 pageviews and 44,024 unique visitors
10. 52,813 pageviews and 44,574 unique visitors

The last 5 of these are porn, 3 are sports clips, and one is a TV clip.

Raw analytics data is available as CSV files for you to peruse:

* [Sessions per day](https://mediacru.sh/transparency/state-of-mediacrush-2014/overview.csv)
* [Languages](https://mediacru.sh/transparency/state-of-mediacrush-2014/language.csv)
* [Locations](https://mediacru.sh/transparency/state-of-mediacrush-2014/location.csv)
* [Browsers](https://mediacru.sh/transparency/state-of-mediacrush-2014/browser-os.csv)
* [Internet Service Providers (top 5,000 only)](https://mediacru.sh/transparency/state-of-mediacrush-2014/isp.csv)
* [Referrals (top 100 only)](https://mediacru.sh/transparency/state-of-mediacrush-2014/referrals.csv)

<div id="downtime"></div>

## Downtime

There have been two severe (greater than 10 minutes) downtime events, as well as 3
minor (fewer than 10 minutes) events. One of the severe events was planned and
lasted 2 hours. The other was unplanned and lasted 4 hours. All minor events were
planned. There have also been two events during which the site did not go down,
but experienced extremely high page load times under stress.

The first severe downtime event was as a result of a significant change to our
internal architecture to support [arbitrary file
formats](http://blog.mediacru.sh/2013/12/13/Supporting-all-media-types.html). We
had to spend quite a few cycles examining our existing data. Retrospectively, we do
not see a way this could have been improved and do not plan on mitigating this
sort of event in the future.

Each minor planned event was the result of server reboots so we could get an
updated kernel for various reasons. We do not plan to mitigate these events, but
we have researched possible solutions such as kexec.

The second severe downtime event was the result of stupidity. We updated some of
our dependencies and did not test carefully enough to notice that we missed some
other dependencies that depended on our updated dependencies (what?). Anyway, we
won't do that again.

The two events where we had abnormal load caused us to improve our architecture to
support higher loads. The first event drove our traffic to around 2000 concurrent
users, while simultaneously undergoing a DDoS attack. Individually, we could
handle these as a result of careful nginx configuration to keep the brunt of the
load away from our sensitive gunicorn backend. However, both together ended up
bringing down our backend, leaving only direct links in place. We mitigated this
issue by adding caching to nginx that only permits one request per second to proxy
through to the backend.

The second load event occured under strange and unknown circumstances. It appears
that gunicorn had a stroke and we still do not know why (this problem happened
last week). We were able to "solve" the problem by switching to uwsgi for our
backend web server. We are very happy with uwsgi and will likely continue without
gunicorn into the foreseeable future.

<div id="discussions"></div>

## Discussions

MediaCrush has been discussed on several social networks. If you're interested in
reading these threads, we've provided links to the ones we know about:

* Hacker News: [\[1\]](https://news.ycombinator.com/item?id=6773039) [\[2\]](https://news.ycombinator.com/item?id=6189397)
* 4chan (NSFW): [\[1\]](https://archive.rebeccablacktech.com/g/thread/S40959612)
* Hubski: [\[1\]](http://hubski.com/pub?id=143800)

There have also been countless [Reddit
posts](http://www.reddit.com/domain/mediacru.sh/new) (NSFW) and
[tweets](https://twitter.com/search?f=realtime&q=mediacru.sh&src=typd).

<div id="press"></div>

## Press Coverage

* [The Red Ferret](http://www.redferret.net/?p=41651)
* [Linux Magazine](http://www.linux-magazine.com/Online/Features/Anonymous-Sharing-with-MediaCrush)
* [Digital Trends](http://www.digitaltrends.com/social-media/how-to-make-your-gifs-load-faster/)
* [Gigaom](http://gigaom.com/2013/10/08/like-inception-for-memes-mediacrush-converts-animated-gifs-back-into-videos/)
* [Oarena (greek)](http://osarena.net/logismiko/applications/mediacrush-anichtou-kodika-web-ipiresia-diamirasmo-archion.html)
* [BetaNews](http://betanews.com/2014/01/27/mediacrush-an-ad-free-open-source-photo-music-and-video-host/)

## What else?

MediaCrush is mostly made by two guys, [Drew
DeVault](https://twitter.com/sircmpwn) and [Jose
Diez](https://twitter.com/jdiezlopez). We thought you might want to know what
we've been up to outside of MediaCrush.

Jose: I have been experimenting with 3D printing, and contributed some code to
open source projects in the 3D printing community. I have also built a couple of
RC quadcopters (with lots of printed parts!), and doing some freelance software
development as well as keeping up with university.


Drew: I'm working on a number of things, like continuing my
[calculator operating system](http://knightos.org), [working on a website for
Kerbal Space Program mods](http://beta.kerbalstuff.com), and maintaining a bunch
of other open source projects. I'm also writing a bunch of software for companies
that need someone to write a bunch of software for them.

## Going Forward

We'll keep on keeping on with MediaCrush. You can keep up with us on
[Twitter](https://twitter.com/mediacru_sh) or on
[Reddit](https://pay.reddit.com/r/mediacrush). Leave a comment here if you have
any questions we could answer, or just to chat.

Thanks for flying MediaCrush!
