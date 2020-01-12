---
layout: post
title:      "Object Lifecycles"
date:       2020-01-12 21:34:40 +0000
permalink:  object_lifecycles
---


Given the robust nature of objects in Ruby and how they created by well defined classes, it is equally as important to understand the life cycle of an object as well as how it relates to other objects to fully utilize metaprogramming.  For example in the Music CLI Lab we needed to understand the overall hierarchy of when an object is instantiated, when attributes are read, and when modules are extended/included to be able to manipulate objects and return desired results.  One of the most important factors to remember is Instantiation - when an object is created, what information is available to them?  Can information be accepted as an arguement?  This allows us to think about the hiearchy in which the object exists to decide what information can be accessible, which information is muteable, basically a foundation of how we want the object to behave.

In the Music CLI, we set out to define classes that would contain objects relating to things such as: Songs, Artists, Genres and included a module that extended certain methods across different classes.  It was extremely important to be aware of what attributes were shared amongst each class object to then be able to define methods that relied on a chain of objects and their subsequent values.

 It was also extremely important to know when during an object's life cycle a certain method can be implemented, as there are situations and circumstances that require new information being stored, such as the @@all method and when certain songs should be saved.  The new_from_filename method relied on a shared method contained in a module, which then accesses an array of music objects to determine whether a new object should be added to that array.  




