---
title: Search tweak for ID's
---
Take a look at this file:

```
8908 15:55:39 INFO  ExecuteQueryAgainstLucene (social_messages_master): +container:mastercd8da5e23b654a14b7a69f41181ce172en +(_template:a6575aec8bc54591b7308fcdc0ccddb5 _template:7e49b0aa5eba4a02852c4744e2288ac2) - Filter : 
8908 15:55:39 INFO  ExecuteQueryAgainstLucene (social_messages_master): +container:masterd51cfe9b67d940c39aa731456fb9aefben +(_template:a6575aec8bc54591b7308fcdc0ccddb5 _template:7e49b0aa5eba4a02852c4744e2288ac2) - Filter : 
8908 15:55:39 INFO  ExecuteQueryAgainstLucene (social_messages_master): +container:master616c618f863141998cde34fbeb398c6cen +(_template:a6575aec8bc54591b7308fcdc0ccddb5 _template:7e49b0aa5eba4a02852c4744e2288ac2) - Filter : 

```

It's part of a search log from Sitecore. If you ever looked at indexing you know that item ID's in Lucene don't have dashes.
In log you can see there is a template with ID 7e49b0aa5eba4a02852c4744e2288ac2. So imagine you want to see this template and you 
paste this value into Content Editor search box and it doesn't work. ARRR. It doesn't work. Why? Because this kind of string; although it's a valid guid; is not recognized as Item ID. My first reaction to this situation was to use online tool which changed format with dashes but after few times I had enough.
I'm making baby steps in Sitecore world and this looked like good opportunity to learn something new. 

Turn's out it was super easy to do. After finding search pipeline it was a matter of minutes to write a processor which handles such case.

``` csharp
public class IDSearch
{
    public void Process(SearchArgs searchArgs)
    {
        Assert.ArgumentNotNull((object)searchArgs, "searchArgs");
        Guid guid;

        if (!Guid.TryParse(searchArgs.TextQuery, out guid))
            return;
        
        var item = searchArgs.Database.GetItem(new ID(guid));
        if (item == null)
            return;         

        var result = SearchResult.FromItem(item);
        args.Result.AddResultToCategory(result, Translate.Text("Direct Hit"));
        args.AbortPipeline();
    }
}
```


