---
author: Chris Nicola
date: '2010-03-03 11:15:00'
layout: post
slug: mvvm-and-virtualizingstackpanel-wpf
status: publish
title: 'MVVM and VirtualizingStackPanel: WPF!!?'
comments: true
wordpress_id: '68'
categories:
- .NET
- fail
- MVVM
- WPF
---

**Update**: Part two of this post with the solution I ended up using is available [here][1].

Ok this has been rather frustrating and I am hoping someone out there can help with this.  Let me start with the scenario:

> I have a TreeView that contains a two-level hierarchy of editable data objects.  The objects are databound view-models and they are bound to the TreeItem with a DataTemplate.  They have the IsSelected and IsExpanded properties also bound.
> 
> A user can add a new data object by hitting the enter key and sometimes that item is added off screen.  When an item is selected I want to make sure it is scrolled into view (sometimes it is added to the end of a long list off screen).
> 
> Additionally, there is some data validation, the user can make some mistakes and continue on while being able to correct them later.  I would like to have a “Find Next Error” feature that finds the next data object with an error, selects it and scrolls it into view.

<!--more-->

This has been harder than I had imagined.  One thing I tried was [Josh Smith’s attached behaviors solution][2].  This attaches a “BringIntoView” behavior to the OnSelected event of a TreeViewItem.  This works as long as you are not using virtualization with the tree view.  However some of the hierarchies have large collections of child objects, over 1000 sometimes (which I do realize is ridiculous and probably a sign of bad UX but it is the reality of my situation).  Without virtualization expanding one of the larger items take 5-10s which is unacceptable.  Even if my collections were smaller, virtualizing the UI is still the smart choice.

Then I looked into [another solution from Ben Carter][3].  This was intended to solve the problem of scrolling a particular item into view when using a virtualized TreeView.  Except the context is different, in this case we know which item we want to move to, we scroll to it and then set IsSelected.

The problem here is that I want to change a property in my ViewModel like “IsSelected” and have the UI respond to that change.  The virtualized TreeView however has not yet bound the view-model’s that are not in view yet so it does not know about that change and hence cannot respond to the change in any way.

Now there are still a few ways I could potentially solve this.  One idea is that I could fire an event somewhere else that sends the object I want to scroll into view to some handler that looks like Ben Carters which would then scroll the item into view and select it.  However that doesn’t feel very MVVM like to me.  Another way would be to use the parent view-model (the VM that is bound to the view containing the tree-view) somehow update the tree-view, but that would probably require the view have a ScrollIntoView(object item) method in the codebehind that the VM could then call.  Not sure I like that but it could work.

I am asking for help or guidance from the WPF/SL gurus out there.  How should I be handle this?  I am going to keep working on it so if I manage to find a solution on my own I will certainly post it here.

Update: I have a potential solution from this based on this [solution][4] which is very similar to the one Ben Carter did.  It still isn't completely to my liking but I've been playing with it and trying to make it more MVVM friendly, using the VM more (and doing some R#-fu) has also reduced the solution from 100+ lines to 13 which I am pretty proud of.  I have it mostly working but the VSP doesn't completely scroll my item into view yet, it isn't fully capable of getting to the exact position in one go.  So, still needs some work but I'm almost there, once I have it finished I will post the final code I've used in a sample solution.

   [1]: http://lucisferre.net/2010/04/21/virtualizing-stack-panel-wpf-part-duex/
   [2]: http://www.codeproject.com/KB/WPF/AttachedBehaviors.aspx
   [3]: http://code.msdn.microsoft.com/getwpfcode/Release/ProjectReleases.aspx?ReleaseId=1446
   [4]: http://blogs.msdn.com/wpfsdk/archive/2010/02/23/finding-an-object-treeviewitem.aspx

