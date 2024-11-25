---
layout: post
title: TypeScript Utility Types
date: 2024-11-26
category: TypeScript
tags: TypeScript
---

TypeScript provides several utility types to facilitate common type transformations. These utilities are available globally.

- Utility types are `type`s.
- Utility types are used to **transform** one type to anther type.

## Record
`Record<Keys, Type>`
Constructs an object type whose property keys are `Keys` and whose property values are `Type`. 
This utility can be used to map the properties of a type to another type.

Need an Example explain it: 
```ts
type CatName = "miffy" | "boris" | "mordred";
 
interface CatInfo {
  age: number;
  breed: string;
}
 
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
 
cats.boris; // Here, the 'cats' is 'Record<CatName, CatInfo>'
```

### When you need an object which can be assigned new properties, you can use Record utility to define the object type.

Do this in past: 
```ts
var obj: {[k: string]: any} = {};
obj.prop = "value";
obj.prop2 = 88;
```

Now using `Record<Keys, Type>` is a better way.
```ts
var obj: Record<string, unknown> = {};
obj.prop = "value";
obj.prop2 = 88;
```

`Record<Keys,Type>` is a much cleaner alternative for key-value pairs where property-names are not known. It's worth noting that `Record<Keys,Type>` is a named alias to `{[k: Keys]: Type}` where `Keys` and `Type` are generics.

## Pick
`Pick<Type, Keys>`

Constructs a type by picking the set of properties Keys (string literal or union of string literals) from `Type`.
```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

## Omit
`Omit<Type, Keys>`

Constructs a type by picking all properties from `Type` and then removing `Keys` (string literal or union of string literals). The opposite of `Pick`.

Classic usage is omiting the `id` property for a object type.
```ts
type User = {
  id: string;
  name: number;
}
type Guest = Omit<User, `id`>; // then, Guest type just contain name. 
```

## ReturnType
`ReturnType<Type>`

Constructs a type consisting of the return type of function Type.
```ts
type T0 = ReturnType<() => string>;
// type T0 = string

function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<typeof f>;   
// type P = {
//     x: number;
//     y: number;
// }
```
