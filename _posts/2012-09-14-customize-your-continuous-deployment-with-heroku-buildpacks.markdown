---
layout: post
title: "Customize your continuous deployment with Heroku buildpacks"
date: 2012-09-14 12:14
comments: true
categories: heroku, ruby, rails
---

One of the great things about building on PaaS/IaaS is all the devops work you
get to avoid. One of the drawbacks is there is a different class of devops work
that come with these services. That of understanding how to manage and use them
intelligently. Overall the tradeoff is generally a win but it is good to know
all the ways in which you can customize.

Heroku is actually quite customizable via their buildpacks, which are basically
the bootstrapping scripts they run when you push code up to them.

This will be the first in a set of posts I'm going to be doing describing
building software (read: startups) exclusively using hosted services for
operations. I'm not suggesting that this is for everyone, merely describing some
of the things I've been doing and how it's been working out, for good or bad.

<!--more-->

## Getting started with a buildpack

I would recommend starting your custom buildpack from an existing template. For
that Heroku has provided all of their default buildpacks for you to use as a
base on [Github](http://github.com/heroku). If you're using Rails you'll want
the Ruby buildpack and will likely be editing the `lib/language_pack/rails3.rb`
or `lib/language_pack/rails2.rb`.

## What can you do?

That's really up to you. For me I had two problems with Heroku's default build.
Both relating to the asset pipeline.

1. The assets are recompiled even if there have been no changes to said assets.
2. The asset compilation can fail and it will still deploy the site.

The first problem just means that builds may take 2-3 minutes longer than they
have to. The second problem is far worse since Heroku just lets rails fallback
to on-the-fly compilation of assets, however since we failed to compile them
one can assume this will fail in production. This will almost always result in
500's in this situation, so it is just very, very bad.

So I searched the active forks of the default buildpack and found an already written solution for #1 [here](https://github.com/nthj/heroku-buildpack-ruby/commit/889d9f98b229dd999567144a4ce9e63f09a0ae47).

For the second problem, I just changed the default behavior to exit with an error code if the asset compilation fails. You can see [the commit here](https://github.com/TicTalking/heroku-buildpack-ruby/commit/3c63ca391da1faaeab3f88b1cb21929ce4e8dad8).

## Finishing Up

To get your build pack working you can you simple need to tell Heroku to use it:

`heroku config:add BUILDPACK_URL=https://github.com/heroku/heroku-buildpack-ruby`

Or if you are starting a new app:

`heroku create myapp --buildpack https://github.com/heroku/heroku-buildpack-ruby`

Check out Heroku's [own documentation here](https://devcenter.heroku.com/articles/buildpacks)

Obviously this is tip of the iceberg stuff, but it goes to show you how much
flexibility you really have on Heroku. There is a downside though, Heroku
doesn't handle failures here all that well, so if it fails to pull the
buildpack from Github (which happens more often than I'd like) it just gives
up, it has no retry policy on this. So you have a bit of a reliability issue.
For me it's ok as I'd rather have to re-run a deployment than have a deployment
that will almost certainly fail. However I'm also thinking of submitting a pull
request with these changes (subject to configuration variables switches).
