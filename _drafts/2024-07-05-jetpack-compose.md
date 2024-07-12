---
layout: post
title: Jetpack Compose Overview
date: 2024-07-04
category: Android
tags: Android Kotlin 
---

## What is Jetpack Compose

- Jetpack Compose is Android’s recommended modern toolkit for building native UI. 
- It simplifies UI development on Android with powerful tools, and intuitive Kotlin APIs.
- You won't be editing any XML layouts or using the Layout Editor. 
- You will call composable functions to define what elements you want, and the Compose compiler will do the rest. 
- Composable functions are like regular functions with a few differences: functions names are capitalized, you add the `@Composable` annotation before the function.
- In Jetpack compose you write declarative code that describes how data should be displayed as UI. 


## Three Phases

There are three phases in the process of transforming data into UI:
- **composition**: transforme composable functions into a UI tree.
- **layout**: use this UI tree as input, the collection of layout nodes contain all the information needed to eventually decide on each node's size and location in 2D space. during the layout phase the tree is reversed  using the following three-step algorithm:
  - first a node **measures** its children if any and 
  - then based on those measurements it **decides** on its own size and,
  - finally it **places** its children relative to its own position. 
At the end of the phase each layout note will have an assigned width and height and an x y coordinate of where it should be drawn.
each node only visited once.
- **drawing**: now we know the sizes and XY coordinates of all of our layout nodes. the tree is traversed again from top to bottom and each node draws itself on the screen.

## Modifier

Modifiers play a very important role in Jetpack Compose.

### How to use
We can chain multiple modifiers, like `Modifier.clip(CircleShape).size(40.dp),`. Each modifier node wraps the rest of the chain and the layout node Within.
For example when we chain a clip in a size modifier, the clip modifier node wraps the size modifier node which then wraps the image layout node in the layout phase:
{% include figure.html path="assets/img/android-modifier.png" class="img-fluid rounded z-depth-1" width="80%" %}


## The `MainActivity.kt` file

Notice there are some automatically generated functions in this code, specifically the `onCreate()` and the `setContent()` functions.

The `onCreate()` function is the entry point to this Android app and calls other functions to build the user interface. In Kotlin programs, the `main()` function is the entry point/starting point of execution. In Android apps, the onCreate() function fills that role.

The `setContent()` function within the `onCreate()` function is used to define your layout through composable functions. All functions marked with the `@Composable` annotation can be called from the `setContent()` function or from other Composable functions. The annotation tells the Kotlin compiler that this function is used by Jetpack Compose to generate the UI.

## FAQ

- Why not use HTML, CSS and JavaScript for UI? 

