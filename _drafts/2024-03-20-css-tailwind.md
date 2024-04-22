---
layout: post
title: Tailwind CSS Instroduction
date: 2024-03-20
category: CSS
tags: CSS HTML
---

Tailwind CSS is an open source CSS framework. 

Tailwind CSS works by scanning all of your HTML files, JavaScript components, and any other templates for class names, generating the corresponding styles and then writing them to a static CSS file.

## Main Feature
- unlike other CSS frameworks like Bootstrap, it does not provide a series of predefined classes for elements such as buttons or tables. 
- Instead, it creates a list of "utility" CSS classes that can be used to style each element by mixing and matching.

For example, in other traditional systems, there would be a class message-warning that would apply a yellow background color and bold text. To achieve this result in Tailwind, one would have to apply a set of classes created by the library: `bg-yellow-300` and `font-bold`.

## Setup Tailwind CSS

### Play CDN

Use the Play CDN to try Tailwind right in the browser without any build step. The Play CDN is designed for development purposes only, and is not the best choice for production.

Add the Play CDN script tag to the `<head>` of your HTML file, and start using Tailwind’s utility classes to style your content.
```html
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello world!
  </h1>
</body>
</html>
```

### Using PostCSS

Installing Tailwind CSS as a PostCSS plugin is the most seamless way to integrate it with build tools like webpack, Rollup, Vite, and Parcel.

1. Install Tailwind CSS
Install tailwindcss and its peer dependencies via npm, and create your `tailwind.config.js` file.
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```
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

1. Install Tailwind CSS
Install tailwindcss via npm, and create your tailwind.config.js file.
```
npm install -D tailwindcss
npx tailwindcss init
```

2. Configure your template paths
Add the paths to all of your template files in your tailwind.config.js file.

3. Add the Tailwind directives to your CSS
Add the `@tailwind` directives for each of Tailwind’s layers to your main CSS file.

4. Start the Tailwind CLI build process
Run the CLI tool to scan your template files for classes and build your CSS.

5. Start using Tailwind in your HTML
Add your compiled CSS file to the `<head>` and start using Tailwind’s utility classes to style your content.
```html
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="./output.css" rel="stylesheet">
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello world!
  </h1>
</body>
</html>
```


## FQA


- Responsive Design
- How can I memorize so many utility classes?
- Utility Classes Are the Same As Inline Styles?

## References

- [Tailwind official](https://tailwindcss.com/)

