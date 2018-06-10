---
layout: post
title:  "Persisting the Redux state in the Session Storage"
tagline: "A shopping cart example"
date:   2018-01-4 17:50:00
categories: main
tags:
- Redux
---

During the last couple of weeks I was involved in two projects that use Redux. Both required to use either the Local Storage or the Session Storage to persist some of the data that was in the Redux state.

One of the apps required to store some shopping cart data in the Session Storage. During the development of this requirement I faced three possible approaches that I want to share and illustrate in this post. 

The first one is the easiest but the one you shouldn't do and involves setting the Session Storage directly in the Redux actions or the reducers. The second one involves using the Redux Store's `subscribe()` method which will execute a callback any time an action has been dispatched. The third and last one involves using Redux Middleware, which allows us to execute some logic between the dispatch of an action and the execution of the reducers.

Before looking at the approaches, let's take a look at the code involved before doing any manipulation of the Session Storage:

### Store Configuration

```javascript

// store configuration

```

### Actions

```javascript

// actions

```

### Reducer

```javascript

// reducer

```

-----

## Persisting the Session Storage

Now that we've seen the codebase, let's look at the possible approaches to persist the Redux state in the Session Storage.

### Setting the Session Storage directly inside the Redux actions/reducers (Don't do this)

```javascript

// setting session storage inside actions/reducers

```

The reason why you shouldn't do this is because Redux's reducers ---and action creators too--- should be **Pure Functions**. This mean that they shouldn't have secondary effects and their result should depend solely on the received params. So, the reducer's only responsability is to return a new version of the portion of the state it is resposible for ---in this case it would be the shopping cart state--- and this new version of the state depends on what is received from the actions. If we do something else ---just like storing data into the session storage--- it would mean the reducer will no longer be a Pure Function. 

### Using the Redux Store's subscribe() method

```javascript

// Store subscribe()

```


### Using Redux Middleware

```javascript

// Store subscribe()

```

-----

## Retrieving the Session Storage and putting it in the Redux Store

OK, we have already persisted the Redux State in the Session Storage but we are still missing a key part and it is to retrieve the data from the Session Storage and store it in the Redux Store. We want this to happen when the app loads so the store is populated before any other action occurs. This is fairly simple to achieve by creating a function that retrieves the data and passes it to the configureStore function:

```javascript

// retrieving from session storage

```