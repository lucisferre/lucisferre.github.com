---
author: Chris Nicola
date: '2010-04-21 16:41:00'
layout: post
slug: virtualizing-stack-panel-wpf-part-duex
status: publish
title: Virtualizing Stack Panel WPF!??? Part Duex
comments: true
wordpress_id: '77'
categories:
- .NET
- fail
- WPF
---

So, as I mentioned in an update to part one I did end up finding a solution to the problem.  It was a hybrid of all the other, largely broken solutions out there.  I should also mention I was not happy with this solution, it felt hacky and performed somewhat poorly, instead I took a different approach altogether to get the desired result for my users.  Nonetheless, despite not using this solution I think the resulting code is worth writing about and considering so here it is.

A couple of conclusion I came to from working on this and discussing it with others:

  1. The ability to bring an item into view should be built-in, not a hack or afterthought.  I can't understand how it was released this way. 
  2. The VSP current design completely breaks the MVVM pattern, since virtualized items don't actually have a view and thus the view-model is left unbound to the UI (@kellyleahy and I discussed this at ALT.NET Seattle and he has some ideas to solve this that I may try to implement in a custom VSP implementation) 

<!--more-->

I do realize that WPF and SL development probably began before the MVVM pattern was considered fundamental, so that is probably part of the reason for this (and other) oversights.

You can download the solution code [here][1].  It provides a static `BringIntoView` extension method for the TreeView control.  Now lets look at the code in more detail.

The first thing to note is that to do this at all we have to extend the VSP in order to expose a protected method that is protected for no valid reason I can think of: 
    
```csharp
public class MyVirtualizingStackPanel : VirtualizingStackPanel {
  public void BringIntoView(int index) { BringIndexIntoView(index); }
}
```

Lest you think this is just a limitation of my solution, let me point out that both of the solutions provided by MS employees used the same approach.

One of the things I didn't like about the other solutions is that they did not make use of the MVVM pattern at all.  They roughly searched through the tree for the item they wanted.  I understand why though, these are only sample solutions provided for guidance, which is fair enough (though it isn't exactly hard to use MVVM even in a sample).

I made sure to pass my VM instance for the item I wanted to bring into view to my `BringIntoView` method and extended the TreeView control like this:

```csharp
public static TreeViewItem BringTreeViewItemIntoView(this TreeView treeView, TreeViewItemModel item) {
  if (item == null) return null;
  ItemsControl parentContainer = (ItemsControl) treeView.BringTreeViewItemIntoView(item.Parent) ?? treeView;
  return parentContainer.BringItemIntoView(item);
}
```

Finally, I found that initially this was not exactly working as expected.  It would scroll my TreeView but not   
actually scroll my item into view.  My working theory is that the VSP does not actually know how large it's items are until it renders them first.  Again probably another short-coming (or defect) in the current VSP.

I tried numerous things to make this work and I found the most reliable way was to make a second call to bring the item into view, but use the Dispatcher to call it at a lower priority than my first call.  In theory this should always get called after the VSP renders the item first:
    
```csharp
for (int i = 0; i < container.Items.Count; i++) {
  vsp.Dispatcher.Invoke(DispatcherPriority.ContextIdle, (Action<int>) vsp.BringIntoView, i);
  var nextitem = (TreeViewItem) container.ItemContainerGenerator.ContainerFromIndex(i);
  if (nextitem.DataContext == item) {
    nextitem.Dispatcher.Invoke(DispatcherPriority.ContextIdle, (Action) nextitem.BringIntoView);
    return nextitem;
  }
}
```

Anyways I am no WPF/SL expert.  I am certain there is a better way, however I am not certain there is a better way without implementing a completely new VSP.  If there is I would certainly like to hear about it though.

I will probably spend a bit of time playing with the VSP code from SL (thank you for open sourcing it MS) to see if I can't learn more and find a more appropriate solution.

[TreeViewItemFinder.cs (Gist)][1]

   [1]: https://gist.github.com/755546#file_tree_view_item_finder.cs

