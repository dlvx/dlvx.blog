---
layout: post
title:  "Persisting the Redux state in the local or session Storage"
tagline: "A shopping cart example"
date:   2018-01-4 17:50:00
categories: main
tags:
- Redux
---

A few months ago I was involved in two projects that use Redux. Both required to use either the Local Storage or the Session Storage to store some of the data that was in the Redux state.

One of the apps required to store some shopping cart data in the Local Storage. During the development of this requirement I faced three possible approaches that I want to share and illustrate in this post. 

The first one is the easiest but not so good approach and involves setting the Local Storage directly in the Redux actions or the reducers. The second one involves using the Redux Store's `subscribe()` method which will execute a callback any time an action has been dispatched. The third and last one involves using Redux Middleware, which allows us to execute some logic between the dispatch of an action and the execution of the reducers.

## Local/Session Storage use cases

Let's first take a look at the difference between Local Storage and Session Storage:

 - **Local Storage**: Use this one when you want to persist the data. If you persist some data in the Local Storage you will then be able to retrieve it at any time even if you closed the browser. The only way of clearing the Local Storage is programatically via Javascript or by clearing the browser's cache. 

 The two methods we will use today are:

```javascript 

localStorage.setItem('name', 'value');
const foo = localStorage.getItem('name');

``` 

 - **Session Storage**: Similar to Local Storage but the data won't persist and will be cleared when the page session ends. It's methods follow the same structure: 

```javascript 

sessionStorage.setItem('name', 'value');
const foo = sessionStorage.getItem('name');

``` 

Make sure to use the one that fits your requirements. The good thing is that the same approaches can be used with either.

Let's now take a look at the approaches we can do to set/get data from the either kind of storage:

## 1. The bad approach: setting/getting the storage directly in the reducers/actions

I wanna talk about this one cause I've seen it being used. It's OK to think about this one as the first option and the easiest but it's not the ideal one. Let's take a look at an example.

Let's say you want to persist the data in an action creator. You may think it makes sense to kill two birds with one stone if you do so:

```javascript

function setName(name) {

  localStorage.setItem('name', name); // <-- Here

  return {
    type: SET_NAME,
    name
  }
}

```

Let's say you don't do it in an action creator but in a Reducer like so:

```javascript

function formReducer(state = initialState, action) {
  switch (action.type) {
    case SET_NAME:

      localStorage.setItem('name', name); // <-- Here

      return Object.assign({}, state, {
        name: action.name
      })
    default:
      return state
  }
}

```
### Why is this a bad approach?

Even tho this will work OK and the same data you pass to the reducer will be persisted in the Local Storage, it breaks one of the principles of Redux, which is that reducers and action creators should be **Pure Functions**. This means that their result should depend solely on the received params and more importantly in this case, that they shouldn’t have secondary effects. 

The only responsability of action creators should be to return an action object and the only responsability of a reducer should be to return a new state based on the received actions.

## 2. A better approach: store.subscribe()

This is the approach suggested by Dan Abramov ---co-author of Redux--- in this Stack Overflow **[answer](https://stackoverflow.com/a/35675304/1438421){:target="_blank"}** and it's pretty straightforward to implement.

## 3. My favorite approach: Redux Middleware


-----

## Retrieving the Session Storage and putting it in the Redux Store

OK, we have already persisted the Redux State in the Session Storage but we are still missing a key part and it is to retrieve the data from the Session Storage and store it in the Redux Store. We want this to happen when the app loads so the store is populated before any other action occurs. This is fairly simple to achieve by creating a function that retrieves the data and passes it to the configureStore function:

```javascript

// retrieving from session storage

```