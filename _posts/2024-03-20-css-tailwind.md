---
layout: post
title: Tailwind CSS Instroduction
date: 2024-03-20
category: CSS
tags: CSS HTML
toc:
  - name: What is Tailwind CSS
  - name: Use Tailwind CSS
  - name: Tailwind Docs
  - name: Setup Tailwind CSS
  - name: FQA
  - name: References
---

## What is Tailwind CSS

### CSS Problems

CSS have it's problems: 
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

## Use Tailwind CSS

### Tailwind CSS just is CSS 

95% of Tailwind utility classes can convert to CSS directly. For example `padding`, you can search at Tainwind Doc and it will show you that how it design classes around padding and how to use them. And it also show you that `p-0` is `padding: 0px;` etc.
In Tainwind website, and press `ctrl + k`, you can access search directly.

### Tailwind CSS IntelliSense

Using 'Tailwind CSS IntelliSense' VSCode plugin and CSS knowledge can do 75% of Tailwind CSS coding directly. 
You might need to check the Tailwind Doc for the usage of another 25%. For example `position`, Tailwind is using `static`, `fixed`, etc. It may be a bit confusing at the beginning.

## Tailwind Docs

Ok, 95% classes can convert to CSS simply. And 5% of clssses are not map 1 to 1 to CSS. You just need to go to the **'Core Concepts'** section in the Tainwind Doc, and glance over them, you will get the most of the idea.

### Hover, Focus, and Other States

'Hover, Focus, and Other States' in 'Core Concepts' Section go over 99% of the different edge use cases for handling things like hovering, focusing, pseudo element, and so on, which you can't handle with CSS classes. But Tailwind get around that and implement these functionality for pretty much everything with classes. 

For example, 
- It use **colon** to show hover effect: `hover:bg-violet-600`.
- It have **group** concept: 
When you need to style an element based on the state of some parent element, mark the parent with the `group` class, and use `group-*` modifiers like `group-hover` to style the target element.
- It have **peer** concept:
When you need to style an element based on the state of a sibling element, mark the sibling with the peer class, and use peer-* modifiers like peer-invalid to style the target element

### Responsive Design

> Working mobile-first
{: .block-warning}

By default, Tailwind uses a mobile-first breakpoint system.
What this means is that unprefixed utilities (like uppercase) take effect on all screen sizes, while prefixed utilities (like md:uppercase) only take effect at the specified breakpoint and above.

Where this approach surprises people most often is that to style something for mobile, you need to use the unprefixed version of a utility, not the sm: prefixed version. Don’t think of sm: as meaning “on small screens”, think of it as “at the small breakpoint“.

Example: 
Prefix the utility with the breakpoint name, followed by the `:` character.
```html
<!-- Width of 16 by default, 32 on medium screens, and 48 on large screens -->
<img class="w-16 md:w-32 lg:w-48" src="...">
```

If you’d like to apply a utility only when a specific breakpoint range is active, stack a responsive modifier like `md` with a `max-*` modifier to limit that style to a specific range.

### Dark Mode

**By default**, Tailwind is use `dark` variant to implement Dark Mode.
**By default**, Tailwind uses the `prefers-color-scheme` CSS `media` feature to trigger Dark Mode. 
You can change all these behavior if you want!

#### `dark` variant
Tailwind includes a `dark` variant that lets you style your site differently when dark mode is enabled.
Like this: `<div class="bg-white dark:bg-slate-800"></div>`

#### `selector` strategy
If you want to support toggling dark mode **manually** instead of relying on the operating system preference, for example, using `localStorage` to toggle Dark mode, then use the `selector` strategy instead of the `media` strategy. 
Add this:
```js
// in tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: 'selector',
  // ...
}
```
Now instead of `dark:{class}` classes being applied based on `prefers-color-scheme`, they will be applied whenever the `dark` class is present earlier in the HTML tree.
```html
<!-- Dark mode not enabled -->
<html>
<body>
  <!-- Will be white -->
  <div class="bg-white dark:bg-black">
    <!-- ... -->
  </div>
</body>
</html>

<!-- Dark mode enabled -->
<html class="dark">
<body>
  <!-- Will be black -->
  <div class="bg-white dark:bg-black">
    <!-- ... -->
  </div>
</body>
</html>
```

You can even Customize the attribute selector, for example `data-mode`, instead of `class`.
Do this:
```js
tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ['selector', '[data-mode="dark"]'],
  // ...
}
```

#### Supporting system preference and manual selection
Then do this:
```js
// On page load or when changing themes, best to add inline in `head` to avoid FOUC
if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
  document.documentElement.classList.add('dark')
} else {
  document.documentElement.classList.remove('dark')
}

// Whenever the user explicitly chooses light mode
localStorage.theme = 'light'

// Whenever the user explicitly chooses dark mode
localStorage.theme = 'dark'

// Whenever the user explicitly chooses to respect the OS preference
localStorage.removeItem('theme')
```

#### Overriding the dark variant
If you’d like to replace Tailwind’s built-in dark variant with your own custom variant, you can do so using the `variant` dark mode strategy:
```js
// in tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ['variant', '&:not(.light *)'],
  // ...
}
```
When using this strategy Tailwind will not modify the provided selector in any way, so be mindful of it’s specificity and consider using the :where() pseudo-class to ensure it has the same specificity as other utilities.

### Some Usage Cases

- `space-y-8`, this class can work as a **container** class which can make the items inside have 8 space between each other in `y` axle.
- `line-clamp-3`, this class allow 3 text lines to show in side it. `truncate`, allow 1 text line inside it.
- Show Gradient Color: `bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500`.
- Use `ring` border to have a shine button.
- Support Animation: 
  - Add the `animate-spin` utility to add a linear spin animation to elements like loading indicators.
  - Add the `animate-pulse` utility to make an element gently fade in and out — useful for things like skeleton loaders
- The official Tailwind CSS **Typography** plugin provides a set of `prose` classes you can use to add beautiful typographic defaults to any vanilla HTML you don’t control, like HTML rendered from Markdown, or pulled from a CMS.

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
