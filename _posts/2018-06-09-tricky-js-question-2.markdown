---
layout: post
comments: true
title:  "Tricky Javascript Question #2"
tagline: "Assignment by reference"
date:   2018-06-09 18:42:00
author: "José Del Valle"
categories: main
tags:
- Javascript
---

Hello all, welcome to the second tricky Javascript question explanation.
You can read the first one **[here]({{ site.baseurl }}{% post_url 2018-04-01-tricky-js-question-1 %})**. 

Just like I told you in the previous post, the questions from these series are intended to trick the interviewees a little bit but these are no more than simple questions involving the core concepts of Javascript. 

I've asked today's question to several people, juniors and seniors alike and most of the times they have failed it.

## What can we expect from the interviewee by asking this question? 

We expect that the interviewee knows the concept of **assignment by reference**, when does it happen and how it differs from **assigment by value**. 

Let's start!

-----

## What does the following console.log print?

```javascript

var a = ['Hello']
var b = ['Hello']

console.log(a === b)

```

So, what is it then? 
Almost always I receive a **true** as an answer. When I ask why, the most common explanations have been something like "*because both a and b are the same*" or "*because both a and b have the same contents and both are the same type*".

Well... this is actually **false**. 

### But why?

The first reason why this is **false** is because even tho both arrays have the same content, they are two independent objects which are also stored in different spots in memory ---arrays are objects in Javascript---.

The other and definitive reason is because what is assigned to `a` and `b` is not the arrays per se, but a reference to where these arrays are stored in memory. As both arrays are stored in separate spots in memory, both `a` and `b` will have different references assigned to them. This is called **assignment by reference** and happens when we are assigning any kind of object to a variable ---objects, arrays and functions---.

So when we do `a === b` we are comparing if both `a` and `b` have the same value and the same type, and this fails cause `a` holds a different value than `b` ---a different reference---.

Behind the scenes it would be something like this:

```javascript

var a = '0000' // Where 0000 points to where the array ['Hello'] is stored
var b = '0001' // Where 0001 points to where the other array ['Hello'] is stored

console.log(a === b)

```

Confused? Let's explain this a little further.

## What is assignment by reference?

**Assignment by reference** happens when we assign an object to a variable. The variable won't hold the object per se but a reference to where this object lives in memory. The reference tho, is no more than a simple value, like an address. 

This differs from **assignment by value** which happens when we assign a **[Primitive][primitivesLink]{:target="_blank"}**
to a variable. This variable will hold a copy of the actual value it has been assigned with.

For example:

```javascript

var a = 1;
var b = 'Hello';
var c = true

var d = { name: 'José' }

```

Here, the variables `a`, `b` and `c` hold the values of `1`, `'Hello'` and `true` respectivelly ---those values are primitives--- but variable `d` has a reference to the memory location where the object with name `José` is actually stored. 

## Why does it happen only when assigning objects?

Things are gonna get a little bit confusing here. The following Stack Overflow's **[answer][stackAnswer]{:target="_blank"}** explains this very good:

> Javascript is always pass by value, but when a variable refers to an object (including arrays), the "value" is a reference to the object.

The thing is, Javascript variables will always hold a **Primitive** value and no more than that. So, when assigning objects, a reference is assigned instead. That reference is actually a **Primitive** value. 

A good way to see this would be with the following example:

```javascript

var a = { name: 'José' }
var b = a;

```

What happens there is that variable `a` will have the reference to the object with name `José`. Variable `b` will have a copy of that reference which is assigned by value ---cause the reference is just a primitive---.

At this point we can almost say **assigment by reference doesn't exist at all** in Javascript but it's still useful to say that objects are assigned by reference just to differentiate.

## Beware of assignment/passing by reference

You have to be carefull when you do the following:

```javascript

var a = { name: 'José' }
var b = a
b.name = 'Peter'

```

If you don't know what is happening underneath, you'll probaly think `b` is a copy of `a` and you would expect `a` to remain with the name `José`. But when you do `var b = a` you are actually assigning the same reference to `b`, menaning that both `a` and `b` will have access and will be able to ***mutate*** the same object.

All this also applies when passing an object to a function like so:

```javascript

var person = { name: 'José' }

function getNewPerson(person){
    var newPerson = person
    newPerson.name = 'Peter'
    return newPerson
}

var newPerson = getNewPerson(a)

```

What happens there is that the function `getNewPerson` will receive a reference of the object which is then assigned to `newPerson`, the same object is mutated after. At the end both variables `person` and `newPerson` will point to `{ name: 'Peter' }`.

-----

Let's leave it here, it was kind of a long explanation. I hope this was useful to all of you and if you have any questions just let me know and feel free to give any feedback.

Thanks for reading!


[primitivesLink]: https://developer.mozilla.org/en-US/docs/Glossary/Primitive
[stackAnswer]: https://stackoverflow.com/a/6605700/1438421