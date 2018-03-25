---
layout: post
title:  "Tricky JS Interview Questions Part 1"
tagline: "Coercion and operators"
date:   2018-01-4 17:50:00
categories: main
tags:
- Javascript
---

Welcome to the first part of my "Tricky JS Interview Questions" series.
I've been interviewing people at my current job for a couple of months now, specifically Javascript and Front End developer prospects. It has been a great oportunity to revisit some of the basics and of course to keep improving my knowlendge and skillset. 

I've been improving my interview questions from time to time and I've also gotten very cool question ideas from my friends at the company.

Now, the questions I will be sharing here are not difficult -well maybe some- but tricky. These of course involve knowing the basics and core concepts of Javascript. 

This is not a single question actually but a series of questions related to the same topic and asked in a certain order to induce to failure - I know it's mean but it's for a good cause -.

The idea with this is to see how concentrated the interviewees are, how good are they at the basics and how good are they at deduction. 

So, let's start with the first tricky question which is related to Coercion and Operators.

I have asked this question to around 20 people now, some with little experience and some with several years of experience, even more years than me...sometimes a lot more and they all have failed it. 

We first start giving some confidence to the interviewee by asking the following easy questions that he or she will most likelly answer correctly.

## What is the result of each of the following comparissons? 

{% highlight javascript %}

0 < 1; // true
false > 1; // false
false < true; // true
'1' < 2; // true 
3 > '1'; // false

{% endhighlight %}

Enough with easy comparissons. As some of you can tell, there is some 'Coercion' going on. We'll talk about that in a minute. 

The next question is when the trick starts.

## What about the following ?  

{% highlight javascript %}

1 < 2 < 3; // true

{% endhighlight %}

They will of course say it is true, because 1 is less than 2, which is less than 3. That's how our math teacher in school taught us. The thing is that Javascript has it's own way to do this, and it is not how our brain does it. More on this in a second.

So yeah, the interviewee says it is true, we aknowledge that without asking why is it true and instantly throw the next question.

## What about this one ?

{% highlight javascript %}

3 < 2 < 1; // true

{% endhighlight %}

Here's where everything ends. The interviewee will most likely say this is false, which is incorrect.

## But why ? 

How the hell could this be true? You may ask...
Don't blame me. It is all fault of one thing called Coercion.

### Coercion

Javascript likes to do coercion, which means it will implicitly convert a value's type to another, this is possible thanks to its dynamic typing. 

The good old Javascript messing with things at convenience. 

Let's go back to the first operations:

{% highlight javascript %}

false > 1; // false is coerced to 0, this gives 0 > 1 which is false.
false < true; // both false and true are coerced to 0 and 1 respectivelly, this gives 0 < 1 which is true.
'1' < 2; // '1' is coerced to 1 (Number)
3 > '1'; // '1' is also coerced to 1 (Number)

{% endhighlight %}

So here's what happens with the other operations:

{% highlight javascript %}

1 < 2 < 3; 

{% endhighlight %}

Here the first operation to be executed is the following:

{% highlight javascript %}

1 < 2; // This gives us true 

{% endhighlight %}

Hence we then have this:

{% highlight javascript %}

true < 3; // true is coerced to 0, so 0 < 3 results in true 

{% endhighlight %}

So yeah, the interviewee was probably right at this one but not because 1 < 2 < 3, but because Javascript first evaluated 1 < 2, resulting in true and then did true < 0 where true is coerced to 0 and of course 0 < 3 results in true.

The same thing hapens with the last one, so let's see:

{% highlight javascript %}

3 < 2 < 1; 

{% endhighlight %}

Again, Javascript first evaluates the comparisson at the left which is:

{% highlight javascript %}

3 < 2; // results in false

{% endhighlight %}

That one results in false, which gives us the following:

{% highlight javascript %}

false < 1; // false is coerced to 0

{% endhighlight %}

Here false is coerced to 0, which gives us 0 < 1, which results in true.

There you have it, Javascript has just tricked our brains. 

I hope this was usefull to all of you. I will try to keep bringing more tricky Javascript questions.

Thanks for reading.

### Important note

The order and direction in which the operators are evaluated is called Operator Precedence and Associativity. You can find a cool table about it in here: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence

