---
layout: post
title: TypeScript Operators
date: 2024-11-27
category: TypeScript
tags: TypeScript
---

## satisfies Operator

TypeScript developers are often faced with a dilemma: we want to ensure that some expression matches some type, but also want to keep the most specific type of that expression for inference purposes.

That means,
You don't have to use type annotation to define a variable in order to make it matches specific type. When you use type annotation to define a variable, this variable will be taken as the specific type. At this point, this variable will under the constraint of the specific type. 
In some case, you may just want to make sure the variable in the shape of specific type but  don't want the constraint. 
Then use satisfies operatior.

### Not care about property name, Just care about the type of property
Maybe we don’t care about if the property names match up somehow, but we do care about the types of each property. In that case, we can also ensure that all of an object’s property values conform to some type.
Like this:
```ts
type RGB = [red: number, green: number, blue: number];
const palette = {
    red: [255, 0, 0],
    green: "#00ff00",
    blue: [0, 0] //   will catch this error!
} satisfies Record<string, string | RGB>;
// Information about each property is still maintained.
const redComponent = palette.red.at(0); // this still work
const greenNormalized = palette.green.toUpperCase(); // this still work
```

### ensure that an object has all the keys of some type, but no more:
```ts
type Colors = "red" | "green" | "blue";
// Ensure that we have exactly the keys from 'Colors'.
const favoriteColors = {
    "red": "yes",
    "green": false,
    "blue": "kinda",
    "platypus": false //  ~~~~~~~~~~ error - "platypus" was never listed in 'Colors'.
} satisfies Record<Colors, unknown>;
// All the information about the 'red', 'green', and 'blue' properties are retained.
const g: boolean = favoriteColors.green;
```



