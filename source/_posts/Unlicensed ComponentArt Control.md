---
title: ComponentArt Unlicensed Control
date: 2018-03-07
tags: [ Sitecore, Content Editor, ComponentArt ]
---

Unlicensed ComponentArt controls is not that uncommon. Problem is not limited to Sitecore. 
Google search shows some forum questions and blog posts regarding that topic.
Typical cause is that license file is missing and it has to be copied from Sitecore archive or other instance.
But... what if license file is there?

![whhaaat](https://media.giphy.com/media/26vIfTgLGNyr3CXPq/giphy.gif)

This happened on environments controlled by our customer and the cause was in security settings.
Exactly one setting -> "System cryptography: Use FIPS compliant algorithms for encryption, hashing, and signing".
( It can be found in Control Panel > Administrative tools > Local Security Policy > Group Policy > Local Policies > Security Options. )
If that option is Enabled some cryptographic classes in .net won't be usable. RijndaelManaged will throw following error

 This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.

```
Exception: System.InvalidOperationException
Message: This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.
Source: mscorlib
   at System.Security.Cryptography.RijndaelManaged..ctor()
```

Obviously solution for this is Disabling mentioned option. Be sure to consult this with system administrators ;)