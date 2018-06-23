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

This is the first part of a series where I'll explain the key differences between Redux and MobX.

I have been working with Redux full time since 2016. I really enjoy working with it and have used it in several apps of all sizes. It is the *de facto* state management solution if you are working with React and you'll probably find it in most React apps.

After some time working with Redux I began to wonder if there was an alternative and after a quick research I found MobX. It seemed to be a very cool tool which is also widely used so I decided to give it a chance. 

In this guide I'll try to cover the most important concepts but won't try to tell you which one is better nor will tell you why you should use one or the other cause at the end it's all up to you and the requirements of your project.

I expect that you already know some Redux cause I will explain almost everything **from a Redux developer perspective** that is moving to **MobX** for the sake of learning.

## What is MobX ? 

In a few words MobX is just another state management solution like Redux. Both share common concepts but also have very important differences. There's a store --or several-- in MobX, there are actions and there are *connected* components --although this concept is a little bit different in MobX--. 

The general difference relies on the approach or paradigm suggested by each of them. **Redux** is **functional** oriented and relies on having an immutable state where the **reducers** --pure functions-- are the responsibles of returning a new version of the state without directly mutating it. The information to be updated comes from the dispatched **actions** which are no more that plain Javascript objects. 

**MobX** approach is in some way **Object Oriented** where we can use **classes** with **attributes** and **methods** as the representation of our stores. The attributes or properties being our state and the methods being the responsibles of either changing the state or computing data from it --like **setters** and **getters**--. The methods that change the state in MobX are actually called **actions** and they **mutate** it directly. 

The MobX concepts go way beyond that, so I will cover those in the next posts but that is the basic difference we will find between both. 

Having said that:

- **Redux** state is immutable. **Actions** are plain objects and only carry the information that will be updated in the state. **Reducers** are pure functions that return a new version of the state based on what they receive from the actions, but do not mutate the state.

- **Mobx** state is mutable. **Actions** can be any method or function that directly mutate the state. 

