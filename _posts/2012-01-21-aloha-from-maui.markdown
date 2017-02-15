---
title: Aloha from Maui
date: 2012-01-21 16:33:00 Z
categories:
- blogging
- octopress
- travel
layout: post
comments: true
---

![maui sunset][sunsetthumb]

You may think it's strange, but I actually like sitting here, by the ocean, writing blog posts while the sun goes down. It may not be for everyone but for me it's relaxing.

I spent a bit of time over the last couple of days converting my blog over to Octopress, the latest in blogging platforms for hipster software developers. It has everything a developer hipster would want: markdown editing, git-based publishing, rake tasks and of course beautiful typography and layout that will probably have said hipster developer turning down hipster designer job offers.

<!--more-->

But seriously, it's actually pretty great. I initially resisted Octopress, it was honestly far too tempting. I mean, a github based blogging platform, it felt like it was customed designed to suck me in, and I was certain I would regret any decision to leave reliable old Wordpress for something shiny and new. But alas, as I read more and more people's experience with it, it was clear to me, this was *too* much of a good thing and yet it was still *good*.

## What I love

I love writing my blog posts in markdown, but unfortunately the WP tools I could find like Vimpress and Ultrablog are still just too buggy and awkward to satisfy. Also they are completely VIm dependant so if I decide I want to use Sublime or Emacs at some point I'm hooped.

The syntax highlighting is fracking amazing. It works with inline code blocks, Github Gists and can even include code files. It's very versatile, looks great (I went with the Solarized light theme which I think looks awsome in blog posts) and is extremely easy to use and maintain (I did have to clean up a mess of incorrectly formatted and converted code snippets from other lesser tools).

I also love the fact that the site uses extremelyclean HTML5, CSS3 with SCSS and seems very easy to customize and extend (being a static site generator makes things extremely simple). Wordpress by comparison is a PHP nightmare, and just about every theme CSS file is so complicated and messy it looks like it was written about 10 years ago and possibly by a 12 year old hacker.

## What I like

I like the fact that it comes with a few really cool plugins that I can't even find for Wordpress. The Github and Pinboard sidbar plugins have been things I've wanted for some time now.

I also like disqus and since I was already using it, switching over should be pretty seemless (it doesn't appear to be working right now but I believe that is DNS updating).

Finally, overally I like the defaults. The style is a nice subdued HTML5 responsive layout (try resizing yoru browser). The default plugins work great.

## What I don't like

The style feels a bit generic, possibly because it is being used on just about every person's Octopress site out there. I'll definitely have to spend some time personalising it. Fortunately that looks fairly easy. I'm also not convinced about the serif fonts for body text. They do look quite readable but I've always been told not to use serif for body text displayed on computer screens. I'm curious what others think about the readability.

Conversion is also not terribly seemless. I used [exitwp][exitwp] which is a bit buggy (or uses buggy python modules) and I had to hack with it a bit. Even then several posts had no paragraphs whatsoever (which was random since most did). I had to fix a couple of things with a bash script as well, image links, author names, turning comments on. Pretty straightforward but this is definitely a developers blogging tool.

Also, while not really an Octopress issue, Github is a bit spammy with a notification each time it generates the pages, which can be a bit annoying. Need to see if I can turn that off. Github also doesn't support multiple DNS entries pointing to one pages site, so lucisferre.org is officially dead.

## How I did it

I'm not going to give the play-by-play. There are lots of good resources for people trying to convert. I will cover a couple of things I had to do though.

First I used [exitwp][exitwp] to convert from a WP export file. It seemed easier than the alteratives which required access to the WP database (in hindsight though they may be more reliable). This did not work well at all. Every link was inline markdown and was wrapped at 80 chars which broke any links that were wrapped. The fix is outlined in this [issue][exitwpissue]. 

Second I went and fixed all my code blocks. Many had been broken or just ugly for quite some time on Wordpress as I had no interest in fixing up HTML. In markdown this was tedious, but still a breeze. They look great. I'm glad I did it. I also had to fix the embedded Gists tags I was using, the WP plugin for this works differently than the Octopress one. Again pretty simple.

I had to run a bash script to replace the WP username with my name in the author fields of the posts (you could also just delete these, it falls back to the setting from `_config.yml`). Basically a find and sed works for this:

    find source/_posts -name "*.markdown" -exec sed -i '' 's/author\: chrisn/author: Chris Nicola/g' {} \;

I also used a similar command to replace links that pointed to files that the Wordpress site was hosting. Exitwp has a utiltiy to download these images, but it doesn't update the URLs (it can't really it doesn't know what they should be).

To enable comments on a post you need to add `comments: true` to it. This required adding a line. This little script will do that for you:

    find source/_posts -name "*.markdown" -exec sed -i '' '8i\
    comments: true
    ' {} \; 

The rest of it was really just following through the [Octopress documentation][octodoc] which is pretty straightforward. I setup the sidebars the comments and github pages as well as my DNS. All pretty easy aside from the time spend re-editing my posts (which arguably took longer because I couldn't help but tidy them up a bit while I was at it).

## Final thoughts

Sadly, this is the 4th blogging system I've used since I started almost 3 years ago, mostly because I was too stubborn to just use Wordpress from the start. I honestly think this one is a keeper. Even if something better comes along there is no way I'm switching unless it supports markdown and Jekyll statically generated pages, and I can just copy over my files. For me, Octopress is definitely a home run.

[sunsetthumb]:https://lh6.googleusercontent.com/-T1VavFTtTME/TxuGesuolXI/AAAAAAAAAfc/m_SRew83wXE/s800/IMGP5742.jpg
[exitwp]:https://github.com/thomasf/exitwp
[exitwpissue]:https://github.com/thomasf/exitwp/issues/6
[octodoc]:http://octopress.org/docs/
