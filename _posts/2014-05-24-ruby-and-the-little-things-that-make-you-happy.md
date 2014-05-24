---
layout: post
title: "Ruby, and the little things that make you happy"
date: 2014-05-24 14:58
tags: [ruby]
categories: programming
---

Ruby was [designed to make programmers happy][], and there are lots of little
things in it that contribute to achieving this goal.  What amazes me is that,
even after using it for years, one can still find some of these little things
here and there, that you did not know about.  Today was one of those days for
me.

[designed to make programmers happy]: http://quoty.me/quotes/209

It turns out that the syntax for specifying class inheritance in Ruby, bares
some resemblance to the syntax for testing wether two classes are related by
inheritance.  The following code snippet says it all:

```ruby
class Person < ActiveRecord::Base
end

if Person < ActiveRecord::Base
  puts "The class 'Person' is a subclass of ActiveRecord::Base"
else
  puts "The class 'Person' is NOT a subclass of ActiveRecord::Base"
end
```

Brilliant, isn't it?

It turns out you can use all four inequality operators, and you'll get the
expected result.  Given a class `A` with a subclass `B`, the following code
shows how it works:

```ruby
B < A    # true
B > A    # false
B <= A   # true
B >= A   # false
A >= A   # true
A > A    # false
```

This is the kind of little things that make me a happy programmer ;)
