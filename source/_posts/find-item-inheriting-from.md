---
title: Finding items using a certain template
date: 2017-10-22
tags: [ Sitecore, Fundamentals, Templates ]
---

So few words about creating a template. When you create a template it inherits from  '/sitecore/templates/System/Templates/Template'.
This was not so obvious for me at the beggining. I thought that templates inherited from each other like in C#. Only when I had to run introduction presentation for newcommers it came to me that I was wrong. Another interesing thing is that Template template is Template. Moving further Template has some Base templates set which also use Template. Conclusion: circural inheritance is not a problem for Sitecore.

When you create an item you will se this window:

![Bucket settings](/images/inserttemplate.png)

Which clearly says that you will select Base template not a template. As in this image when you create a template suggested base template is 'Standard template'. Only difference between a Template and Standard Template is that second one has extra Workflow template in Base template section. Question I'm asking myself is: why Standard Template Base Template has values which are already in Template. It doesn't have any effect on anything but at the same time it's a bit redundant.

So how can we find items that use specific template. First thing that comes to ming is a Query or a Fast Query which will look like this:
```csharp
Sitecore.Factory.GetDatabase("master").SelectItems("/sitecore/content//*[@@templatename = 'Foo']")

```
This will quite ok, but not in all situations. When Bar template inherits from Foo ( or in other words: uses Foo as a Base template ) query won't return items which use Bar template.  Luckuly when we have a template we can find it's descendants. From there it's easy to get all templates using Foo.

```csharp
var fooId = ID.Parse("{6669DC16-F106-44B5-96BE-7A31AE82B5B5}")

var master = Factory.GetDatabase("master");
var template = TemplateManager.GetTemplate(fooId, master);

foreach (var site in template.GetDescendants()
    .SelectMany(x => master.SelectItems($"/sitecore/content//*[@@templateid='{x.ID}']")))
{
        yield return site;
}

```