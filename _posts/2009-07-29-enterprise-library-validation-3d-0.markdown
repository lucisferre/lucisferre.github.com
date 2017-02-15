---
title: Enterprise Library Validation != 0
date: 2009-07-29 04:19:00 Z
categories:
- ".NET"
- fail
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '30'
---

There has to be a better way to tell Microsoft's Enterprise Library's Validators that zero is invalid...

```csharp
[PropertyComparisonValidator("Zero", ComparisonOperator.NotEqual, MessageTemplate = "Entry amount should not be zero", Ruleset = "Standard")]
public decimal Amount
{
  get { return _entry.Amount; }
  set { if (IsSelected && value != _entry.Amount) { _entry.Amount = value; NotifyPropertyChanged("Amount"); } }
}

public decimal Zero { get { return 0.0M;} }
```
