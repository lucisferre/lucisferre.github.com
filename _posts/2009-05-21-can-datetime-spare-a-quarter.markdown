---
title: Can DateTime spare a quarter?
date: 2009-05-21 00:14:00 Z
categories:
- ".NET"
- C#
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '87'
---

Ok so this isn't really very impressive for a blog post, but I am curious if anyone has a more sensible way to do this.

The DateTime struct understands, seconds, minutes, hours, days, months, years, etc. However, I find it kind of odd that the `DateTime` struct doesn't understand "quarters". Quarters of a year are a common concept in any finance application and unlike weeks they are not all the same length.

What I need to determine is the start and end dates of the quarter for the current date minus one month. There are a million ways I could do this:

  1. I could just have a helper class with the 4 start date of the quarter and have it do comparisons. Seems a bit overkill to have a class. 
  2. I could just compare the `DateTime.Month` value to values 1, 4, 7, 10 to figure out which quarter I am in and then set the start and end dates. This is nice and simple. 
  3. Instead I did this: 

``` csharp
//Get the previous month mod(3) (0,1,2) 
var monthInQuarter = (DateTime.Today.Month + 1) % 3;

//Set start and end dates
EndDate = DateTime.Today.AddDays(-(DateTime.Today.Day));

//Start date is the first day of the beginning of the quarter
StartDate = EndDate.AddMonths(-1*monthInQuarter).AddDays(-(EndDate.Day-1));
```

mmmmm... mod math ;-).  Ok so it isn't that cool, but I'm sure I'm going to need to use this again.

Chris

Edit: Fixed my total failure with brackets.
