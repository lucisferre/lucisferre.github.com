---
layout: post
title: "Making promises with AngularJS"
date: "Sun Jul 06 20:56:46 -0700 2014"
categories: AngularJS
---

I recently had a need to wait on multiple resource queries in an AngularJS
controller and then do something with the returned data only after _all of the
data_ had been returned. A naive implementation of this might look something
like this.

```coffeescript
User = $resource '/user/:id/', { id: '@id' }
Accounts = $resource '/accounts/:id', { id: '@id' }
@user = User.get { id: $routeParams.id }, (user) =>
  @account = Accounts.query { userId: $routeParams.id }, (accounts) =>
    # Do work after user and accounts have loaded here.
```

_Note: The above is based ont the "controller as" approach but you can
substitute `$scope` just as well_

This is both ugly from a nesting perspective and it means that we are waiting
on one request before we make another when our browser is perfectly capable of
making both requests simultaneously.

AngularJS includes a promises service called `$q` which is actually used
underneath the hood in a lot of its built-in services like `$http` and
`$resource`. However, it often gets overlooked when actually building things.
One of the things you can do is use it to create a promise of promises.
Something like this:

```coffeescript
User = $resource '/user/:id/', { id: '@id' }
Accounts = $resource '/accounts/:id', { id: '@id' }
@user = User.get({ id: $routeParams.id }).$promise
@accounts: Accounts.query({ userId: $routeParams.id }).$promise
waitUntilLoaded = $q.all
  user: @user
  accounts: @accounts
.then (results) =>
  # Do work with results.user and results.accounts here.
```

_Note: `$q.all` can take an object or array, it will be passed to the `then`
callback with the values resolved by the promises in place of the promises
themselves._

Defining our own "promise of promises" has the added benefit of being
re-usable. The `then` callback will execute immediately assuming the promise
has been resolved, otherwise it will wait. This is particularly handy if we
have a `$watch` in our controller that shouldn't execute until after the data
is loaded.

```coffeescript
$scope.$watch 'someUIValue', (value) =>
  waitUntilLoaded.then =>
  # Now we can ensure this code will only # execute after
  # the waitUntilLoaded promises have been resolved.
```

This is a much better design than the alternative of setting up the `$watch`
inside of the `then` because this way if we end up with some sort of "race
condition" where the watch gets fired slightly before the promises are resolved
we will still ensure that we fire the watch as soon as the appropriate data is
loaded.

### What about router resolves?

There is an alternative approach to making controllers wait on promises which
is using the [Angular router's resolves][resolves]. However, I've found this
interface clunky and don't like the idea of dealing with these sorts of
dependencies inside my routing table. Some of the patterns of dealing with this
that have been suggested are quite unsatisfactory and seem highly dependent on
how you organize your code and define your classes.

I'd like to use resolves but I don't see why it should be dependent on the
routing instead of just a dependency of the controller. I'm considering some
alternative patterns but have not yet finalized anything that works yet. I'll
definitely follow up on this post if and when I do. For now, using `$q`
satisfies my needs until I find a way to use resolves that I like.

[resolves]: https://egghead.io/lessons/angularjs-resolve
