---
title: Javascript MVC - The paradox of choice
date: 2011-10-02 10:49:05 Z
categories:
- javascript
- MVC
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '675'
---

As I continue to chug along on my Rails project I'm faced with a dilemma that every web developer must face at one point or another. Which javascript "rich-client" framework am I going to use? At work we have been using [backbone.js][1] and while I like it, I have a few, small issues with it.

<!--more-->

For one, backbone doesn't really have any sort of built in data-binding concept, or observer pattern out-of-the-box. This means it isn't actually an MVC framework at all. That's not necessarily a bad thing, but right now I'm focused on RAD and I really want something that will do as much work as possible for me, so that includes data binding. Though it's worth Derrick Bailey actually has created a [data-binding add-on for backbone][2]. Despite that there are a few other things I don't love about backbone, one in particular is how much jQuery templates are needed. I'd really love to avoid the use of templates on the client side if I can.

[Sproutcore][3] and [ExtJS][4] both seem designed to be a complete out-of-the-box rich client SDK, however here I feel both of them are almost _too_ much, ExtJS doubly-so, it reminds me of the "rich-client" WinForms and WPF packages from companies like Telerik. Here, my gut tells me I should stick with simplicity and standard HTML/HTML5 over a lot of drop in widgets. I've been bitten by complex UI frameworks before, hard to configure, hard to debug, etc. etc. So I'm shying away from these too.

This leaves me with two potential contenders, both of which look decent to me: [knockout.js][5] and [batman.js][6] (BIFF! POW! ZAM!). Batman is a relative newcomer, but is heavily in favor of convention over configuration, which is a personal preference of mine too. Knockout is also fairly clean looking and both look easy enough to pickup in a few hours. What I like most about both is there is no need for templates. I can generate a static HTML page with data from Rails initially and knockout/batman are simply there to provide a rich client experience on top of this. This makes progressive enhancement much easier (though PE is certainly an overrated feature in rich web "apps", and more important when talking about public facing "documents" where SEO is important).

Nonetheless I still have a dilemma. Do I use the shiny and new batman or the venerable knockout? Personally I'm leaning towards knockout for one reason only, knockout has more complete documentation and a more obvious way to integrate it with Rails apps. The batman guide starts with node and NPM and doesn't explain at all how you integrate with an existing site (yes I'm sure it's fairly trivial, that doesn't change the fact that when I read a "getting started" guide I actually want to get started and not setup a demo app).

So that's my stream of thought for today. I'd love to here from anyone who has more experience than me with these frameworks. Which ones have to tried, which did you prefer and why?

   [1]: http://documentcloud.github.com/backbone/
   [2]: http://lostechies.com/derickbailey/2011/07/24/awesome-model-binding-for-backbone-js/
   [3]: http://www.sproutcore.com/
   [4]: http://www.sencha.com/products/extjs/
   [5]: http://knockoutjs.com/
   [6]: http://batmanjs.org/

