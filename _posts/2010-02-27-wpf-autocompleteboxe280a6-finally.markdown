---
author: Chris Nicola
date: '2010-02-27 17:32:46'
layout: post
slug: wpf-autocompleteboxe280a6-finally
status: publish
title: WPF AutoCompleteBox. finally!
comments: true
wordpress_id: '67'
categories:
- .NET
- MVVM
- WPF
---

I was delighted to find out that the WPF Toolkit now includes an AutoCompleteBox, something that is a standard UI component and has been available in Silverlight for a while now.  Up until now I've had to make do with something I sort of hacked together out of a ComboBox and some creative ViewModel bindings.

The AutoCompleteBox is easy to use with databinding, since I typically use MVVM when working with WPF I used a ViewModel with a property containing the list of auto-complete items and bound the AutoCompleteBox to that property.  The code is shown below.

<!--more-->

The AutoCompleteBox:
    
```xml
<toolkit:AutoCompleteBox 
  ItemsSource="{Binding Clients}" 
  ValueMemberPath="FullName" 
  FilterMode="Contains" 
  Text="{Binding ClientName}" 
  IsTextCompletionEnabled="True"/>
```

My ViewModel and Client classes:
    
```csharp
public class ViewModel {  
  ...   
  public List<Client> Clients { get { return _clientsList; } }  
  public string ClientName {  
    get { return _entrySplit.ClientName; }  
    set {   
      if (value != _clientName) { _clientName = value; NotifyPropertyChanged("ClientName");}  
    }  
  }  
  ...  
}  

public class Client {  
  public virtual string Fullname { get; set; }  
  public virtual string Lastname { get; set; }  
  public virtual string Firstname { get; set; }  
  public virtual string RepCodeString { get; set; }  
  public virtual int ClientUID { get; set; }  
}
```

In this case the `ItemSource` is a list of objects that provide the auto-complete values.  If your `ItemSource` is of type `IEnumerable<string>` you are done at this point, you do not need to specify anything else.  If you are binding to a more complex object then you would either specify the `ValueMemberBinding `using a binding expression (allowing you to specific an `IValueConvertor` as well),` `or alternatively the `ValueMemberPath` as I have done above.  Also, if you actually want to have auto-complete functionality and not just a drop down with suggestions you must set `IsTextCompletionEnabled="True"`.

You can specify a `FilterMode` with several different options like 'Contains', 'StartsWith', etc. which will determine how auto-complete will filter your ItemSource list.  If you have a need to use a customized filter you can use the `ItemFilter `property which allows a lambda based filter expression.  You can do this in your ViewModel by binding to a property like this:
    
```xml
<toolkit:AutoCompleteBox 
  ItemsSource="{Binding Clients}" 
  ValueMemberPath="FullName"  
  ItemFilter="{Binding ClientFilter}" 
  Text="{Binding ClientName}"   
  IsTextCompletionEnabled="True"/>  
```
```csharp
public AutoCompleteFilterPredicate<Client> ClientFilter {  
  get { return (search, item) => item.Fullname.Contains(search); }   
}
```

Obviously this is a somewhat silly filter to define since it does the same thing as setting `FilterMode="Contains"` but you can see the flexibility you have here.

One caveat about the AutoCompleteBox is that it does not recognize the TabIndex property.  It is currently just a direct port of the Silverlight control and apparently Silverlight does not have a TabIndex concept.  It turns out that the TabIndex property isn't really needed in WPF either, you will always tab through your controls in the order they are defined in your XAML.  So to ensure tab order will be respected, do not set the TabIndex property on any of your controls and just make sure you place them in the XAML in the desired tab order.

If you want more information on using the AutoCompleteBox and the other features of the WPFToolkit, [Jeff Wilcox has provided links to the download and several useful blog posts][1].

   [1]: http://www.jeff.wilcox.name/2010/02/wpfautocompletebox/

