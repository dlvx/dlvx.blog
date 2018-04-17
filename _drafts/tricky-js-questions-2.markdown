---
layout: post
title:  "Tricky JS Interview Questions Part 2"
tagline: "Assignment by reference"
date:   2018-01-4 17:50:00
categories: main
tags:
- Javascript
---

Hello all, this is the second part of my **Tricky JS Interview Questions** series.
You can read part 1 **here**. 

Just like I told you on the previous post, the questions from these series are intended to trick the interviewees a little bit but these are no more than simple questions involving the core concepts of Javascript. 

I've asked today's question to several people, juniors and seniors alike and most of the times they have failed it.

## What can we expect from the interviewee by asking this question? 

We expect that the interviewee knows the concept of **assignment by reference**, when does it happen and how it differs from **assigment by value**. 

Let's start!

-----

## What does the following console.log print?

{% highlight javascript %}

var a = ['Hello']
var b = ['Hello']

console.log(a === b)

{% endhighlight %}

So, what is it then? 
I have almost always received a **true** as an answer. When I ask why, the most common explanations have been something like "*because both a and b are the same*" or "*because both a and b have the same contents and both are the same type*".

The problem is that this is actually **false**. The first reason why this is **false** is because even tho both arrays have the same content, they are two independent objects ---arrays are objects in Javascript--- which are stored in different spots in memory.

The other and definitive reason is because what is assigned to `a` and `b` is not the arrays per se, but a reference to where these arrays are stored in memory. As both arrays are stored in separate spots in memory, both `a` and `b` will have different references. This is called **assignment by reference** and happens when we are assigning any kind of object to a variable ---objects, arrays and functions---.

So when we do `a === b` we are comparing if both `a` and `b` have the same value and the same type, and this fails cause `a` holds a different value ---the reference--- than `b`.

Confused? Let's explain this a little further.

## What is the difference between assignment by value and assignment by reference and when do they happen? 

This is actually another question I usually ask in the interviews.

The key definitions for these two concepts are not something specific to Javascript but to other programming languages too but I'll explain this from the Javascript perspective.

In short terms, **assigment by value** happens when we assign any **[Primitive][primitivesLink]{:target="_blank"}**
 to a variable. This variable will hold the actual value it has been assigned with.  For instance:

{% highlight javascript %}

var a = 1;
var b = 'Hello';
var c = true

{% endhighlight %}

Here, the variables `a`, `b` and `c` hold the values of `1`, `'Hello'` and `true` respectivelly ---those values are primitives---. 
Another example would be the following:

{% highlight javascript %}

var a = 1;
var b = a

{% endhighlight %}

Here both `a` and `b` hold the value of `1`. Here `b` has a **copy** of what's stored in `a`.

**Assigment by reference** happens when we assign any kind of **object** to a variable, the variable won't hold the object itself but a **reference** to where this object is stored in memory.

Let's look at a simple example:

{% highlight javascript %}

var a = { name: 'José' }

{% endhighlight %}

What happens there is that the object with name José is being created in memory, but what is assigned to the variable `a` is a reference ---an address--- of where in memory this object is stored. 

All this also happens when we pass parameters to a function. When we pass a **primitive** we say it is **passed by value** and when we pass an **object** we say it is **passed by reference**. 

Now, to better explain this I will steal the following explanation from a Stack Overflow's [answer][stackAnswer]{:target="_blank"}:

> Javascript is always pass by value, but when a variable refers to an object (including arrays), the "value" is a reference to the object.

## Beware of assignment/passing by reference

-----

There you have it, Javascript had just tricked us. 

I hope this was usefull to all of you. I will try to keep bringing more tricky Javascript questions.
Feel free to give any feedback or let me know if you spot any error.

Thanks for reading.


[primitivesLink]: https://developer.mozilla.org/en-US/docs/Glossary/Primitive
[stackAnswer]: https://stackoverflow.com/a/6605700/1438421