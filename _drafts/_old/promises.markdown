---
layout: post
title:  "When do we use promises?"
date:   2018-01-17 17:50:00
categories: main
tags:
- Javascript
---

https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261

# What is a promise?

A promise is an object that may produce a single value some time in the future

A promise is an object which can be returned synchronously from an asynchronous function. It will be in one of 3 possible states:
Fulfilled: onFulfilled() will be called (e.g., resolve() was called) 
Rejected: onRejected() will be called (e.g., reject() was called)
Pending: not yet fulfilled or rejected

"Settled" = "not pending" (either resolved or rejected)

A settled promise is immutable, meaning that it cannot be resolved or rejected again.

Native JavaScript promises don’t expose promise states. Instead, you’re expected to treat the promise as a black box. Only the function responsible for creating the promise will have knowledge of the promise status, or access to resolve or reject.

