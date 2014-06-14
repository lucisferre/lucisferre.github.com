---
author: Chris Nicola
date: '2009-07-06 19:42:00'
layout: post
slug: dont-repeat-yourself
status: publish
title: Don't repeat yourself
comments: true
wordpress_id: '24'
categories:
- clean code
- DRY
---

As one of the many things on my list of "stuff to read" from DevTeach is [Clean Code][1] by Robert C. Martin. It is an outstanding book that really nails the concept of writing clean, effective and efficient code.  While it is in Java the principles apply to just about any language, particularly C# because the syntax, like Java, is very C/C++-like.

I am finding it invaluable so far, however today I read one example today that just looked awkward to me.  Martin starts by factoring some really ugly code into something much more elegant which you see below:

<!--more-->

```java
public void delete(Page page)
{
  try
  {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
  }
  catch (Exception ex)
  {
    logger.log(ex.Message);
  }
}
```

That looks pretty good to me, in fact I have many functions that look like that. Then he goes on to say that try/catch blocks are actually ugly so we should extract the bodies into functions of their own. The result looks like this:

```java
public void delete(Page page)
{
  try
    deletePageAndAllReferences(page);
  catch (Exception ex)
    logError(ex);
}
```

```java
public void deletePageAndAllReferences(Page page)
{
  deletePage(page);
  registry.deleteReference(page.name);
  configKeys.deleteKey(page.name.makeKey());
}
```

```java
public void logError(Exception ex)
{
  logger.log(ex.Message)
}
```

I will admit that it does probably make sense to factor out the try block here and it looks fine to me, but the function `logError(e)` seems like redundancy to me. It seems to violate the principle espoused on the very next page of the book _"Don't Repeat Yourself"_.

"Don't repeat yourself", as he defines it there, really refers to repeating code, but I have heard many developers use it as a reason to avoid other things, such as redundant comments (if your code clearly explains what it is doing, which it should, why do you have a comment explaining it again?). To me this is a redundant method. `logger.log(ex.Message)` says exactly the same thing to me as `logError(ex)`.

I am probably nit-picking though.  Realistically, the author probably expects you will have more complex error handling than just `logError(ex)`, so you will have a class that provides error handling methods including logging and it will likely include a `logError()` function, or perhaps a `notifyUserAndLogError()` which notifies the user through the UI and logs it.  Regardless, taken as-written the refactoring of `logger.log()` feels unneeded to me.

   [1]: http://www.amazon.ca/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882

