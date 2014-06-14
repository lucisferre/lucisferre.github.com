---
layout: post
title: "In which I do a partial about face on CQRS"
date: 2012-01-30 8:00
comments: true
categories: [CQRS, C#, rant]
---

I've spent nearly two years now exploring and applying both CQRS and event
sourcing. I've applied it in two projects though with minimal success. In the
first project the team voted to remove the CQRS and return to the N-tier
architecture shortly after I left. While they agreed with the ideas
conceptually, and could see value in the design, in the end it wasn't clear
enough how it all worked and people didn't feel comfortable enough to continue
supporting it.

In the second case I opted to remove the event sourcing components myself,
though I am keeping some of the messaging architecture by way of [TinyMessenger][tiny].
I feel the messaging provides some decoupling benefits that are critically
important, particularly when developing in C#.

I want to emphasize, I clearly see the value of both CQRS and ES, but in
applying them both I've also discovered a number of irritations and pains. I
want to outline my criticisms here in as constructive a way as I can for those
in the community to consider and hopefully either learn from or respond to. I
feel this is particularly important now in light of the decision by P&P to
explore and create guidance around CQRS, which will inevitably create greater
awareness and usage of it.

<!--more-->
###Event sourcing considered dangerous

CQRS/ES is most effective when used to develop domain models with rich
behaviour. Those that encompass clear and critical business operations.
However I've found many models don't have this or simply don't need it. In
other cases the business values and domain model is not clearly expressed or
understood yet, which is one the reasons we prototype early and iterate often.
I feel that in both of these cases represent situations where it is more
trouble than it is worth to apply event sourcing.

My latest project involved a product catalogue. Now there is clear business
value in a product catalogue, even though the primary operations focus solely
on CRUDdy operations, the creating and updating product details. Perhaps down
the road there may be more complex interactions to model but to begin with the
*most* important initial functionality is absolutely data-centric and CRUDdy.

In these scenarios it may seems sensible to start with commands and events
like `ChangeProductModel`, `ProductModelChanged`, `ChangeProductManufacturer`,
`ProductManufacturerChanged`, etc., which I did. However, it became clear to me
early on this was a flawed and very tedious model to work with and it took some
time before I was able to clearly understand how to craft a more elegant
solution.

The solution to this became clear to me as I considered events that
represented changes in the image and video assets being related to the
products. Obviously these events did not contain the *data* of the image but
simply contained a reference to some blob storage. It then occurred to me to
do the same thing with almost all data. Initially resulted in a hybrid
model idea that combined CQRS with the temporal-object CQRS that Udi Dahan
[suggested][udi].

I refactored down to only three events: `ProductCreated`,
`ProductRevisionPublished` and `ProductRetired` (not deleted) to encompass all
of the possible state transitions. Instead of including the data details of
these changes, these events simply referenced a document by Id which contained
the revision data. In this way the data structure of a product could change
without the model or events needing to change. Completely OCP.

The lesson I learned here was that events, should only describe real business
events, and not be dependant on simple data values. A customer becoming
preferred is probably a significant business event, the fact that someone
corrected the spelling of the customer's name in the database is probably not.
The means events should generally be representable by only the event name, and
references to aggregates or global data resources **by ID only**. As a trivial
example:

```csharp
// Good event
public class BlogPostPublished {
	public int PostId;
	public int RevisionId;
	public DateTime PublishedOn;
}

// Bad event
public class BlogPostPublished {
	public int PostId;
	public int RevisionId;
	public string Title;
	public string Content;
	public string[] Tags;
	...
	public DateTime PublishedOn;
}
```

Despite this epiphany and potential solution, I still decided that event
sourcing was largely unnecessary here. There is, in my opinion, limited
benefit to storing the `ProductRevisionPublished` events when you are using
the "temporal object pattern" approach and keeping all the past revisions. Udi
mentions as much in [his post][udi]. Not that there isn't any benefit to
publishing these events, but ES also brings with it a trade-off in
architectural complexity and an additional abstraction that will have to be
understood and maintained by developers who come later.

With this in mind, and considering the difficulties many unfamiliar with the
technique have, I think event sourcing could be considered dangerous without
at least some clear business justifications and a well understood domain. I
feel that as a community we could be making these dangers clearer to allow
people to more effectively determine how and when to apply ES in their
projects. This can be accomplished simply by providing guidance on how CQRS
can be applied without ES but with the flexibility to integrate ES later on.

This is guidance I recommend the P&P group try to provide as part of their work.

###Abstracting our abstractions?

Now I come to what is my biggest concern with the way we are typically
implementing our CQRS/ES solutions.

A typical CQRS command execution goes: 

`CommandMessage -> CommandBus -> CommandHandler -> AggregateRoot -> EventMessages -> EventHandlers`

If this all seems a bit of redundant it probably should. If not, look at one
of the original definitions of an object from Smalltalk, the original OOP
language. An object can:

1. Hold state (references to other objects).
2. Receive a message from itself or another object.
3. In the course of processing a message, send messages to itself or another object.

{% img right /images/yodawgcqrs.jpg 400 Yo dawg I heard you like CQRS %}

Funny, this sounds exactly like what we are creating an abstraction of with CQRS.
So, if I understand this correctly, we are creating an abstraction of OOP
on top of our OOP..? To paraphrase another [WAT?][wat].

{% blockquote Alan Kay http://c2.com/cgi/wiki?AlanKaysDefinitionOfObjectOriented Alan Kay's definition of OOP %}
OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late binding of all things.
{% endblockquote %}

Alan Kay's opinion seems to be that what we are accomplishing with messaging
in our CQRS should be a feature of the language, not something we should need
to abstract on top of it. 

Now, I'm arguably a novice when it comes to dynamic languages, but it still
seems to me that dynamic languages are better able to handle extensibility,
open-closed principle and decoupling without needing all this messaging
ceremony and thus manage to avoid a lot of this problem entirely.

{% blockquote Alan Kay http://c2.com/cgi/wiki?AlanKaysDefinitionOfObjectOriented Alan Kay's definition of OOP %}
Until real software engineering is developed, the next best practice is to develop with a dynamic system that has extreme late binding in all aspects.
{% endblockquote %}

I want to make it very clear I'm not suggesting CQRS isn't correct in
principle, or that event sourcing isn't a highly valuable pattern when used
correctly for appropriate scenarios. In fact, I can say from experience that
any developer will find it well worth their time to learn both. Just that in
the C# (or Java) world we also seem rely on messaging oriented CQRS
implementations to solve a limitation of the language as well. In doing so we
add at least some amount of forced ceremony, redundancy and verbosity as well
as creating a somewhat leaky abstraction, one which, as I've pointed out, many
developers seem to find confusing.

Internally, the language should be flexible enough for us to develop in a clean,
extensible and decoupled fashion without resorting to messages and message
buses and C# just doesn't seem to cut it in this respect.

###In closing

As I move on from .NET and statically typed languages (more on this in my next
blog post) I expect I will probably not be using "capital-CQRS" as it has
typically been implemented and described in C# and Java. However, I still fully
agree with the principles and they are bound to continue to influence the way I
design software.

I hope the community will take what I'm saying with a grain of salt. I'm no
expert. However I think my concerns and criticisms have been echoed before by
many other who have worked to learn and apply CQRS to real-world problems.
As the CQRS community works with Microsoft on creating more awareness around
CQRS, it will do well to take these things into consideration and provide
guidance that helps newcomers to avoid these typical CQRS pitfalls.

As for the language limitations, well perhaps if P&P can provide it's guidance
in F# or IronRuby as well as C# we will all understand the problem a bit
better.

[tiny]:https://github.com/grumpydev/TinyMessenger
[udi]:http://www.udidahan.com/2011/10/02/why-you-should-be-using-cqrs-almost-everywhere%E2%80%A6/
[wat]:https://www.destroyallsoftware.com/talks/wat