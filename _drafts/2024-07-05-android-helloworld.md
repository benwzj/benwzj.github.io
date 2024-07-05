---
layout: post
title: Android App First View
date: 2024-07-04
category: Android
tags: Android Kotlin 
---

## The `MainActivity.kt` file

Notice there are some automatically generated functions in this code, specifically the `onCreate()` and the `setContent()` functions.

The `onCreate()` function is the entry point to this Android app and calls other functions to build the user interface. In Kotlin programs, the `main()` function is the entry point/starting point of execution. In Android apps, the onCreate() function fills that role.

The `setContent()` function within the `onCreate()` function is used to define your layout through composable functions. All functions marked with the `@Composable` annotation can be called from the setContent() function or from other Composable functions. The annotation tells the Kotlin compiler that this function is used by Jetpack Compose to generate the UI.


