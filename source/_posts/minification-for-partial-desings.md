---
title: SXA - Minification for partial designs
date: 2018-10-19
tags: [ Sitecore, SXA, Minification]
---

Not that long ago I wrote about AjaxMin problems with some newer javascript syntax. That problem was in a project in which I'm more like a satelite then regular member of a team. Frontend guys choosed to disable minification. From my perspective that was the end of a problem. I was quite shocked after recently discovering that authors are not able to edit partial designs without some hassle. They needed to add `aodisabled=1` to query string. Otherwise they were not able to use Experience Editor and following error could be seen:
```
Inner Exception: Object reference not set to an instance of an object.
   at Microsoft.Ajax.Utilities.FunctionObject.SafeIsReferenced(HashSet`1 visited)
   at Microsoft.Ajax.Utilities.FunctionObject.SafeIsReferenced(HashSet`1 visited)
   at Microsoft.Ajax.Utilities.JSVariableField.get_IsReferenced()
   at Microsoft.Ajax.Utilities.ActivationObject.AnalyzeNonGlobalScope()
   at Microsoft.Ajax.Utilities.ActivationObject.AnalyzeScope()
   at Microsoft.Ajax.Utilities.GlobalScope.AnalyzeScope()
   at Microsoft.Ajax.Utilities.JSParser.InternalParse()
   at Microsoft.Ajax.Utilities.Minifier.MinifyJavaScript(String source, CodeSettings codeSettings)
   at Sitecore.XA.Foundation.Theming.Bundler.AssetBundler.MinifyContent(MemoryStream memoryStream, StreamWriter writer, OptimizationType type)
```
After some digging I've found out that SXA doesn't respect minification settings in certain scenario.  If it cannot resolve Page Design for current item it will use default minification settings. Solution for this is to create custom `AssetConfigurationReader` and `AssetLinksGenerator` implementations and use the latter in `SxaLayout.cshtml` to minify assets. `AssetLinksGenerator` must be implemented just to use custom `AssetConfigurationReader`.  Beginning of a method that needs to changed in that class looks like this:
``` csharp
public virtual AssetConfiguration ReadConfiguration()
{
    AssetConfiguration assetConfiguration = new AssetConfiguration();
    Item designItem = ServiceLocator.ServiceProvider.GetService<IPresentationContext>().DesignItem;
    ....
```
In order to 'fix' the problem we have to replace `GetService<IPresentationContext>().DesignItem`  with `GetService<IPresentationContext>().PageDesignsItem` and rest of the code accoringly. After that minification was not performed while editing Partial Designs. [Gist with implementation.](https://gist.github.com/twiernik/bef94db2f8b421b605d4a94b7a38ebed)