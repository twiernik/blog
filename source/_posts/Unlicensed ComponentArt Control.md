---
title: ComponentArt Unlicensed Control
date: 2018-03-07
tags: [ Sitecore, Content Editor, ComponentArt ]
intro: What if license file is in place
---

Unlicensed ComponentArt controls is not that uncommon. Problem is not limited to Sitecore. Google search shows some forum questions and blog posts regarding that topic. Typical cause is that license file is missing and it has to be copied from Sitecore archive or other nstance. But... what if license file is there?

<img src="https://media.giphy.com/media/26vIfTgLGNyr3CXPq/giphy.gif" alt="whhaat?" style="width: 200px"/>


This happened on environments controlled by our customer and the cause was in security settings. Exactly one setting -> "System cryptography: Use FIPS compliant algorithms for encryption, hashing, and signing". ( It can be found in Control Panel > Administrative tools > Local Security Policy > Group Policy > Local Policies > Security Options. ) If that option is Enabled some cryptographic classes in .net won't be usable. RijndaelManaged will throw following error

> This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.


>Exception: System.InvalidOperationException
>Message: This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.
>Source: mscorlib
>   at System.Security.Cryptography.RijndaelManaged..ctor()

Obviously solution for this is Disabling mentioned option. Be sure to consult this with system administrators ;)

__Update:__
Another solution is to apply changes described in documentation: 
https://doc.sitecore.net/sitecore_experience_platform/setting_up_and_maintaining/security_and_administration/enable_fips

There are also some issues with WFFM but Sitecore managed to provide a patch.

