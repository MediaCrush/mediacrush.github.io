---
layout: post
title: MediaCrush now supports nearly every media format
---

Starting today, MediaCrush supports **over 500** different media formats. This includes everything
we already supported and more. Some of my favorite new formats are lossless audio (like FLAC), and
more video formats (like MKV and WMV), or more vector graphics, and raw photographs. We'll also take
more kinds of audio, like raw PCM data. Odds are, for any particular media format, we'll support it.
[Want to give it a try?](https://mediacru.sh)

A [whole lot of work](https://github.com/MediaCrush/MediaCrush/pull/449) went into this, and there
are lots of internal improvements as well. You should notice faster processing times and more
stability overall. Instead of whitelisting certain file formats, we examine every uploaded file and
try to determine if we're actually capable of processing it at all, with tools like ffmpeg and
imagemagick. That way, we actually support everything we *possibly can* support. We still run things
through the pipeline and convert them all to browser-friendly formats so that you can easily view
them on the web.

Thanks to this change, MediaCrush is now uniquely qualified as the best service for sharing any
kind of media you have. Enjoy uploading all the media you can think of, and thanks again for using
MediaCrush!
