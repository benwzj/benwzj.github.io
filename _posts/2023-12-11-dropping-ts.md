---
layout: post
title: Should TypeScript be ditched
date: 2023-12-11
category: TypeScript
tags: TypeScript JavaScript
---

Many people answer question why using TS, they usually saying I can use inferface, I can use generic to make reuseful functions. Are them your main reasons?

Now the people begin talk about dropping TypeScript. Why?

The main reasons looks like this:

> Not just because it requires an explicit compile step, but because it pollutes the code with type gymnastics that add ever so little joy to my development experience, and quite frequently considerable grief. Things that should be easy become hard, and things that are hard become `any`.

One of the longest running schisms in programming is that of static vs dynamic typing. There were a million arguments from both sides throughout these years, but seen very few of them ever convinced anyone of anything. As rationalizations masquerading as reason rarely do in matters of faith. 

We know that types are always there, but the difference is that they are in your codes or in your mind.

## JSDoc 

Both TypeScript and JSDoc aim to improve the development experience and enhance JavaScript codebases.

TypeScript introduces a type system to JavaScript, enabling developers to catch errors at compile-time rather than runtime. 

What is JSDoc?
JSDoc is a markup language and a documentation tool for JavaScript. It allows developers to add structured comments to their JavaScript code, providing valuable information about the codebase. Here are some notable aspects of JSDoc:

- Type Annotations: JSDoc supports type annotations similar to TypeScript, allowing developers to document the expected types of variables, parameters, and return values.
- Code Documentation: JSDoc allows developers to document functions, classes, objects, and their members, providing information about their purpose, usage, and expected behavior.
- Tool Integration: JSDoc-generated documentation can be utilized by various tools and IDEs to provide context-aware help, autocompletion suggestions, and inline documentation.
- Custom Tags: JSDoc supports custom tags, which enable developers to extend the existing set of annotations and document additional information specific to their codebase or project.

### JSDoc vs TypeScript

- Type Checking: TypeScript performs static type checking during compilation, catching type-related errors before runtime. JSDoc, on the other hand, is primarily a documentation tool and does not provide type checking by itself.
- Language Features: TypeScript introduces additional language features like interfaces, classes, and modules, while JSDoc focuses on providing documentation annotations for existing JavaScript code.
- Ecosystem and Tooling: TypeScript has a mature ecosystem with strong tooling support, including IDE integrations, build systems, and popular frameworks. JSDoc, being a documentation tool, complements existing JavaScript development workflows and is commonly used alongside other tools.

## Some libraries turn around
So big libraries like Turbo, Svelte, Drizzle are deciding to ditch TypeScript from their code bases. Are there more to ditch TypeScript in the future? 

TypeScripte become popular since it come out at 2012 from MicroSoft. The fact is not changed, the pros and cons of TypeScript are not changend. But people can change.

Let us enjoy JavaScript in the glorious spirit it was originally designed: Free of strong typing.