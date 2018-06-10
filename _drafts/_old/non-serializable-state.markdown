---
layout: post
title:  "Putting non-serializable data in the Redux store"
tagline: "And why you shouldn't do it"
date:   2018-01-17 17:50:00
categories: main
tags:
- Redux
- React
- Javascript
---

Recently while doing a code review I found out that a function was being stored in the redux state. 
This portion of the redux state contained some properties (and that function) which would then be used by a component.
The component would display these properties and would use the stored function to compute some logic over some received params
and return a result.

---
At the time I noticed that, I only knew it was a bad practice but I didn't know why it was like that so I decided to play a little bit with it and to do some research to find out. 
---

The official redux docs says this is a bad practice and that we shouldn't put "non-serializable" stuff into the state. What does "non-serializable" mean and why does this represents a hazard? We will get back to that in a moment. 



We'll begin with a simple example, that won't cause any trouble to us.
Let's say we have the following portion of the state, which has some primitives and a function. 

```javascript
{
    id: "123",
    name: "José Del Valle",
    yearOfBirth: 1993,
    getAge: (yearOfBirth) => { return (new Date()).getFullYear() - yearOfBirth }
}
```

Now let's connect a component to that part of the state and use the getAge function.

```javascript

```

Excecuting this function wouldn't cause us any trouble as it is a pure function that depends solely on the received params.

https://redux.js.org/docs/faq/OrganizingState.html#organizing-state-non-serializable
https://stackoverflow.com/questions/27926619/why-is-a-function-not-serializable
http://web.archive.org/web/20150419023006/http://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html