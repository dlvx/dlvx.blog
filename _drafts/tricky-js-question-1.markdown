---
layout: post
title:  "Tricky JS Interview Questions Part 1"
tagline: "Coercion and operators"
date:   2018-01-4 17:50:00
categories: main
tags:
- Javascript
---

Welcome to the first part of my **Tricky JS Interview Questions** series.
I've been interviewing people at my current job for a couple of months now, specifically Javascript and Front End developers. It has been a great oportunity to revisit some of the basics and of course to keep improving my knowlendge and skillset. 

I've been improving my interview questions from time to time and I've also gotten very cool question ideas from my friends at the company. The questions I will be sharing here are not difficult -well maybe some- but tricky. These involve knowing the basics and core concepts of Javascript. 
Each part is composed of several questions related to the same topic, starting with the easy ones so the interviewee gets into a confort zone and then we just strike with the tricky ones.  

The idea with this is to see how concentrated the interviewees are, how good are they at the basics and how good are they at deduction. 

I have asked this question to around 20 people now, some with little experience and some with several years of experience, even more years than me - sometimes a lot more - and they all have failed it. 

So...let's start! 

This part is related to **Coercion and Operators**.

As I said before, we just start with the easy ones:

## What is the result of each of the following comparissons? 

{% highlight javascript %}

0 < 1

false > 1

false < true

'1' < 2

3 > '1'

{% endhighlight %}

I hope that the interviewee can at least get these ones right. 
The last two could cause some confusion cause there is some **Coercion** going on. We'll talk about that in a minute. 

## What about the following ?  

Here's where the trick starts.

{% highlight javascript %}

1 < 2 < 3; 

{% endhighlight %}

We were taught in school that 1 is less than 2 which is less than 3. That's easy. The interviewee will probably say the result of this operation is in fact **true**. Well, yes...it is, but not because what I've just said. Javascript's logic is different. We'll get back to that shortly.

For now let's just agree with the interviewee and continue with the next question.


## What about this one ?

{% highlight javascript %}

3 < 2 < 1; 

{% endhighlight %}

What do you think is the result here? False?
If you said **false** then you're **wrong**. I was wrong too when I first saw this. Both the brain and Javascript have tricked us. So far all my interviewees have failed too. Not because this is difficult, but because we just don't pay enough attention to what's happening. We could still don't know what **Coercion** is and still get this right. We just have to go step by step. Let's put this in a more verbose way -sorry about the long variable names-:

{% highlight javascript %}

var isThreeLessThanTwo = 3 < 2; // This results in false
var isThreeLessThanTwoLessThanOne = isThreeLessThanTwo < 1 // asks if false is less than 1

{% endhighlight %}

We have divided the tricky operation in two, and assigned each part to a different variable. Let's assign the first part `3 < 2` to the variable `isThreeLessThanTwo`. What would be the value assigned to that variable? **false** cause `3 < 2` is **false**.
Now let's go to the variable `isThreeLessThanTwoLessThanOne` and assign it the value of `isThreeLessThanTwo < 1`. That would be the same as doing `false < 1`. Even tho we wouldn't know what **Coercion** means and when does it happen, we would still know or at least have heard that **false** is converted to **0** and **true** is converted to **1**.

So going back to `isThreeLessThanTwoLessThanOne`, which will hold the result of `isThreeLessThanTwo < 1` and `isThreeLessThanTwo`. We would be asking if `false < 1` which is **true**.

This means that `3 < 2 < 1` is **true**.

Having said that.

### What is Coercion ?

It just means Javascript will implicitly convert a value's type to another, this is possible thanks to its dynamic typing - type checking being done at run time -. **Type Casting** is when we do that **explicitly** and **Coercion** is when it happens **implicitly**.

So, if we go back to two of the first easy questions where we had this:

{% highlight javascript %}

'1' < 2

3 > '1'

{% endhighlight %}

In both cases `'1'` (a string) will be **coerced** to `1` (a number) so the evaluation can compare the two numbers. This is just one scenario of many where coercion can take place.

The good old Javascript messing with things at convenience. This is why it is recommended to do comparissons with `===` rather than `==` because `===` will check if the type is the same - no coercion happening -, whereas `==` allows for coercion. That's why `3 == '3'` is **true** but `3 === '3'` is **false**. 

There you have it, Javascript had just tricked us. 

I hope this was usefull to all of you. I will try to keep bringing more tricky Javascript questions.
Feel free to give any feedback or let me know if you spot any error.

Thanks for reading.

### *Important note

The order and direction in which the operators are evaluated is called **Operator Precedence and Associativity**. You can find a cool table about it in **[here][operatorsTabe]{:target="_blank"}**

[operatorsTabe]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence