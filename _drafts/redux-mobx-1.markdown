---
layout: post
comments: true
title:  "From Redux to MobX Part 1"
tagline: "From a Redux developer perspective"
date:   2018-06-20 17:50:00
author: "José Del Valle"
categories: main
tags:
- Redux
- Mobx
---

This is the first part of a series where I'll explain **from a Redux developer perspective** the key differences between Redux and MobX.

I have been working with Redux full time for about 2 years now. I've enjoyed it a lot and have been able to understand it pretty well. It wasn't easy at the beginning mostly due to its functional approach but I got used to it pretty quick. I got to work with it in several projects from small to medium-large sized apps. 

Redux is the *de facto* state management solution if you are working with React and you'll probably find it in most React apps. 

With time I began to wonder if there was an alternative to Redux, something new and refreshing to me that I could learn and use so I found MobX. 

## What is MobX ? 

In a few words MobX is just another state management solution like Redux. Both share some common concepts but also very important differences. Yes we have a store in MobX ---we can have several stores actually--- and yes...we have actions in MobX too, we kind of have *connected* components but we'll jump into that and other concepts in the following articles. 

The first question I made myself when looking at MobX was...

## Where are the reducers?

Well there are no reducers in MobX. There is no such a thing as a **Pure Function** that responds to **actions** and returns a new version of the state. 

If you are moving from Redux to MobX, well...get rid of the reducers. 

The first important thing to have in mind from now on:

- The state is mutable in MobX unlike Redux's which should be immutable

## But who mutate the state?

**Actions** in MobX are the responsible of mutating or alter the state. 
While Redux Actions are just plain Javascript objects that tell us what in the state should change, in MobX these are methods that belong to the store and mutate the state directly.