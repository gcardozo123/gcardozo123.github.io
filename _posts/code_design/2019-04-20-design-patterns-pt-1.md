---
layout: post
title: Design Patterns pt 1
modified:
categories: code_design
description:
tags: [code design]
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2019-04-20T07:18:26-04:00
---

_Design Patterns: Elements of Reusable Object-Oriented Software_ by Erich Gamma, John Vlissides, Ralph Johnson and Richard Helm is one of those books that's mentioned everywhere as "one book every developer should read", then I've started reading it and decided to share my thoughts as it helps me to absorb the information. The
first chapter is a nice review on object oriented programming, here we go!

## Introduction

What is a design pattern? The book says "_Each pattern describes a problem which occurs over
and over again in our environment, and then describes the core of the solution
to that problem, in such a way that you can use this solution a million times
over, without ever doing it the same way twice_", thus, design patterns make it easier to reuse successful designs and architectures.
Design patterns cover architectural problems that are definitely not trivial to solve, and they're categorized in two criterias:

* Purpose: 
  * "_**Creational patterns** concern the process of object creation._"
  * "_**Structural patterns** deal with the composition of classes or objects._" 
  * "_**Behavioral patterns** characterize the ways in which classes or objects interact and distribute responsibility._"

* Scope:
  * "_**Class patterns** deal with relationships between classes and their subclasses. These relationships are established through
inheritance, so they are static—fixed at compile-time._" 
  * "_**Object patterns** deal with object relationships, which can be changed at run-time and are more dynamic. Almost
all patterns use inheritance to some extent. So the only patterns labeled "class patterns" are those that focus on class relationships.
Note that most patterns are in the Object scope_."

## Object Granularity
Design patterns help to determine the object granularity, i. e, size and number of objects. They also help to decide what should be an object,
for example, you can use the _Facade_ pattern to produce a higher level of abstraction for a complex system (I intend to detail
specific patterns on next posts).

## Object Interfaces
"_The set of all signatures defined by an object's operations is called the interface to the object. An object's interface characterizes the
complete set of requests that can be sent to the object._" A **type** denotes a particular interface, that said, an object might have several types -
as long as it implements multiple interfaces - and different objects might share the same type - part of an object may characterize one type and
other part another type.

A nice thing about interfaces is that they don't specify implementations, each class is free to implement an interface on its own way. Different 
objects might support the same request with completely different implementations. "_The run-time association of a request to an object
and one of its operations is known as **dynamic binding**._"

"_Dynamic binding means that issuing a request doesn't commit you to a particular implementation until run-time. Consequently, you can write programs that expect
an object with a particular interface, knowing that any object that has the correct interface will accept the request_." The ability to trust the same request to different objects (that implement the same interface) is known as **polymorphism**.

## Object Implementations (AKA Classes)
An object's implementation is defined by its class, but some classes might prefer to defer - some or all of its - implementations to subclasses, those classes are known as **abstract classes** and since they defer implementation(s), you can't instantiate an object from an abstract class. Classes that are not abstract are known as **concrete classes** and inheritance may be used to override (redefine) the parent's class implementations.

## Class vs Interface
"_An object's class defines how the object is implemented. The class defines the object's internal state and the implementation of its operations. In contrast,
an object's type only refers to its interface — the set of requests to which it can respond_." In other words, **type** only refers to the **interface** and 
**class** refers to the **implementation and internal state**. So there's a conceptual difference between **class inheritance** and
**interface inheritance (or subtyping)**: 
* _Class inheritance defines an object's implementation in terms of another object's implementation: it's a
mechanism for code and representation sharing._
* _Interface inheritance (or subtyping) describes when an object can be used in place of another_."

"_Class inheritance is basically just a mechanism for extending an application's functionality by reusing functionality in parent classes.
It lets you define a new kind of object rapidly in terms of an old one. It lets you get new implementations almost for free, inheriting most of what
you need from existing classes._" The author adds that the proper way of using inheritance is when "_all classes derived
from an abstract class will share its interface_", implying that "_ a subclass merely adds or overrides operations and does not hide operations of the parent
class_", that is, if you're trying to reduce a parent's functionality in a subclass you probably shouldn't be using inheritance. Why? Because requests should
be able to trust on an object's interface, that's how polymorphism works. There are some benefits of manipulating objects in terms of their interfaces:
* The system that asks requests to an interface is unaware of the specific types of objects they use, as long as the objects adhere to the interface it expects. Which
makes easier to change behavior just by swaping objects that implement the same interface.
* The system remain unaware of the classes that implement these objects. Systems only know about the abstract class(es) defining the interface. Thus, reducing coupling
since the system can trust multiple classes that implement a given interface.

These qualities lead us to the following principle of reusable object-oriented design:

<div style="text-align:center"><b>Program to an interface, not an implementation.</b><img src ="https://media.giphy.com/media/xT0xeJpnrWC4XWblEk/giphy.gif" /></div>

## Reuse mechanisms: Inheritance vs Composition
Inheritance and composition are two common reuse mechanisms in object-oriented systems. Reuse by inheritance is also known as _white box reuse_, refering to
the fact that internals of a parent class are visible to subclasses. On the other hand, reuse by composition is achieved by composing objects with well 
defined interfaces to achieve a complex functionality (wikipedia has a [good example](https://en.wikipedia.org/wiki/Composition_over_inheritance#Composition_and_interfaces)), it's known as _black box reuse_ due to the fact that object's internal details are not visible.

There are some disadvantages of inheritance, for example, "_you can't change the implementations inherited from parent classes at run-time, because inheritance
is defined at compile-time. Second, and generally worse, parent classes often define at least part of their subclasses' physical representation... any change in the parent's implementation will force the subclass to change._", i. e., if the parent class doesn't fit the problem domain exactly, you'll need a new parent class. The author
suggests to inherit from abstract classes whenever is possible, since they don't provide implementations, the problem won't happen.

Object composition also solves the previous problem, since it trusts on the interface, it's decoupled from implementations allowing objects to be dynamically defined
at run-time. And since no implementations are exposed, encapsulations is never broken, leading us to the second principle of object-oriented design:
<div style="text-align:center"><b>Favor object composition over class inheritance.</b></div>

You shouldn't need new components to achieve reuse, some component should already provide the necessary functionality for your system. But since that's rarely the case,
*inheritance and composition work together*. 

It's good to keep in mind that composition isn't perfect, using it in order to achieve more flexibility also yields highly parameterized software, possible computational
inefficiencies due to multiple indirections and code that's hard to understand.

## Parameterized types (AKA generics, AKA templates)
Parameterized types are another way of achieving reuse on typed languages. You probably already seen some `template <typename T> class Something` out there on C++ wilderness, that's how you say that the type of `T` will be determined by whoever uses `Something`, like in `Something<int>`, that's how containers such as `std::vector` are implemented.

## Toolkits and Frameworks
They provide another way of achieving reuse, their main difference is that a toolkit is basically a library, for example, you might use a specific math lib throughout your system and it won't affect your system's architecture, but using a framework would. A framework comes with several design decisions already implemented, thus taking
away some code design complexity from the hands of the developer. Some good examples of frameworks are game engines such as MonoGame, Heaps, Phaser, and some
Python frameworks such as Django and Pytest.

"_If applications are hard to design, and toolkits are harder, then frameworks are hardest of all._"

Keep in mind that using frameworks makes your application to rely on them, thus the application needs to evolve together with its frameworks.

## Conclusion
The book covers a lot more ground than is written here, but these are the parts I consider good to highlight. I'm enjoying it so far, I wasn't expecting such a good object-oriented programming review and principles on its first chapter. I intend to share my thoughts on the next chapters as well.

<div style="text-align:center"><img src ="../../images/potato-long-post.png" /></div>

<div style="text-align:center">
<script type='text/javascript' src='https://ko-fi.com/widgets/widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Buy me a coffee', '#151a19', 'M4M4NEUK');kofiwidget2.draw();</script> 
</div>