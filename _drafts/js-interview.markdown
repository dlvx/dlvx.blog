---
layout: post
title:  "JS Interview"
date:   2018-01-4 17:50:00
categories: main
tags:
- Javascript
---

These are some javascript questions that I've prepared for technical interviews. Some are pretty easy but very important and some are intermidiate-ish. Some are not strictly related to javascript but to programming in general.

#1. What are primitive values? 

A primitive is a type of data that represents a single value. There are five primitive types in Javascript which are: 

- number
- string
- boolean
- undefined
- null

Primitives are not objects, which means they don't have properties. 

#2. What is the difference between *null* and *undefined* ?

***undefined*** is a special value given to variables that haven't been set, or in other words, that haven't yet been assigned a value. 

As stated in the book [Javascript: The Definitive Guide](https://books.google.co.cr/books?id=2weL0iAfrEMC&pg=PT77&lpg=PT77&dq=javascript+why+is+undefined+not+called+unassigned&source=bl&ots=_bWBoEXZ8F&sig=T-MnuyFGKRfItrYAohj4Bqb1ZEE&hl=en&sa=X&ved=0ahUKEwiO6-iq8sTYAhUOzlMKHQRjCBAQ6AEIMjAB#v=onepage&q=javascript%20why%20is%20undefined%20not%20called%20unassigned&f=false): "This type of *undefined* variable might more usefully be called *unassigned*"

It is actually the Javascript engine who assigns *undefined* to variables that have been declared but haven't been *initialized*. For example:

{% highlight javascript %}
let a //variable declaration. Javascript will assign the value 'undefined' to the variable 'a'
console.log(a) //will return 'undefined'
{% endhighlight %}

While ***null*** also represents a lack of existence, it has to be manually or programmatically assigned to a variable. This means it is never set "automatically" like *undefined*.

It is recommended, not to assign *undefined* to a variable when we want it to have no value, *null* should be used instead.

{% highlight javascript %}
let a = undefined //not recommended
let b = null //recommended
{% endhighlight %}

##If *null* is a primitive type then why **typeof** null equals 'object' ?
[Read this](http://2ality.com/2013/10/typeof-null.html) and [this](https://stackoverflow.com/questions/18808226/why-is-typeof-null-object) :)

#3. What is an object in Javascript ?

The simplest definition of an object in Javascript, as said by **Anthony Alicea** in his course **Javascript: Understanding the Weird Parts** is that "*an object is a collection of key/value pairs*" and these key/value pairs are what we call *object properties* when the value is a primitive or an object and methods when the value is a function.

[More info here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)

#5. What is a method?

A method is just a function that belongs to an object. 

#6. What is the meaning of the keyword 'this' in javascript ?

***this*** is a property available inside a function that refers to the object that invokes it, in other words, it is a shortcut to the invoking object. It will always refer to an object. It is also available in the global scope, in which it will refer to the global window object.

{% highlight javascript %}
console.log(this) //'this' points the global window object

function a(){
  console.log(this)
}

var b = function(){
  console.log(this)
}

a() //'this' points the global window object
b() //'this' points the global window object

var c = {
  name: 'The c object',
  log: function() {
    console.log(this) //When this function is executed, the js engine decides where the variable this should be pointing to.
  }
}

c.log(); //'this' points to the object 'c' because it is 'c' that it's invoking it.

{% endhighlight %}

**this** can be explicitly set to a function by using the **bind()** method

[More info here](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/), [here](https://www.sitepoint.com/what-is-this-in-javascript/) and [here](http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals/)

#7. What does the bind() method do?

#8. What is the scope?

#9. What is hoisting? 

#10. Sync vs Async
------
You'll find this post in your `_posts` 

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com