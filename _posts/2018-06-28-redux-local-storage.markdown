---
layout: post
comments: true
title:  "Persisting the Redux state in the Local Storage"
tagline: "A Redux Middleware use case"
date:   2018-06-28 17:50:00
author: "José Del Valle"
categories: main
tags:
- Redux
---

A few months ago I was involved in two projects that use Redux. Both required to use either the Local Storage or the Session Storage to persist some of the data that was in the Redux state.

One of the apps required to store some shopping cart data in the Local Storage. During the development of this requirement I faced three possible approaches that I want to share and illustrate in this post. 

The first one is the easiest but not so good approach and involves setting the Local Storage directly in the Redux actions or the reducers. The second one involves using the Redux Store's `subscribe()` method which will execute a callback any time an action has been dispatched. The third and last one involves using Redux Middleware, which allows us to execute some logic between the dispatch of an action and the execution of the reducers.

## 1. The bad approach: setting/getting the storage directly in the reducers/actions

I wanna talk about this one cause I've seen it being used. It's OK to think about this one as the first option and the easiest but it's not the ideal one. Let's take a look at an example.

Let's say you want to persist the data in an action creator. You may think it makes sense to kill two birds with one stone if you do so:

```javascript

function setName(name) {

  localStorage.setItem('name', name); // <-- Here

  return {
    type: "SET_NAME",
    name
  }
}

```

Let's say you don't do it in an action creator but in a Reducer like so:

```javascript

function formReducer(state = initialState, action) {
  switch (action.type) {
    case "SET_NAME":

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

I'm implementing it in the same file where I have created the store but you can choose to implement it anywhere you want:

 ```javascript
import { createStore } from 'redux';
import rootReducer from './rootReducer';

const store = createStore(
  rootReducer
)

store.subscribe(() => {
  // We first get the state
  const state = store.getState()
  try {
    // Serializing the state into JSON
    const serializedState = JSON.stringify(state);
    // Persisting it in the local storage
    localStorage.setItem('state', serializedState);
  } catch (err) {
    console.log(err);
  }
})

```

What we have there is a callback that persists the entire state in the local storage. If you want to persist only one part of the state you can do so following the same logic. For example some shopping cart data:


```javascript
function persistReduxState(key, state){
  try {
    // Serializing the shopping cart into JSON
    const serializedState = JSON.stringify(state);
    // Persisting it in the local storage
    localStorage.setItem(key, serializedState);
  } catch (err) {
    console.log(err);
  }
}

// export the function so we can then use it inside the subscribe() callback
export { persistReduxState }
```

We can then import the `persistReduxState` function and use it inside the subscribe method callback:

 ```javascript
import { createStore } from 'redux';
import rootReducer from './rootReducer';
import { persistReduxState } from './persistenceHelpers';

const store = createStore(
  rootReducer
)

store.subscribe(() => {
  // Get the entire state
  const state = store.getState()
  // Pass a key and the specific portion of the state we want to persist
  persistReduxState('shoppingCart', state.shoppingCart)
})

```

This way we have persisted only one part of the state into the local storage. It is most likely that we will want to retrieve that data back from the local storage when our app starts so let's see how we can accomplish that:

### Getting the data back from local storage when the app starts

To get the data back from the local storage and setting it as our initial Redux state is fairly simple. We have to add some logic that will do what we did above but the other way around. 

```javascript
function persistReduxState(state){ /* ... */ }

function loadPersistedState(key){  
  try {
    // get the data from local storage
    const serializedState = localStorage.getItem(key)

    // if there is data
    if(serializedState !== null){
      // return the parsed state
      return { [key]: JSON.parse(serializedState) }
    }

    // otherwise return undefined so createStore set's the default state {}
    return undefined

  } catch (err) {
    return undefined
  }
}

export { persistReduxState, loadPersistedState }
```

We can then create a new function called `persistedState()` where we can call our loader helpers. We can pass it to `createStore()` as the second param which will be the initial state. If it receives `undefined` it will default to `{}`.

 ```javascript
import { createStore } from 'redux';
import rootReducer from './rootReducer';
import { persistReduxState, loadPersistedState } from './persistenceHelpers';

function persistedState(){
  return {
    ...loadPersistedState('shoppingCart'),
    // Other loaders can go here
  }
}

const store = createStore(
  rootReducer,
  persistedState() // setting the initial state
)

store.subscribe(() => {
  const state = store.getState()
  persistReduxState('shoppingCart', state.shoppingCart)
})

```

The `persistedState` function can excecute any loader function and return an object that will then be used as the initial application state, which we just pass to the `createStore` function. 

The complexity of the persistence and loader functions will vary depending on the store's structure and if what you want to work with is deeply nested in the store, but the principles are basically the same. 


## 3. My favorite approach: Redux Middleware


One of my favorite features in Redux is **[Middleware](https://redux.js.org/advanced/middleware){:target="_blank"}** which according to the official docs:

> Provides a third-party extension point between dispatching an action, and the moment it reaches the reducer.

Imagine a middleware function as something that will execute between the moment you dispatch an action and the moment a reducer executes. You can use the data comming from the action and also the data in the state and do whatever you need. 

The coolest part is that you can chain several middleware functions if you want to execute different things for different kinds of actions.

Using middleware isolates any logic you want to derive from an action, any side effect that we want to avoid having in the action creators or reducers. It is useful for persisting data in the local or session storage, logging, calling APIs, event tracking and analytics. There are serveral useful packages that use this approach.

We can now reuse the `persistReduxState` function and use it inside our middleware:

```javascript
import { persistReduxState } from './persistenceHelpers';

const persistenceMiddleware = store => next => action => {
  
  if(action.type === "SET_SHOPPING_CART_DATA"){
    persistReduxState('shoppingCart', action.data)
  }

  return next(action)
}
```

There may be some confusing things about that function so I will explain it further. Putting it in a few words, what we see there is a function that returns a function which returns another function. This is called **[currying](https://medium.com/front-end-hacking/javascript-es6-curry-functions-with-practical-examples-6ba2ced003b1){:target="_blank"}**. Where we actually put our logic is in the last function which will have access to all the parameters passed to the previous functions. 

So in our logic we will have access to the `action` we just dispatched in our app, the `next` function which will pass the `action` to the next middleware in the chain or to the reducer if no more middleware is available, and finally the `store`. 

Remember when we said that middleware will execute between the moment an action is dispatched and the moment it gets to a reducer? 
If we do `store.getState()` before calling `next()` we will get the current state, if we do it after, we will get the next state. So another way we can do this would be the following:


```javascript
import { persistReduxState } from './persistenceHelpers';

const persistenceMiddleware = store => next => action => {

  // Calling store.getState() here would give us the current state
  
  // We call next(action) which will pass the action to the next middleware
  // It will go down the chain of middlewares until it gets to the reducer
  const result = next(action)

  // Calling store.getState() here will now give us the next state
  const nextState = store.getState()
  
  // We can validate the current action type and do some logic there just like in a reducer
  if(action.type === "SET_SHOPPING_CART_DATA"){
    persistReduxState('shoppingCart', nextState.shoppingCart)
  }

  return result
}

```

Connecting our middleware to Redux just requires passing it inside the `applyMiddleware` function as the third parameter of `createStore` like so:

```javascript
import { createStore, applyMiddleware } from 'redux';
import rootReducer from './rootReducer';
import { loadPersistedState } from './persistenceHelpers';
import { persistenceMiddleware } from './middleware',

function persistedState(){
  return {
    ...loadPersistedState('shoppingCart'),
    // Other loaders can go here
  }
}

const store = createStore(
  rootReducer,
  persistedState(), // setting the initial state
  applyMiddleware(persistenceMiddleware) // applying middleware
)

```

As you can see, retrieving the data from the local storage to set it as our initial state remains exactly the same as the previous approach. 

This was just one of many use cases where you can use middleware. It may feel that we are writing a lot of code for something as simple as setting data in the Local Storage but it's important to remember that it is recommended to keep our reducers and actions as pure as possible. This approaches will make more sense if you have to use them for more than one thing and you'll be able to notice how helpful they are. 

If you have any question or suggestion just reach out.

Thanks for reading.
