---
title: Ajaxmin limitations
date: 2018-09-12
tags: 
- Sitecore
- SXA
- Minification
---

>  The Microsoft Ajax Minifier enables you to improve the performance of your web applications by reducing 
>  the size of your Cascading Style Sheet and JavaScript files.
>
> -- <cite>https://archive.codeplex.com/?p=ajaxmin</cite>

I was quite surprised that last ajaxmin release was over 3 years ago ( taking last nuget version into account ). It looks like that project is dead. There is a github repository that used Ajaxmin as a codebase and even added few new features: https://github.com/xoofx/NUglify .

I'm writing about this because last week we had a problem with AjaxMin minifier, which is used by SXA build in minification feature. 

![minifier error](/images/ajaxmin.png)

This was caused by ECMAScript 2015 feature. AjaxMin supports ECMAScript 2015 however it looks like our UI's were able to find an edge case. This is the code that was the cause of exception:
``` javascript
var handler = {
	get() {
	  return "hello";
	}	
};
```
The "get" here is crucial. There is no problem after renaming function to something else. Another solution is to add "function" before get. 