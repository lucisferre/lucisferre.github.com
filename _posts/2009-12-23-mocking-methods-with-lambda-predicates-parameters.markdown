---
title: Mocking methods with lambda predicates parameters
date: 2009-12-23 12:55:00 Z
categories:
- ".NET"
- Software Development
- csharp
- linq
- mocking
- TDD
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '57'
---

![lambdareflection][1]

I often work with lambda’s, as I’ve mentioned before I am a bit of a LINQ junkie.  As a result I have started creating methods that takes predicates as parameters, however I was finding methods like this difficult to mock.  It took a bit of doing but I finally managed to figure this out.

<!--more-->

I have such methods defined in an interface like this: 

```csharp
public interface ILinqRepository<T> { 
  void Delete(T target); 
  T FindOne(int id); 
  T FindOne(Expression<Func<T, bool>> predicate); 
  IQueryable<T> FindAll(); 
  IQueryable<T> FindAll(Expression<Func<T, bool>> predicate); 
  void Save(T entity); 
  void SaveAndEvict(T entity); 
} 
```

Now say I have a test for finding a particular entity from my repository

```csharp
[Test] 
public void CanGetSecurityByISMNumber() { 
  _securityRepository.Stub(m => m.FindOne(null)).IgnoreArguments().Return(TestData.Security); 
  Assert.That(_tasks.GetSecurityByISMNumber(TestData.Security.IBMSecurityNumber) == TestData.Security); 
} 
```

A pretty simple test where I am using some static `TestData` with the mock.  However the problem I have with this test is that this test is not driving the design I want (TDD right), I am not actually testing what I want to test.  I could implement `GetSecurityByISMNumber` to call `FindOne` with any predicate value and make this pass.

Instead what I can do is use RhinoMocks `Callback` feature like this:

```csharp
[Test] 
public void CanGetSecurityByISMNumber() { 
  _securityRepository.Stub(m => m.FindOne(null)) 
    .IgnoreArguments() 
    .Callback<Expression<Func<Security, bool>>>(p => p.Compile().Invoke(TestData.Security)) 
    .Return(TestData.Security); 
  Assert.That(_tasks.GetSecurityByISMNumber(TestData.Security.IBMSecurityNumber) == TestData.Security); 
} 
```

Perhaps a tad bit messy but I think it communicates my design intent clearly.  I could also factor out the lambda there into it’s own function like _QueryIsValid_

```csharp
private static bool QueryIsValid(Expression<Func<Security,bool>> predicate) { 
  return predicate.Compile().Invoke(TestData.Security); 
} 
```

I should also note that if I wasn’t using `Expression` here I could skip the call to `Compile` which would make it a bit more readable.

   [1]: /images/lambdareflection1.png (lambdareflection)

