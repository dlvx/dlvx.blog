---
layout: post
title:  "Session Storage Management using Redux Middleware"
tagline: "A shopping cart example"
date:   2018-01-4 17:50:00
categories: main
tags:
- Redux
- Javascript
---

During the last couple of months I was involved in two projects that use Redux. Both required to use either the local storage or the session storage to persist some of the data that was in the Redux store.

One of the apps required to store the shopping cart data in the session storage. During the development of this requirement I faced three possible approaches that I want to share and illustrate in this post. 

The first one is possibly what will first come to mind and possibly the easiest one but incorrect if we want to respect the Redux approach of using pure functions for the actions and reducers. I'll still show it in here so you know what you shouln't do.

The second one is fairly simple and involves using a store subscriber. This method was recommended by Dan Abramov in a StackOverflow answer regarding this same topic.

The third and most advanced is using Redux middleware. Don't worry if you don't know what Redux middleware is cause I will explain it in here. 

I'll go through all the three approaches starting with a simple shopping cart app built with React and Redux ---you can clone it from here---. What it currently does is to store the shopping cart items in the Redux store but it doesn't persist this data yet ---which is a common requierement in e-commerce sites---. We'll use the Session Storage for this particular case.


The link to the repo with the final version will be available at the end of this post. 

Let's just take a quick look at the app's components, reducers and actions.

{% highlight javascript %}



{% endhighlight %}


Now, let's integrate the session storage management using the first approach. I'll explain why you shouldn't do it but it's still important to know it cause I think it could help you identify these kinds of bad practices when using Redux.

What we will do here is to directly set and get the session storage data in the reducer.


{% highlight javascript %}



{% endhighlight %}

The reason why you shouldn't do this is because Redux's reducers ---and action creators too--- should be **Pure Functions**. This mean that they shouldn't have secondary effects and their result should depend solely on the received params. So, the reducer's only responsability is to return a new version of the portion of the state it is resposible for ---in this case it would be the shopping cart state--- and this new version of the state depends on what is received from the actions. If we do something else ---just like storing data into the session storage--- it would mean the reducer will no longer be a Pure Function. 