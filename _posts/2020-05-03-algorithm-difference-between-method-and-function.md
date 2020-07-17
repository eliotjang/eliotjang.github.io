---
title: "[algorithm]Difference between a method and a function"
excerpt: "stackoverflow.com/questions/155609/whats-the-difference-between-a-method-and-a-function"

categories:
  - algorithm
tags:
  - stackoverflow

last_modified_at: 2020-05-03T22:00:00+09:00
---

## First answer

A **function** is a piece of code that is called by name. It can be passed data to operate on (i.e. the parameters) and can optionally return data (the return value). All data that is passed to a fucntion is explicitly passed.  


A **method** is a piece of code that is called by a name that is associated with an object. In most respects it is identical to a function except for two key differences:  

1. A method is implicitly passed the object on which it was called.
2. A method is able to operate on data that is contained within the class
    - (remembering that an object is an instance of a class - the class is the definition, the object is an instance of that data)  


## Second answer

A method is on an object.  
A function is independent of an object.  

For Java and C#, there are only methods.  
For C, there are only functions.  

For C++ and Python it would depend on whether or not you're in a class.  


## Simple way to remeber

- **F**unction → **F**ree (Free means not belong to an object or class)
- **M**ethod → **M**ember (A member of an object or class)

