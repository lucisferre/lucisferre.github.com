---
title: Even images need boundaries
date: 2010-01-22 10:58:00 Z
categories:
- ".NET"
- clean code
- csharp
- TDD
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '63'
---

This morning I was looking for some simple code to do basic resizing of images.  A website I am working on has an image upload and the upload needs to restrict the images to maximum height and width values.  This is a very simple and probably fairly common task.  As a rule, I don't usually keep a reference library worth of code snippets in my head (or anywhere for that matter) since I can quickly google anything when I need it and doing so I found [this post][1] which had a solution I liked.

<!--more-->

Immediately upon reading the code, something about the geometry of the solution seemed off to me.  As far as I could tell, this would only correctly shrink the image if the boundary values were 1:1.  Clearly, no such restriction is placed on the input values, so we can assume that this is a bug.  I made some corrections and did some _clean-code_ refactoring and ended up with what I thought was working code.  Actually, trying it out revealed another bug though, the logic was entirely backwards. 

It was resizing by the width when it needed to use height and vice versa.  A couple more changes and some more _clean-code_ refactoring and the final corrected code is as follows:


```
private static Image ResizeImage(Image original, int maxHeight, int maxWidth) { 
    if (ImageIsSmallerThanMaxSize(original, maxWidth, maxHeight)) return original; 
    var scaleFactor = (AspectRatioIsLessThanMaxRatio(original, maxWidth, maxHeight)) 
        ? Decimal.Divide(maxHeight, original.Height) 
        : Decimal.Divide(maxWidth, original.Width); 
    return ScaleImage(original, scaleFactor); 
} 
 
private static Bitmap ScaleImage(Image original, decimal scaleFactor) { 
     var output = new Bitmap((int) (original.Width * scaleFactor),(int) (original.Height * scaleFactor)); 
     var tmpGraph = Graphics.FromImage(output); 
     tmpGraph.InterpolationMode = System.Drawing.Drawing2D.InterpolationMode.HighQualityBilinear; 
     tmpGraph.DrawImage(original, 0, 0, output.Width, output.Height); 
     return output; 
 } 
  
 private static bool ImageIsSmallerThanMaxSize(Image original, int maxWidth, int maxHeight) { 
     return original.Width < maxWidth && original.Height < maxHeight; 
 } 
  
 private static bool AspectRatioIsLessThanMaxRatio(Image original, int maxWidth, int maxHeight) { 
     return Decimal.Divide(original.Width, original.Height) < Decimal.Divide(maxWidth, maxHeight); 
 } 
 ```

I think you will agree that not only is the code correct but it is much clearer that it is, in-fact, correct.  So what can I take away from this:

  * Always try to double check your blog post code (I'm sure this statement is going to bite me in the ass) 
  * Never take anything you find via. Google or the internets for granted 
  * Cleanliness is next to correctness 
  * I should have TDD'd this code 

   [1]: http://www.mikeborozdin.com/post/High-Quality-Image-Resizing-with-NET.aspx

