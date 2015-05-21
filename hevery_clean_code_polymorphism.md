
Title: "The Clean Code Talks -- Inheritance, Polymorphism, & Testing"
Author: Miško Hevery
URL: https://youtu.be/4F72VULWFvc
Length: 38:24
Conference: Google Tech Talks
Date: 20.11.2008

Intro
-----
When moving from procedural to OOP languages, we can get rid of quite a
few `if` statements (not all of them) by replacing them with polymorphism.

Code without `if`:
* easier to read
* easier to test
* polymorphic systems are easier to maintain and to extend

Use polymorphism when:
* the object behaves differently based on state (the method is dispatched
  at runtime)
* you have to check same thing in different places (happens with flags)

Use `if` if doing comparison of primitives (==, <, >) - in this case it's
impossible to get rid of `if`s.

To make code `if` free, don't return `null`, because you can't dispatch
on `null`s. That will force you to check if the returned value is a null
and then dispatch. Also don't return error codes, just throw runtime
exceptions.

For polymorphism we use inheritance.

Note: don't overuse inheritance - the hierarchy can become unmanageable.

State based behavior
--------------------
Replace conditionals with polymorphism. Don't check some object attribute
or flag to decide on the behavior, better move those different behavior
to different classes and then make the original method abstract.

There's an exercise that ask you to implement 1 + 2 * 3 using some kind
of Node structure. It should have `evaluate()` and `toString()` methods.

What devs usually do, is implement the `evaluate()` using switch/case by
checking the value of the operator. But it's no good, because then leaf
nodes will point to null values (see 9:57). Also the
value of some nodes will be 0, which is a silly idea.

The correct way to do this is to subclass `Node` and create a `ValueNode`
and an `OpNode` (see 12:13).

Conclusion: polymorphism is better, because new behavior can be added
without having or changing the original source code and each concern is
separate due to subclassing - this makes it easier to test and understand.

Summary: If you see a switch statement, then you should use polymorphism.
If you see an `if`, maybe it's just an `if`, maybe not.

Repeated condition
------------------
If the same `if` statements is repeated in different portions of the code
(when checking some flag for example), replace conditionals with polymorphism.

Using polymorphism allows also the modern JVM to inline your code and
execute it faster (better performance). Moreover there is already no
need to run the if statement all the time if you are in a loop.

There are 2 piles when building applications: the business logic pile and
the construction pile (also called the factories that wire the objects
together). So the decision making with `if`s moves from business logic
to factories.

Conditionals are now in a single location (not sprinkled all over the code),
no mode duplication (single if statement, easy to fix, less bugs), separates
the global state. Now all related things are in one location, makes it easy
to test independently and figure out what different subclasses are doing.

When to use polymorphism
------------------------
1. When the behavior changes based on state
2. When we have same `if` statement over and over again

**"I will not abuse conditionals in polymorphic languages."**
