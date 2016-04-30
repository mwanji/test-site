---
title: "Java Coders Distrust Their Neighbours"
layout: post
og_description: "A dysfunctional flow."
og_image: "2011/java-coders-distrust-neighbours/jtrust.jpg"
---

<img src="{{site.baseurl}}/img/2011/java-coders-distrust-neighbours/jtrust.jpg" width="550" height="367" title="Would you trust these people?"/>

Here's a dysfunctional flow I came across today at work and that seems to pop up fairly often.

1. Query some entities from the database
2. Extract the IDs and put them in an array
3. Pass the array to another method
4. Load each entity from the database, but by ID this time

Ludicrous. If we're lucky, the by-ID lookups will only hit the first-level cache, but those calls shouldn't even be happening at all. What is to blame?

__Stupidity?__ No, my colleagues aren't stupid at all.
__Large codebase?__ This is a fairly big application, but not *that* big.
__Laziness?__ In a sense. There is one case in which the ID array is passed in directly from the client without having been loaded on the server-side first. It could be that the author didn't want to write a second method.
__Paranoïa__? Probably. Java developers are prone to that. 

A lot of Java developers seem to resist thinking in overall flows in favour of treating every method as an island that can be attacked on all sides by anything at any time. So, each method is front-loaded with a bunch of `Assert.notNull(someValue)`, objects are copied "just to be sure no-one else is messing with them". The code just gets cruftier.

It seems to me that much of this is nonsense and could be alleviated:

* Apply strong validation at the beginning of any flow, then the strict minimum afterwards. The database should have the strongest constraints possible.
* Enforce the convention that methods neither accept nor return null values.
* In the single-threaded context of, say, a typical web request, there's no mystery about who is manipulating your objects. Don't worry about it.

These don't always apply, but are pretty good rules of thumb.
