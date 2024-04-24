---
layout: post
title: Tailwind CSS Instroduction
date: 2024-03-20
category: CSS
tags: CSS HTML
toc:
  - name: What is Tailwind CSS
  - name: Setup Tailwind CSS
  - name: Use Tailwind CSS
  - name: References
---

## What is Tailwind CSS

### CSS Problems

1. Separation
2. Verbosity
3. Too Much Power
4. Zombies

Tailwind CSS is an open source CSS framework. It looks like solve the CSS problems.

### Tailwind CSS Main Feature

Unlike other CSS frameworks like Bootstrap, it does not provide a series of predefined classes for elements such as buttons or tables. 
Instead, it creates a list of **"utility" CSS classes** that can be used to style each element by mixing and matching.

For example, in other traditional systems, there would be a class message-warning that would apply a yellow background color and bold text. To achieve this result in Tailwind, one would have to apply a set of classes created by the library: `bg-yellow-300` and `font-bold`.

### How it work

Tailwind CSS works by scanning all of your HTML files, JavaScript components, and any other templates for class names, generating the corresponding styles and then writing them to a static CSS file.

### Why is Tailwind CSS Winning

Tailwind CSS is very popular nowaday. Why it is winning among so many CSS solution competitors?
Here some benefits may tell something:
- You aren’t wasting energy **inventing class names**. This is the hardest job for developer. No more adding silly class names like `sidebar-inner-wrapper` just to be able to style something, and no more agonizing over the perfect abstract name for something that’s really just a flex container.
- Your CSS stops growing. Using a traditional approach, your CSS files get bigger every time you add a new feature. With utilities, everything is reusable so you rarely need to write new CSS.
- Making changes feels safer. CSS is global and you never know what you’re breaking when you make a change. Classes in your HTML are local, so you can change them without worrying about something else breaking.
- No zombies CSS code!

Cons:
- Ugly ass HTML
- Learning Curve
- Non-standard
- Requires tooling

> Pro tip:
> Using 'Inline fold' VSCode plugin can fix the Tailwind CSS pollution.

### Tailwind CSS vs inline styles

- Designing with **constraints**. Using inline styles, every value is a magic number. With utilities, you’re choosing styles from a predefined design system, which makes it much easier to build visually consistent UIs.
- **Responsive design**. You can’t use media queries in inline styles, but you can use Tailwind’s responsive utilities to build fully responsive interfaces easily.
- **Hover, focus, and other states**. Inline styles can’t target states like hover or focus, but Tailwind’s state variants make it easy to style those states with utility classes.

## Setup Tailwind CSS

### Using PostCSS

Installing Tailwind CSS as a PostCSS plugin is the most seamless way to integrate it with build tools like webpack, Rollup, Vite, and Parcel.
1. Install Tailwind CSS
Install tailwindcss and its peer dependencies via npm, and create your `tailwind.config.js` file.
2. Add Tailwind to your PostCSS configuration
Add tailwindcss and autoprefixer to your `postcss.config.js` file, or wherever PostCSS is configured in your project.
postcss.config.js:
```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

### Using CLI

The simplest and fastest way to get up and running with Tailwind CSS from scratch is with the Tailwind CLI tool. 
1. Install Tailwind CSS.
2. Configure your template paths. 
Add the paths to all of your template files in your `tailwind.config.js` file.
3. Add the Tailwind directives to your CSS. 
Add the `@tailwind` directives for each of Tailwind’s layers to your main CSS file.
4. Start the Tailwind CLI build process. 
Run the CLI tool to scan your template files for classes and build your CSS.
5. Start using Tailwind in your HTML. 
Add your compiled CSS file to the `<head>` and start using Tailwind’s utility classes to style your content.
```html
<head>
  <link href="./output.css" rel="stylesheet">
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello world!
  </h1>
</body>
```

### Play CDN

Use the Play CDN to try Tailwind right in the browser without any build step. The Play CDN is designed for development purposes only, and is not the best choice for production.
```html
<head>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello world!
  </h1>
</body>
```

## Use Tailwind CSS

Tailwind CSS just is CSS. 95% of utility classes can convert to CSS directly. For example `padding`, you can search at Tainwind Doc and it will show you that how it design classes around padding and how to use them. And it also show you that `p-0` is `padding: 0px;` etc.

In Tainwind website, and press `ctrl + k`, you can access search directly.

> Pro tip:
> Using 'Tailwind CSS IntelliSense' VSCode plugin to write Tailwind CSS code.

Using 'Tailwind CSS IntelliSense' and CSS knowledge can fix 75% of Tailwind CSS coding directly. 
Another 25% you might need to check the Tailwind Doc. Like `position`, Tailwind is using `static`, `fixed`, etc. It may be a bit confusing at the beginning.

### Document `Core Concepts` Section

Ok, 95% classes can convert to CSS simply. And 5% of clssses are not map 1 to 1 to CSS. You just need to go to the `Core Concepts` section in the Tainwind Doc, and glance over them, you will get the most of the idea.

`Hover, Focus, and Other States` section go over 99% of the different edge use cases for handling things like hovering, focusing, pseudo element, and so on, which you can handle with CSS classes. But Tailwind gotten around that and the way they do that for pretty much everything with classes. 

For example, 
- It use **colon** to show hover effect: `hover:bg-violet-600`.
- It use **group** concept. 

## FQA

- Responsive Design
- How can I memorize so many utility classes?
- Utility Classes Are the Same As Inline Styles?
- What is `@tailwind` directives?
- What is Tailwind’s layers?

## References

- [Tailwind official website](https://tailwindcss.com/)
- [tailwind components](https://tailwindcomponents.com/)
- [tailwind cheatsheet](https://tailwindcomponents.com/cheatsheet/)

