---
author: Chris Nicola
date: '2009-10-16 18:59:00'
layout: post
slug: integration-testing-with-a-service-bus
status: publish
title: Integration Testing with a Service Bus
comments: true
wordpress_id: '46'
categories:
- .NET
- TDD
- SOA
---

Using a service bus and messaging architecture allows for all sorts of good things with service oriented architectures, but one use I found for them recently was integration testing by using mocking framework with my messaging architecture.  I was able to do this by simply by getting a mock of the `IConsumer<T>` and subscribing it to the bus. 

<!--more-->

You can see an example of such a test here:
    
```csharp
[Test]
public void CanLoadCashTransactionsFromISMFileAndSaveToDatabase() {
  var transactionCounter = MockRepository.GenerateMock<IConsumer<TransactionSuccessful>>();
  transactionCounter.Expect(m => m.Consume(Arg<TransactionSuccessful>.Is.Anything)).IgnoreArguments().Repeat.Times(6);
  var sink = IoC.Resolve<ITransactionService>();
  var source = IoC.Resolve<ITransactionDataReader>();
  IoC.Resolve<IMessageBroker>()
    .Subscribe(Subscription.For(sink).Sync())
    .Subscribe(Subscription.For(transactionCounter).Sync());
  source.ProcessDataFromStream(GetTransactionData());
  transactionCounter.VerifyAllExpectations();
}
```

I am using Rhino Mocks here along with the AAA syntax for this test. 

I have a transaction source service sending `TransactionMessage` which are received by a transaction sink service which processes the transactions.  The sink sends a `TransactionSuccessful` message if the transaction is processed correctly.  A mock `IConsumer<TransactionSuccessful>` can be subscribed to the service bus to verify any expectations I have of the result messages.  I can also create other mocks if I expect other results:
    
```csharp
[Test]
public void CanOnlyAddTransactionsOneTime() {
  var transactionCounter = MockRepository.GenerateMock<IConsumer<TransactionSuccessful>>();
  transactionCounter.Expect(m => m.Consume(Arg<TransactionSuccessful>.Is.Anything)).IgnoreArguments().Repeat.Times(6);
  var duplicateCounter = MockRepository.GenerateMock<IConsumer<TransactionExists>>();
  duplicateCounter.Expect(m => m.Consume(Arg<TransactionExists>.Is.Anything)).IgnoreArguments().Repeat.Times(6);
  var sink = IoC.Resolve<TransactionService>();
  var source = IoC.Resolve<IISMTransactionService>();
  IoC.Resolve<IMessageBroker>()
    .Subscribe(Subscription.For(sink).Sync())
    .Subscribe(Subscription.For(transactionCounter).Sync())
    .Subscribe(Subscription.For(duplicateCounter).Sync());
  source.ProcessDataFromStream(GetTransactionData());
  source.ProcessDataFromStream(GetTransactionData());
  transactionCounter.VerifyAllExpectations();
  duplicateCounter.VerifyAllExpectations();
}
```

Here I am expecting that when duplicate transactions are received that a TransactionExists result message is sent along the bus and I can verify this using a second mock.

Monitoring the messages on the bus seems to be a very simple and clean way to verify expectations along the integration points between services.

Note: I should point out that currently Rhino Mocks does not check if something happens more than `Times(int number)` only that it isn't called fewer times.  Perhaps something like `.Exactly.Times(int number)` will be added to Rhino in the future
