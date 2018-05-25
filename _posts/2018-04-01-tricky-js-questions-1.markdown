---
layout: post
title:  "Tricky JS Interview Questions Part 1"
tagline: "Coercion and operators"
date:   2018-04-01 17:50:00
author: "José Del Valle"
categories: main
tags:
- Javascript
---

Welcome to the first part of my **Tricky JS Interview Questions** series.
I've been interviewing people at my current job for a couple of months now, specifically Javascript and Front End developers. It has been a great oportunity to revisit some of the basics and of course to keep improving my knowlendge and skillset. 

I've been improving my interview questions from time to time and I've also gotten very cool question ideas from my friends at the company. The questions I will be sharing here are not difficult ---well maybe some--- but tricky. These involve knowing the basics and core concepts of Javascript. 

The idea with this is to see how concentrated the interviewees are and how good they are at the basics.

I usually start with simpler questions ---but related--- if the interviewee is a Junior Dev but most of the times I just ask the tricky questions right away. 

I have asked today's question to around 20 people now, some with little experience and some with several years of experience, even more years than me ---sometimes a lot more--- and most of them have had a hard time answering it. 

## What can we expect from the interviewee by asking this question? 

We expect the interviewee knows what **Coercion** is and how **Operators** work ---or at least has had some experience with---

So...let's start! 

-----

## What does the following console.log print ?

{% highlight javascript %}

console.log(3 < 2 < 1); 

{% endhighlight %}

What do you think is the result here?

Most of the interviewees have said this is **false**. Well...it's actually **true**. I was wrong too when I first saw this. The problem here is that if we don't know what **Coercion** is or at least we haven't experienced it yet and if we don't know how **Operators** work, our brain will trick us and we will think the result is **false** because of course `3` is not less than `2` and `2` is not less than `1`. 

One problem I've seen is that the interviewees answer this right away without taking a moment and analyzing what is really happening. 

Let's put this in a more verbose way ---sorry for the long variable names---:

{% highlight javascript %}

var isThreeLessThanTwo = 3 < 2; // This results in false
var isThreeLessThanTwoLessThanOne = isThreeLessThanTwo < 1 // is false less than 1 ?

{% endhighlight %}

We have split the tricky operation into two and assigned each part to a different variable. Let's assign the first part `3 < 2` to the variable `isThreeLessThanTwo`. 

What would be the value assigned to that variable? Well `3 < 2` returns **false**.

Now let's go to the variable `isThreeLessThanTwoLessThanOne` and assign it the value of `isThreeLessThanTwo < 1`. That would be the same as doing `false < 1`. 

Even tho we wouldn't know what **Coercion** is and when does it happen, it's still probable that we have experienced it when using **booleans** as **false** and **true** are implicitly converted to `0` and `1` respectivelly.

So going back to `isThreeLessThanTwoLessThanOne`, we would be asking if `false < 1`. Here Javascript will implicitly *coerce* this to `0 < 1`, which returns **true**.

This means that `3 < 2 < 1` is in fact **true**.

Having said that.

## What is Coercion ?

It just means Javascript will implicitly convert a value's type to another, this is possible thanks to its dynamic typing ---type checking being done at run time---. **Type Casting** is when we do that **explicitly** and **Coercion** is when it happens **implicitly**.

So, let's take a look at some simpler operations:

{% highlight javascript %}

false > 1

false < true

'1' < 2

3 > '1'

{% endhighlight %}

Javascript will *coerce* some of the values in order to make the operation possible. This is what Javascript will do behind the scenes:

{% highlight javascript %}

0 > 1 // returns false

0 < 1 // returns true

1 < 2 // returns true

3 > 1 // returns true

{% endhighlight %}

Just like we saw before, `false` and `true` are *coerced* into `0` and `1` respectively.

In the last two operations `'1'` (a string) will be *coerced* to its numerical representation so the two numbers can be evaluated. These are just a few scenarios when coercion takes place.

The good old Javascript messing with things at convenience. This is why it is recommended to do comparissons with `===` rather than `==`. When using `===`, Javascript will check if the type is the same ---coercion won't happen---, whereas `==` allows for coercion. 

That's why `3 == '3'` is **true** but `3 === '3'` is **false**. 

-----

I hope this was usefull to all of you. I will try to keep bringing more tricky Javascript questions.
Feel free to give any feedback.

Thanks for reading!

### *Important note

The order and direction in which the operators are evaluated is called **Operator Precedence and Associativity**. You can find a cool table about it in **[here][operatorsTabe]{:target="_blank"}**

[operatorsTabe]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence
