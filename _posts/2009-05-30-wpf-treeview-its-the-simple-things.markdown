---
author: Chris Nicola
date: '2009-05-30 04:18:00'
layout: post
slug: wpf-treeview-its-the-simple-things
status: publish
title: 'WPF TreeView: It’s The Simple Things'
comments: true
wordpress_id: '8'
categories:
- .NET
- WPF
---

I have generally found learning WPF very rewarding. It is a significant improvement to UI development over the old WinForms approach. That said you can tell it is new and it still needs a lot of work. I often say that WPF makes complex things simple (spend some time with the MVVM pattern and you will beging to understand this) but often makes simple things >:XX...

Today I ran into one of those simple things...

<!--more-->

The WPF TreeView like all WPF controls is very, very flexible in what you can do with it, how you can extend it, change the look, feel, etc.  I have it currently being used as a data editing control.  Each TreeViewItem displays a custom UserControl containing a set of editable TextBox and ComboBox controls whenever it is selected.  The user can edit this data, and when it is deselected it will persist to the database via nHibernate.

One of my users noticed that whenever she presses the keypad's minus key (in .NET this is `Key.Subtract`) it collapses the current branch.  Since this is an accounting applications, and hence, the users are accountants, they like to use the keypad to make decimal entries.  So essentially this is preventing them from entering negative numbers in their standard way.

"No problem," I figured, I'm sure I can disable the keyboard navigation, or intercept the event or something like that with little trouble.

Turns out there is no way to quickly disable the TreeView's default navigation control.   Even worse there doesn't even seem to be a way to customize it.  A pretty glaring oversight in my opinion.  You can see this discussed briefly here in this [CodeProject article by Josh Smith][1].  He solves this by handling the `PreviewKeyDown `event.  This has the unfortunate side-effect of disabling _all text entry_ in the TreeView.  Not a solution for a data entry control.

In the end I just had to handle the `PreviewKeyDown `even for the textboxes that handle the decimal inputs.  This is by no means an ideal solution but it works.  To get a good UI feel, you need to make sure it checks the current selection, cursor position and behaves as a user would expect it to.  Here is the event handler:

```csharp
/// <summary>
/// Since TreeView is retarded, we need to insert a minus sign when the user uses the keypad this way
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void txtAmount_PreviewKeyDown(object sender, KeyEventArgs e)
{
  if (e.Key == Key.Subtract)
  {
    var textbox = sender as TextBox;
    int start = textbox.SelectionStart;
    int end = start + textbox.SelectionLength;
    string beginning = textbox.Text.Substring(0, start);
    string ending = textbox.Text.Substring(end, textbox.Text.Length - end);
    textbox.Text = beginning + "-" + ending;
    textbox.SelectionStart = start + 1;
    e.Handled = true;
  }
}
```

   [1]: http://www.codeproject.com/KB/WPF/CustomTreeViewLayout.aspx

