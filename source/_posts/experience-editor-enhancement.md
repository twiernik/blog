---
title: Experience Editor calls with exceptions
date: 2017-10-19
tags: [ Sitecore, Experience Editor, Exceptions ]
---

About week ago Experience Editor didn't allow me to edit pages. Google didn't help and everything looked like it was an expected behavior. I was fooled by Sitecore because everything seem to look ok: 
All requests had status 200 and even debugging SetEditMode.js look exactly normal - except is was returning false for 'ExperienceEditor.EditMode.CanEdit' call. This was king of a mistake, because with each request exception details were added to the log. Maybe it's time for another habbit or tooltip integration for dev instances. 

Anyway.. if httpRequestProcessed pipline will throw an error user won't be able to edit page in Experience Editor. Developer will be able to diagnose situation but for regular user it will be a bit confusing situation. It's a bit narrow case but still. In my case error was caused by custom X-UA-Compatible header in web.config plus SXA with FixInternetExplorerCompatibilityHeaders processor. I came up with two step solution. First step is to replace Sitecore.ExperienceEditor.Speak.Server.RequestHandler with custom class. Problem with this class is that it flushes request which causes headers to be sent before end of the request. In that case there is no way to change them ( this is what FixInternetExplorerCompatibilityHeaders processor does ) when exception occurs. Second thing is that we want to expose the error. That's right: there is no error handler. Each experience editor call returns json object with bool error field. Sadly it doesn't cover all situations. I managed to make slight change to postServerRequest field in  ExperienceEditor.js around line 727.

```javascript
    postServerRequest: function (requestType, commandContext, handler, async) {
      var token = $('input[name="__RequestVerificationToken"]').val();
      jQuery.ajax({
        url: "/-/speak/request/v1/expeditor/" + requestType,
        data: {
          __RequestVerificationToken: token,
          data: unescape(JSON.stringify(commandContext))
        },
        success: handler,
        error: function() {

          var testi = this;
          var showErr = function() {window.parent.Sitecore.PageModes.PageEditor.notificationBar.addNotification(
                new window.parent.Sitecore.PageModes.Notification("apiFail" +  commandContext.url, 
                  "error occured while requesting " + testi.url ,
                {
                  actionText: "Show error " + testi.url,
                  type: "error"
                }));
                console.error(testi);
              };
            setTimeout(showErr, 1000);
        },
        type: "POST",
        async: async != undefined ? async : false
      });
    }
```

I don't know why timeout was needed but was necessary to avoid nulls. I won't use this in production  until I'll find a way to wait for all fields to be initialized. Using alert is an option here but really doesn't fit the platform. 

![better status](/images/ee500.png)

![error visibility](/images/eeerror.png)