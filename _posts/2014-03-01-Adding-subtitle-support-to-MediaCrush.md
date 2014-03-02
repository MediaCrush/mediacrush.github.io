---
layout: post
title: Adding subtitle support to MediaCrush
---

We have just finished rolling out support for video with subtitles and closed
captioning, for the sake of our hard-of-hearing and monolingual friends. We
now support subtitles in the SRT, WebVTT, and SSA formats (the three most common),
so if you upload a video with embedded subtitles, you'll now be able to use
the settings on the bottom-right of each video to turn them on and off.

<div class="mediacrush" data-media="Zji78Nz3_NUh"></div>
<p class="small">Source: Bakemonogatari: Saitou Chiwa - Staple Stable. Subtitles by CoalGirls</p>

We've built this with the help of some great open-source projects. SRT is
converted to WevVTT [in-house](https://github.com/MediaCrush/MediaCrush/blob/master/mediacrush/processing/processors.py#L177), and then WebVTT is supported by the browser
natively when possible, or via [captionator.js](http://captionatorjs.com/) when required.
SSA support is offered by the wonderful [libjass](https://github.com/Arnavion/libjass).
Interestingly, this is the first time we've added client-side external
dependencies to MediaCrush. Thanks, guys, your projects are fantastic.

If you'd like to add subtitles to your own video, we suggest using a matroska
(mkv) container, with libass subtitles. To edit those subtitles, we suggest
another open-source project: [aegisub](http://www.aegisub.org/). Aegisub
[accepts donations](http://www.aegisub.org/), by the way! For that matter,
[so do we](https://mediacru.sh/donate).

Thanks again for using MediaCrush, and enjoy the new features!
