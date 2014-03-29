---
layout: post
title: Compliments as a service
---

We built this for fun this morning:

    $ curl https://mediacru.sh/compliment
    You would look good in glasses OR contacts.

It's CORS enabled, so you can use it via JavaScript if you want.
[Send us some compliments](mailto:support@mediacru.sh) and we'll add them to our
little database.

<blockquote id="compliment"></blockquote>
<script type="text/javascript">
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://mediacru.sh/compliment');
xhr.onload = function() {
    document.getElementById('compliment').textContent = this.responseText;
};
xhr.send();
</script>

We're still trying to figure out a good way to integrate this into MediaCrush proper.
Right now, we use this to generate compliments that our servers email us in their
daily status reports, but a user-facing use-case would be great.

As always, thanks for using MediaCrush!
