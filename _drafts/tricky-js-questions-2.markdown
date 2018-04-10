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

I've asked today's question to several people, juniors and seniors alike and most of the times I have received a wrong answer.

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

The problem is that this is actually **false**. The first reason why this is **false** is because even tho both arrays have the same content, they are two independent objects in memory ---arrays are objects in Javascript---.

The other and definitive reason is because what is assigned to `a` and `b` is not the arrays per se, but a reference to where these arrays are stored in memory. As both arrays are stored in separate spots in memory, both `a` and `b` will have different references. This is called **assignment by reference** and happens when we are assigning any kind of object to a variable ---objects, arrays and functions---.

So when we do `a === b` we are comparing if both `a` and `b` have the same value and the same type, and this fails cause `a` holds a different reference than `b`.

Confused? Let's look at this more in depth:

## What is the difference between assignment by value and assignment by reference and when do they happen? 

This is actually another question I usually ask in the interviews.

The key definitions for these two concepts are not something specific to Javascript but to other programming languages too and should be of general knowledge. 

In short terms, **assigment by value** is when the variable holds the actual data it has been assigned with. This applies for when we are assigning data that is a **[Primitive][primitivesLink]{:target="_blank"}**. For instance:

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

Here both `a` and `b` hold the value of `1`. `b` having a **copy** of what's stored in `a`.

When we talk about **assigment by reference**, the variables don't hold the actual data but a reference of where in memory this data resides. As I said before, this happens when we are assigning any kind of object to a variable. 

Let's look at a simple example:

{% highlight javascript %}

var a = { name: 'José' }

{% endhighlight %}

What happens there is that the object of name José is being created in memory, but what is assigned to the variable `a` is a reference ---an address--- of where in memory this object is stored. 

All this also happens when we pass parameters to a function. When we pass a **primitive** we say it is **passed by value** and when we pass an **object** we say it is **passed by reference**. 

It is important to know that when we pass parameters to a function, what this function has inside is a copy of what has been passed to it, and this applies to both values and references. 

## Beware of assignment/passing by reference

-----

There you have it, Javascript had just tricked us. 

I hope this was usefull to all of you. I will try to keep bringing more tricky Javascript questions.
Feel free to give any feedback or let me know if you spot any error.

Thanks for reading.


[primitivesLink]: https://developer.mozilla.org/en-US/docs/Glossary/Primitive
