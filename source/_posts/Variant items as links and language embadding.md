---
title: Variant items and links and language embadding
date: 2018-03-22
tags: [ Sitecore, SXA, Variants ]
intro: How to remove language from link url in variants
updated: 2018-03-22
---

For our client we're creating multisite solution. Agency working in the project suggested that site language should be on a second place in url. So link for New Zeland site will look like this: www.mysite.com/nz/en. This is not that common but we have nothing against it. As Sitecore by default adds language right after hostname it looked like we needed to implement custom link manager. Someone came up with brilliant idea to achieve that without coding. Plan was to create virtual folders with language in it, like /nz/en and set language at site definition level. That almost worked. There were some urls that added language to generated links despite setting site languageEmbedding to 'never'. If links are generated somewhere in code developer can change defualt option. That happens in variants in SXA. There is an option to mark variant field as a link. Piece of code generating that link will use language embedding AsNeeded. This mode adds language to url if there is no language in cookie. First apporach was to set the cookie in language resolver processor. For some reason that didn't work in all cases. Second approach was to create link manager that resets language embedding to value to one from site definition. That one worked for us. 