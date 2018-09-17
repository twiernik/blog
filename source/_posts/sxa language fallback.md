---
title: Language fallback with SXA
date: 2018-09-17
tags: 
- Sitecore
- SXA
- Language Fallback
intro: Story about setting language fallback in SXA
---

On one of site's we're currently working at there are some pages that will have more then one languge version. Since there are only few of them we want to keep English menu, navigation and so on. Language fallback seems to be a natural choice in this situation. Most cubersome task here was to configure Item level fallback on almost all SXA templates. I was a bit uncertain about switching it for all templates. I went thought all templates and made a list of those which seem to be used as datasource for components. I left of Settings, Sites, Rendering paramters and so on. I wrote small powershell script for creating standard values ( where needed ) and setting Item Language Fallback field to 1. After doing that I had another idea for achieving same result which seems to be cleverer. The idea is to create one setting item - let's call it DataLanguageFallback. Insted of changing Language Fallback for each item template I could use DataLanguageFallback template as a base template for all templates mention earlier. Doing that would allow us to turn on and off Language Fallback for all descendant templates with one click. And that click would be used on checkbox in StandardValues of DataLanguageFallback. It's kind of neat that we can turn on and off that feature with one click, only weird thing is that this config is in /templates branch. 
Beside SXA tempaltes I had to adjust project ones but that went much faster. Last thing was to check if everything works. Only List Components needed intervention (PageList, FileList, ... ). In ListRepository and it's descendants there is a code that retrieves children of the datasource that have a version. In our case all items had English version only as we wanted to relay on Language Fallback. That resulted in empty Social Media links. Bummer was that I needed override each repository since fault behavior was in base class. In this case composition would be better. Overall whole thing went smooth and took less time then I anticipated.

