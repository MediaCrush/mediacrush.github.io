---
layout: post
title: Important API change 
---

Hey! We recently discovered a vulnerability that allowed an attacker to send a user a link that once clicked would result in the file being deleted without asking for confirmation. Unfortunately we had to make a breaking change to the API in order to fix this. If you have built an application that uses the delete API, you might have to make some changes to your application. However, if you use the official wrappers it will just be a matter of updating them; we have updated [PyCrush](https://github.com/MediaCrush/PyCrush/commit/6e016bc4a9faee7fd8de7678bdd43f1de56b7ce6). If you use [mediacrush.js](https://github.com/MediaCrush/MediaCrush/commit/f8d68aa7dde12bb78c1658c6609a1f0f95bd5f1b), you will not need to make any changes unless you're serving a copy manually.

When we originally discovered the vulnerability we [disallowed external calls to this API](https://github.com/MediaCrush/MediaCrush/commit/d86d3c0ea5b95901269302f064ba984285ea2487) while we worked on a more [permanent solution](https://github.com/MediaCrush/MediaCrush/pull/479), which involves changing the delete endpoint to `DELETE /api/<hash>`. We took additional steps to ensure disruption was minimal; a call to the affected API will result in a redirect to a confirmation page. Well behaved API clients should treat an unrecognised return code as an error, and perhaps alert the developer.

The good news is that it never happened in practice. The exploitation of this issue leaves a very distinguishable signature on our access logs: a hit to `/api/hash/delete` with a referal other than mediacru.sh is a clear exploitation. We have verified that this attack vector was never exploited, and no users were actually affected.

Sorry for the inconvenience, and happy MediaCrushing!
