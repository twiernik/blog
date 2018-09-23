---
title: bypassing html cache
date: 2018-09-23
tags: [ Sitecore, caching, tricks ]
intro: How anyone can bypass your html cache
---

Lately I've been taking a closer look on how page rendering works in Sitecore. Yesterday I've stumbled upon interesting behavior. It looks like by adding certain query parameter to address can result in skipping html cache. To test this I've written small rendering that displays DateTime.Now and marked it as cacheable. Below you can see how experiment went.
<img src="/images/skiphtmlcache.gif" style="width:350px" alt="bypassing html cache" />
As you can see adding ```sc_pd_view``` param with any value to query string results in bypassing html cache.  Same results can be observed for param ```sc_pd```. 