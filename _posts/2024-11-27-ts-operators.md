---
layout: post
title: TypeScript Type Operators
date: 2024-11-27
category: TypeScript
tags: TypeScript
toc: 
  - name: Overview
  - name: Typeof
  - name: keyof
  - name: Indexed Access Types
  - name: Mapped Types
  - name: Conditional Types
  - name: Template Literal Types
  - name: satisfies Operator
---


## Overview

| Name              | Description                                                         |     Syntax                                    |
| :---------------- | :-----------------------------------------------------------------: | ------------: |
| typeof            | Obtains the type of a variable                                      | `type XType = typeof x`                                        |
| keyof             | Obtains the keys (property names) of a type.                        | `type PersonKeys = keyof Person`                             |
| Mapped Types      | Allows creating new types based on the properties of existing types | `type Optional<T> = {[K in keyof T]?: T[K]}`                      |
| Conditional Types | Allows expressing a type based on a condition                       | `type TypeName<T> = T extends string ? 'string' : 'non-string'` |


## Typeof

You will often be using `typeof` and `keyof` operators in combination, `typeof` to get the type of an object and `keyof` to index it.

In JavaScript (and TypeScript), "typeof" is a type-checking operator. It returns a string indicating the type of whatever operand you pass to it. 

### Extends typeof
TypeScript also **extends** it to be usable in **a type context** to get the TypeScript type of a variable.

```ts
type ShoppingList = {
    fruitAndVegetables: string[];
    meat: string[];
    dairy: string[]; //etc
};

const shoppingList: ShoppingList = {
    fruitAndVegetables: [],
    meat: [],
    dairy: [],
};

console.log('Shopping List:', typeof shoppingList);
//Shopping List: object
type ShoppingList2 = typeof shoppingList;
/*
    type ShoppingList2 = {
    fruitAndVegetables: string[];
    meat: string[];
    dairy: string[];
}
*/
```

### Use for narrowing
We'll deal with the first case, strings:
```ts
function concat(a: string | unknown[], b: string | unknown[]) {
    if (typeof a === 'string' && typeof b === 'string') {
        return a + b;
    }
}
```
TypeScript is smart enough to use the typeof operator here to know if we're inside that if statement, then a and b must have been strings.
 
## keyof 

The `keyof` operator is introduced by typescript. It gives the properties of an object type in form of a union. For example:
```ts
type X = {a: number; b: boolean;};
const x: X = {a: 1, b: false};
// Y = 'a' | 'b'
type Y = keyof X;
const y: Y = 'a';
/* Without explicitly specifying y as type Y TS will infer its type as string and will throw an error about indexing type X using string */
console.log(x[y]);
```

## Indexed Access Types
We can use an indexed access type to look up a specific property on another type:
```ts
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"]; // type Age = number
```

## Mapped Types

### What is Mapped Types
Mapped types build on the syntax for index signatures, which are used to declare the types of properties which have not been declared ahead of time:
```ts
type OnlyBoolsAndHorses = {
  [key: string]: boolean | Horse;
};
 
const conforms: OnlyBoolsAndHorses = {
  del: true,
  rodney: false,
};
```

Also you can use generic type to iterate through keys to create a type:
```ts
  type OptionsFlags<Type> = {
    [Property in keyof Type]: boolean;
  };
```
The type `OptionsFlags` will take all the properties from the type `Type` and change their values to be a boolean.

### Mapping Modifiers
Two additional modifiers: `readonly` and `?` affect mutability and optionality respectively.
You can remove or add these modifiers by prefixing with `-` or `+`.

### Key Remapping via `as`
You can leverage features like template literal types to create new property names from prior ones:
```ts
type Getters<Type> = {
    [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
};
```

## Conditional Types

Conditional types take a form that looks a little like conditional expressions (`condition ? trueExpression : falseExpression`) in JavaScript:
`SomeType extends OtherType ? TrueType : FalseType;`

Example: 
```ts
interface Animal {
  live(): void;
}
interface Dog extends Animal {
  woof(): void;
}

type Example1 = Dog extends Animal ? number : string; // type Example1 = number
type Example2 = RegExp extends Animal ? number : string; // type Example2 = string
```

## Template Literal Types

Template literal types build on **string literal types**, and have the ability to expand into many strings via unions.

They have the same syntax as template literal strings in JavaScript, but are used in type positions. 
When used with concrete literal types, a template literal produces a new string literal type by concatenating the contents.

```ts
type World = "world";
type Greeting = `hello ${World}`; // Now: type Greeting = "hello world"
```

## satisfies Operator

You may faced with a dilemma: we want to ensure that some expression matches some type, but also want to keep the most specific type of that expression for inference purposes.

That means: 
You don't have to use type annotation to define a variable in order to make it matches specific type. When you use type annotation to define a variable, this variable will be taken as the specific type. At this point, this variable will under the constraint of the specific type. 
In some case, you may just want to make sure the variable in the shape of specific type but  don't want the constraint. 
Then use `satisfies` operatior.

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

