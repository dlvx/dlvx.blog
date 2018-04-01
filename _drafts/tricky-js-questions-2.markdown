---
layout: post
title:  "Tricky JS Interview Questions Part 2"
tagline: "By value, by reference & object comparissons"
date:   2018-01-4 17:50:00
categories: main
tags:
- Javascript
---

Hello all, this is the second part of my **Tricky JS Interview Questions** series.
You can read part 1 **here**. 

Just like I told you on the previous post, the questions from these series are intended to trick the interviewees a little bit but these are no more than simple questions involving the core concepts of Javascript. 

Today's questions involve the concepts of **assignment by value and by reference** and **object comparissons**.

I've asked today's questions to several people, and just like I did in part 1, I first start with a question that most of the interviewees would answer correctly, however most of them have failed at the final and tricky question, not knowing that they have already answered it correctly since the begining. 

Let's start with the first question then.

## What is the difference between assignment by value and assignment by reference? 

Well, this is not something specific to Javascript but to many other programming languages and should be of general knowledge. The good thing is that most of the times, the interviewees have answered this one correctly.

You could ask this too by replacing the word *assigment* with *passing* but I prefer *assignment* because it's more general. 

In short terms, assigment by value is when the variable holds the actual value it has been assigned with. 

When we talk about assigment by reference, the variable doesn't hold the actual data but a reference of where in memory that data resides. We will go back to this in a moment. 

For now, we just hope the interviewee knows what is the difference between both so we ask the next question:

## In which cases do each of them happen?  

This gets a little more JS specific. So, in which case does assignment by value happen and in which case does assigment by reference happen? 

Assignment by value happens when we assign **primitives** to a variable and assignment by reference happens when we assign **objects** to a variable. The same happens when we pass them as parameters to a function. We hope the interviewees already know what **primitive data types** are and what **objects** are so what I usually do is to I ask that at the beginning of the interview, for example asking them to mention the primitive types that are available in Javascript.

Let's see when does assignment by value happens:

{% highlight javascript %}

var a = 1;
var b = 'Hello';
var c = true

{% endhighlight %}

In all of those cases, variables a, b and c hold the actual values of 1, 'Hello' and true respectivelly ---those values are primitives---. 
This also applies when you assign a variable to another variable, for example:

{% highlight javascript %}

var a = 1;
var b = a

{% endhighlight %}

What happens here is that both a and b hold the value of 1. It is not the same 1 tho ---when I say the same 1, I mean the same 1 that resides in memory---. We could interpret this as b's value being a copy of a's value. 

If we go deeper then, what happens is that both a and b's values will have their own spots in memory. 

Let's see when assignment by reference happens:

{% highlight javascript %}

var person = { name: 'José Del Valle' }

{% endhighlight %}

What happens here is that the variable **person** doesn't hold the actual object, but a reference to where this object resides in memory. That actually reveals that variables in Javascript can only hold primitives. The reference of the object above is actually a primitive value, this primitive value being the address in memory of the actual object. 

We could even say that Javascript only assigns by value. The actual value of the data when we assing a primitive, and the value of the address in memory when we assign objects. 

Assigment by reference also applies when we assing arrays and functions to a variable. Let's remember that in Javascript arrays and functions are *special kinds* of objects. 

Enough with explanations, what we really hope here is that the interviewee knows what we have just talked above so let's go to the tricky question. 

## What does the following console.log print?

{% highlight javascript %}

var a = ['Hello']
var b = ['Hello']

console.log(a === b)

{% endhighlight %}

I don't know why but even when the interviewees have answered the previous questions correctly they almost always fail at this one. Maybe because they are not concentrated enough. Most of the times a get a quick *"That is true"* as an answer, even when we have already asked what is the difference between a == and a === comparisson ---which is one of the questions we always ask at the begining of the interview---. 


----------------------

There you have it, Javascript had just tricked us. 

I hope this was usefull to all of you. I will try to keep bringing more tricky Javascript questions.
Feel free to give any feedback or let me know if you spot any error.

Thanks for reading.

### *Important note

The order and direction in which the operators are evaluated is called **Operator Precedence and Associativity**. You can find a cool table about it in **[here][operatorsTabe]{:target="_blank"}**

[operatorsTabe]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence