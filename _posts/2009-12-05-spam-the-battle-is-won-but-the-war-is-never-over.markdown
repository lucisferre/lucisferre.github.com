---
author: Chris Nicola
date: '2009-12-05 19:13:00'
layout: post
slug: spam-the-battle-is-won-but-the-war-is-never-over
status: publish
title: 'Spam: The battle is won, but the war is never over'
comments: true
wordpress_id: '54'
categories:
- .NET
- blogengine
- blogging
- spam
---

![wallofspam][1]

Well I wanted to get one more post out before I go.  I've been pretty dedicated to posting regularly since moving and I plan to stick to that commitment.  I'm leaving Monday for Cuba which will be awesome and a desperately need a vacation right now.  I will not even be taking my laptop!  I do have some books though, I might try to get most of the way through _Lean Software Development_ before I get back.

<!--more-->

This weekend I thought I would finally tackle a small thorn in my side (actually a thorn in the side of just about anyone who has a blog) the comment spam.  BE.NET is a great blog engine but I have noticed it has more spam problems than some.  Before I switched I used to use ReCaptcha, but even that was starting to fail as spam technology finally seems to be able to get a decent percentage of the images correct now (edit: Justice tell me that they actually _pay_ people to process the ReCaptcha forms).  Actually when you think about it ReCaptcha is a terrible idea since as it's used, it likely is also helping some commercial OCR products learn.  Those in turn are probably used in comment spam software to beat the challenge-response, just a theory though.

BE.NET thankfully comes with an Akismet plug-in that guards against spam, great but it doesn't seem to work properly.  It also has a StopForumSpam plugin as a backup, so ideally if one fails the second one takes it's turn. 

Despite this protection, I've noticed spam getting through on my blog as well as a couple other blogs using BE.NET that I read like [Justice Gray's][2].  Today I looked at the BE.NET code to try to figure out what might be happening.  This is what I found:

```csharp
static void RunCustomModerators(Comment comment) 
{ 
    DataTable dt = _customFilters.GetDataTable(); 
    dt.DefaultView.Sort = "Priority"; 

    foreach (DataRowView row in dt.DefaultView) 
    { 
        string fileterName = row[0].ToString(); 

         ICustomFilter customFilter = GetCustomFilter(fileterName);             

         if (customFilter != null && customFilter.Initialize()) 
         { 
             comment.IsApproved = !customFilter.Check(comment); 
             comment.ModeratedBy = fileterName; 

             UpdateCustomFilter(fileterName, comment.IsApproved); 

             // the custom filter tells no further
             // validation needed. don't call others
             if (!customFilter.FallThrough) break; 
         } 
     } 
 } 
```

which is the code run when a comment is posted.  Now this snippet is from the Akismet filter (which implements ICustomFilter obviously):

```csharp
public bool FallThrough { get { return true; } } 
```

I'll forgive anyone who doesn't see this right away, the code isn't entirely clear about what is happening, but it doesn't take too long to see the problem, here it is step by step:

  1. Spam is posted by some asshole 
  2. Akismet checks, finds spam and sets IsApproved = false; 
  3. If FallThrough = true StopForumSpam takes it's turn, since FallThrough is always true this happens always 
  4. StopForumSpam checks, since it isn't all that reliable it doesn't find spam and resets IsApproved = true 
  5. Spam is approved 
  6. Blog owner /facepalm's 

So the solution was relatively simple, just make sure the Akismet filter properly sets a FallThrough value to "false" when it find spam.  Personally I think this is a design flaw though, the spam filter should **always** stop once **any** of the spam checkers believes it has found spam.  It is less work to re-approve false positives than it is to clean-up false negatives given that over 80% of all blog comments are spam (figure taken from the Akismet website).

I've posted the issue and the patch on the [BE.NET codeproject][3] site so I'm sure it will work it's way into the trunk soon.

Anyways, that's it till I get back from Cuba!

   [1]: /images/wallofspam.jpg (wallofspam)
   [2]: http://graysmatter.codivation.com/
   [3]: http://blogengine.codeplex.com/workitem/10811?ProjectName=blogengine

