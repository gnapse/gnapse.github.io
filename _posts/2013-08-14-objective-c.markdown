---
layout: post
title: "Objective-C"
tagline: "my first impressions"
date: 2013-08-14 09:34
tags: [iOS]
categories: programming
---

[Objective-C][] is a relatively unknown programming language.  Only recently,
with the iPhone apps programming boom, it has managed to become more popular.
The name itself induces some curiosity, subtly suggesting that there's some
Object Oriented Programming involved, with some C here and there.  A clever
pun, intended.

But ingenuity is to be found also beyond the name.  The language itself is a
mixture of [Smalltalk][]'s dynamism and the immense power and simplicity of
[C][], two of the most influential programming languages in the history of
computing, and that at first sight appear to be incompatible, but somehow
Objective-C manages to take the best of both worlds, with great success.

After a brief hobby incursion into the world of iPhone apps programming, I can
say that this language surpassed my expectations.  In this article I pretend to
give a few reasons about why I think this way.

[Objective-C]: http://en.wikipedia.org/wiki/Objective-C
[Smalltalk]: http://en.wikipedia.org/wiki/Smalltalk
[C]: http://en.wikipedia.org/wiki/C_(programming_language)

## Dynamism

Although it might not appear so after a first look at it, Objective-C is a
highly dynamic language.

For starters, this language bases most of his OOP philosophy on the concept of
[message passing][] between objects.  There's a subtle but important difference
between this approach and the classic concept of method invocation.  Granted,
the difference may appear more philosophical than practical, but it ends up
changing the mindset of programmers on either sides.

The important thing here is that this feature provides a level of dynamism not
common in compiled (non-interpreted) languages, and allows the use of more
liberal varieties of polymorphism, such as the well known [Duck typing][].  In
contrast, languages like C++, or even some more dynamic, like Java and C#, are
more restrictive, only allowing this kind of dynamic method binding within the
same inheritance chain.

Other features that contribute to the dynamism of Objective-C are:

- [Classes are objects too][], instances of the meta-type `Class`.  This is a
  feature borrowed directly from Smalltalk, and also shared with other modern
  languages like Ruby.  It opens a whole series of possibilities within the
  realm of meta-programming, such as dynamically creating new methods and
  classes, at runtime.
- [Type introspection][] and [Reflection][], which stems from the previous
  point.
- [Code blocks][], a feature similar to that of Ruby, allowing to use chunks of
  executable code as data, values that can be passed around, kept in variables,
  etc.  This is a relatively new feature, which becomes evident in the language
  runtime libraries, that make little use of it, as they were not designed with
  this feature in mind.

[message passing]: http://en.wikipedia.org/wiki/Message_passing
[Duck typing]: http://en.wikipedia.org/wiki/Duck_typing
[Classes are objects too]: http://www.cocoawithlove.com/2010/01/what-is-meta-class-in-objective-c.html
[Type introspection]: http://en.wikipedia.org/wiki/Type_introspection#Objective-C
[Reflection]: http://en.wikipedia.org/wiki/Reflection_(computer_programming)#Objective-C
[Code blocks]: http://goo.gl/YgdDHl

## Declared properties

Objective-C provides a simple mechanism for declaring the [properties][] or
attributes that define and describe on object's state, while at the same time
provides automatic implementation of the getters and setters that control
access to these attributes.  In essence, all instance variables are private to
the object, and their values can only be exposed via the object's properties
and methods.

A property declaration includes a series of characteristics that influence the
behavior and implementation of the generated getters and setters.  These
include, for instance, controlling if assignment to the property works by copy
or by reference, if the property is read-only, or thread-safe, etc.

```objc
@interface XYPerson : NSObject {
  NSString *name;  // private instance variable
}

// A property exposing the instance variable
@property (copy) NSString *name;

// ...
NSString *name = somePerson.name;
somePerson.name = @"John Doe";
```

This language feature is aimed directly at embracing the [encapsulation][]
principle, one of the cornerstones of Object Oriented Programming, which states
that every object must control the way in which other objects access its
internal state.

Properties also play a big role in some of the core features of the runtime
library, such as [KVC][], [KVO][], etc.

[properties]: http://en.wikipedia.org/wiki/Objective-C#Properties
[encapsulation]: http://en.wikipedia.org/wiki/Encapsulation_(object-oriented_programming)
[KVC]: http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/KeyValueCoding/
[KVO]: http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/KeyValueObserving/

## Protocols

A [protocol][] defines a contract that classes can conform to.  They consist of
a set of methods that classes adhering to the protocol should implement.  The
protocol defines no implementation at all, only the signatures of the methods
expected to be implemented by the classes conforming to the protocol.

```objc
@protocol XYEnumerator
- (bool)hasNext;
- (id)next;
@end

// A class conforming to the above protocol
@interface XYArrayEnumerator : NSObject <XYEnumerator>
// ...
@end

// Class implementation must include the methods of the protocol
@implementation XYArrayEnumerator
- (bool) hasNext { /* ... */ }
- (id) next { /* ... */ }
@end
```

If you're familiar with Java or C#, you should have realized by now that
protocols are equivalent to the concept of interfaces in these two languages.
In fact, [Objective-C protocols inspired Java's interfaces][], which in turn
influenced the C# language several years later.

[protocol]: http://goo.gl/tBIWN
[Objective-C protocols inspired Java's interfaces]: http://www.virtualschool.edu/objectivec/influenceOnJava.html

## Categories

[Categories][] allows you to re-open a class definition, and add new methods or
re-define existing ones.  This can even be done with classes from the runtime
library, or classes with no source code available.  A feature like this is only
possible when a language's runtime is highly dynamic, so this reinforces the
case made before, about the dynamic nature of Objective-C.

OOP purists tend to be against including this kind of features in a language
(see [SOLID][] and [Open/Closed Principle][]), but as an enthusiast of dynamic
languages, I tend to think that the advantages can be greater than the
potential problems that could arise, if the feature is used wisely.  Granted,
**it is a controversial feature, and it can be easily abused**.  Generally,
when this happens, it is sign that the system of classes being worked on is not
well designed.

However, if used wisely and not more than necessary, it can become the key of
an elegant and concise design.  A living proof is Ruby on Rails, a library that
would not exist in its present form if it were not for the fact that the Ruby
language has this feature too.

A more mundane but valid use for categories is to split the implementation of a
class into separate source files, be it for organizational purposes, or because
the class interface and implementation are too extensive, and splitting it up
into logical chunks, makes it easier to understand and maintain.

[Categories]: http://goo.gl/l3fPv
[SOLID]: http://en.wikipedia.org/wiki/SOLID
[Open/Closed Principle]: http://en.wikipedia.org/wiki/Open/closed_principle

## Compatibility with C

Unlike C++, Objective-C is a proper superset of C, meaning that every valid C
program or file is also valid Objective-C code.  There's no guideline of what
to do and what not do when using C code in Objective-C.  The most evident
advantage is that Objective-C programs can use any existing C library, without
the need for any modifications or wrappers.

This is where the power of the Smalltalk/C combination shines.  You get a
language that is almost as dynamic as it gets, combined with another language
commonly used to develop powerful low-level stuff, like operating system
kernels, database engines, and many other building blocks of modern computing
systems.  You get the best of two worlds.

## Conclusions

Objective-C, as any other language, is far from being perfect, and it certainly
has some _weirdness_, like its syntax in some cases.  Also, the object
initialization process is a bit too complex and error-prone for my taste.

My perception of it has changed over time though.  When I first started to use
it, it appeared to be cumbersome and bizarre.  But sooner rather than later you
start to appreciate its qualities, accept its limitations, the unconventional
syntax, and eventually you end up enjoying working with it, specially if you
have a background working with modern dynamic languages.
