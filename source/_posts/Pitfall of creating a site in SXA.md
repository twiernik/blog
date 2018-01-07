---
title: Pitfall of creating a site in SXA
date: 2018-01-07
tags: [ Sitecore, SXA 1.5 ]
---

Not so long ago I've started to work on a website with SXA - first which is not a POC, !YAY!
One of my first tasks was to create custom control for navigation purposes. I was about to push my work to repository but all of a sudden creating site was ending with SPE error. At first I founght that I've done something wrong in creating custom scaffolding but it was not it. This is what I saw in log:

```
ERROR Cannot convert argument "template", with value: "System.Object[]", for "ChangeTemplate" to type "Sitecore.Data.Items.TemplateItem": "Cannot convert the "System.Object[]" value of type "System.Object[]" to type "Sitecore.Data.Items.TemplateItem"."
```

Casue of this was sitting in tenant templates folder. I had two templates which had 'Folder' in base templates. Same effect can be caused by duplicating  Home or Page Designs tempaltes in tenant folder. I'm not the first one that stubmbled upon this behavior since it's going to be fixed in SXA 1.6. 

I was asking myself a question why this even happens and I've deducted following things. 
Initial site items are not based on templates from tenant folder. For example Home item is based on '/sitecore/templates/Foundation/Experience Accelerator/Multisite/Content/Home'. After that there is a step that switches templates of all items under Site item to templates from tenant folder. ( This is a simplification of the process). In my case I had two folders. There is an item in Site tree which is created from Folder tempalte
'/sitecore/content/Tenant/Site/Settings/Default links'. 

To avoid this I'll probably move templates to another location. For me tenant template folder was 'the right place' for templates used in the project. In this circumstances I won't use it unless necessarry. 

Yet this can be used as a feature. Let's say we want do add aditional Insert Options to 'Data/Content Tokens' item without changing SXA templates. We can achieve this by creating tempalte with base template '/sitecore/templates/Feature/Experience Accelerator/Content Tokens/Content Token Folder' and set Insert Options as needed. From now on all newly created sites in that tenant will use use this template for Content Token Folder :) 




