---
author: Chris Nicola
date: '2009-10-06 08:22:00'
layout: post
slug: the-7-habits-of-highly-effective-agile-teams
status: publish
title: The 7 habits of highly effective agile teams
comments: true
wordpress_id: '44'
categories:
- agile
- kanban
- lean
- Redmine
---

In my [earlier post][1] about duct tape and technical debt I mentioned how managing technical debt affects the long term progress of software development.  I want to elaborate a bit more on that using something a presentation given by [Phillipe Kruchten][2] for the local Agile community group.  While I missed it,  Adam went and started to use these ideas for our own backlog.  I have found it a very useful way to think about the backlog and measuring productivity.

<!--more-->

One of the reasons for using a backlog is to to measure the progress of software projects, however, not all work in the backlog constitutes real progress towards the project goals.  Phillipe divides the backlog into four categories: Features, Defects, Architecture and Technical Debt.  I have illustrated this below:

![Colored_Backlog][3]

The four sections represent visibility towards the left and value towards the top.  Implementing features and fixing defects are things that are visible to the customer/user, while handling technical debit and building architecture and infrastructure are not.  Features and architecture add value while defects and technical debt do not.  This illustration reminded me of the time management quadrants from Stephen Covey's "7-habits" book.  Whatever you may think of the cheesy management speak sprinkled throughout the 7-habits book, or of people who abusively use terms from it like "paradigm" and "win-win", the book still had some useful (if somewhat obvious) ways of understanding time management.  The time management quadrants divided time usage into urgent vs. not-urgent and important vs. not-important.

![ColoredVelocity][5]

I sort of see the colors approach to the backlog as a similar in concept.  You are want to focus your time on work that adds value, while minimizing the time spent on work that doesn't.  However, that doesn't mean you can ignore defects and technical debt, it just encourages working in a way that will minimize it.  Features come first of course (or as Stephen Covey put it "first things first") and in an effort to deliver features early and often technical debt must be incurred. 

Adam broke down the causes of technical debt into two things: 1. a lack of knowledge which is typical early on in a project and 2. "Tactical shortcuts" knowingly incurring technical debt to meet deadline or if a "duct tape" solution will be sufficient for the situation.  Over the life of the project however these types of decisions will begin lead to defects and a the defects will cut into productive work on the project.  The idea is that time spent managing this technical debt can help to cut down on the defects later on.

In the diagram on the right we see potential phases of an agile project.  Early on there is no technical debt or defects but a lot of time must be spent building architecture and infrastructure to support feature development.  Afterwards with less architecture to build early feature development takes off but we begin to see defects eating into time spent on features.  Next we see the result of neglecting technical debt and architecture as the defect rate increases.  Finally, as technical debt is managed and time is spent building and improving architecture we can manage the defect rate, but this also cuts into feature development

Obviously these are just crude examples, your actual mileage may vary.  The point is to be aware of the long term negative effect not managing defects and technical debt has on productivity.  Of course, it certainly seems obvious, but when you read posts like the [duct tape programmer][7], it is clear there are still people who don't realize just how much "tactical shortcuts" can cost you over the lifetime of a software project.

I have made a small hack to Eric Davis' Kanban plugin for Redmine that allows us to have coloured items in our backlog just like here.  I am hoping to develop it out a bit more but for now you can look at my [feature request][8] for a patch that hardcodes the colors to the tracker names "bug", "feature", "architecture" and "technical debt".  Here is a [screenshot][9] of it in action.

   [1]: http://lucisferre.net/2009/09/28/the-technical-debt-of-red-green/
   [2]: http://philippe.kruchten.com/talks/
   [3]: /images/Colored_Backlog_thumb.png (Colored_Backlog)
   [4]: /images/Colored_Backlog.png
   [5]: /images/ColoredVelocity_thumb.png (ColoredVelocity)
   [6]: /images/ColoredVelocity.png
   [7]: http://www.joelonsoftware.com/items/2009/09/23.html
   [8]: https://projects.littlestreamsoftware.com/issues/3118
   [9]: https://projects.littlestreamsoftware.com/attachments/590/redmine_kanban_colors.jpg

