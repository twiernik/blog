---
title: Content Editor submenu visibility
date: 2018-01-02
tags: 
- Sitecore
- Content Editor 
---

Context menu comes in handy when we want to make using Sitecore more convenient. When our solution provides new set of operation on items we usually place commands in ribbon or context menu. With first choice we have much more space available. With latter we must be careful not to obscure whole context menu. One way to do that is to group commands into submenu. 

If your commands can be run only in certain context you will show or hide them depending on the situation. 

Hiding and showing submenu this is not that obvious. First thing we need to in order to achieve this behavior is to create mock command with proper QueryState implementation. Second and last thing is to is to show submenu. This a bit tricky but if we peek at original submenu we’ll see a javascript function responsible for showing submenu. Using that knowledge we can implement submenu which can be hidden.

``` csharp
public override void Execute(CommandContext context)
{
	var controlId = Context.ClientPage.ClientRequest.Control;
	Context.ClientPage.ClientResponse.ShowContextMenu(
		controlId, "right", $"{controlId}_menu");
}
```