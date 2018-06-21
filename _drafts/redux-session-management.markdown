---
layout: post
title:  "Persisting the Redux state in the local or session storage"
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

Make sure to use the one that fits your requirements. The good thing is that the same approaches can be used with both.

Let's now take a look at the approaches we can do to set/get data from the either kind of storage while using Redux:

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

      return {
        ...state, 
        name: action.name
      }
    default:
      return state
  }
}

```
### Why is this a bad approach?

Even tho this will work OK and the same data you pass to the reducer will be persisted in the Local Storage, it breaks one of the principles of Redux, which is that reducers and action creators should be **Pure Functions**. This means that their result should depend solely on the received params and more importantly in this case, that they shouldn’t have secondary effects. 

## 2. A better approach: store.subscribe()

This is the approach suggested by Dan Abramov ---co-author of Redux--- in this Stack Overflow **[answer](https://stackoverflow.com/a/35675304/1438421){:target="_blank"}** and it's pretty straightforward to implement. 

You just have to use the `subscribe()` method which according to the official Redux documentation: 

 > Adds a change listener. It will be called any time an action is dispatched, and some part of the state tree may potentially have changed.

 You can implement this in the file where you set up the store, like so:

 ```javascript
import { createStore } from 'redux';
import rootReducer from './rootReducer';

const store = createStore(
  rootReducer
)

store.subscribe(() => {
  const state = store.getState()
  try {
    const serializedState = JSON.stringify(state);
    localStorage.setItem('state', serializedState);
  } catch (err) {
    console.log(err);
  }
})

```

But what if you don't want to persist the entire state but just part of it? It's pretty straightforward, let's say we want to persist some shopping cart data. In a separate function we can do something like this: 

```javascript
function persistShoppingCart(state){
  const data = state.shoppingCartState
  try {
    const serializedData = JSON.stringify(data);
    localStorage.setItem('shoppingCart', data);
  } catch (err) {
    console.log(err);
  }
}

export { persistShoppingCart }
```

Then we just call it in the `subscribe()` method:

 ```javascript
import { createStore } from 'redux';
import rootReducer from './rootReducer';
import { persistShoppingCart } from './persistenceHelpers';

const store = createStore(
  rootReducer
)

store.subscribe(() => {
  const state = store.getState()
  persistShoppingCart(state)
})

```

### Getting the data back when the app starts

Now, how do we get the persisted data from the local storage and put it in the Redux state when the app starts? 

We first add a loader function that will get the data from the local storage:

```javascript
function persistShoppingCart(state){ /* ... */ }

function loadShoppingCartData(){  
  try {
    const serializedData = localStorage.getItem('shoppingCart')

    return serializedData !== null ? 
      { shoppingCart: JSON.parse(serializedData) } : undefined
  } catch (err) {
    return undefined
  }
}

export { persistShoppingCart, loadShoppingCartData }
```

Then we just pass that function to the `createStore()` like so:

 ```javascript
import { createStore } from 'redux';
import rootReducer from './rootReducer';
import { persistShoppingCart, loadShoppingCartData } from './persistenceHelpers';

function persistedState(){
  return Object.assign(
    {},
    loadShoppingCartData(),
    // Other loaders can go here
  )
}

const store = createStore(
  rootReducer,
  persistedState() // setting the initial state
)

store.subscribe(() => {
  const state = store.getState()
  persistShoppingCart(state)
})

```

The `persistedState` function can excecute any loader function and return an object that will then be used as the initial application state, which we just pass to the `createStore` function. 

The complexity of the persistence and loader functions would vary depending on the store's structure and if what you want to work with is deeply nested in the store, but the principles are the same. 


## 3. My favorite approach: Redux Middleware


One of my favorite features in Redux is **[Middleware](https://redux.js.org/advanced/middleware){:target="_blank"}** which according to the official docs:

> Provides a third-party extension point between dispatching an action, and the moment it reaches the reducer.

Imagine a middleware function as something that will execute between the moment you dispatch an action and the moment a reducer executes. You can use the data comming from the action and also the data in the state and do whatever you need. 

The coolest part is that you can chain several middleware functions if you want to execute different things for different kinds of actions.

Using middleware isolates any logic you want to derive from an action, any side effect that we want to avoid having in the action creators or reducers. It is useful for persisting data in the local or session storage, [logging](https://github.com/evgenyrodionov/redux-logger){:target="_blank"}, calling APIs, event tracking and analytics. 

Let's see how we can implement a middleware function that will persist our data in the local storage:

-----

## Retrieving the Session Storage and putting it in the Redux Store

OK, we have already persisted the Redux State in the Session Storage but we are still missing a key part and it is to retrieve the data from the Session Storage and store it in the Redux Store. We want this to happen when the app loads so the store is populated before any other action occurs. This is fairly simple to achieve by creating a function that retrieves the data and passes it to the configureStore function:

```javascript

// retrieving from session storage

```