---
layout: post
title: About Theme in website
date: 2019-12-14
updated: 2024-08-30
featured: true
categories: Website
tags: Markdown Jekyll HTML Theme Website
toc: 
  - name: What is Website Theme
  - name: Day Night Theme Swap
    subsections: 
      - name: Trigger Theme Change
      - name: Implement Theme Change
  - name: Jekyll Gem-based Theme
  - name: How Jekyll handle style
---

In this post, I will talk about what is website theme, What is Jekyll theme system, How Jekyll theme system work.

## What is Website Theme

A website theme manages the front-end design, establishing the overall appearance and functionality by managing its front-end design. 
Themes determine all design components: page layouts, backgrounds, color palettes, headers and footers, positioning, sizing, and typography. 
There are a word call **Theming**. Theming can be separate from functioning of the website. There are a separate position which is profession at creating website theming.

For example: 
- Different topic website should have different themes.
- Mobile and laptop should have different themes. 
- Day and night theme are for day night environment. 

> How Do Website Themes Work?
> A website theme works via CSS HTML and JS all together! 

## Day Night Theme Swap

Generially, implement theme on website will do these steps:

### Trigger Theme Change

#### Local storage
Use local storage to store current theme. 
Like this:
``` javascript
localStorage.setItem("theme", theme);
```

#### `prefers-color-scheme`
Use The `prefers-color-scheme` CSS media feature, A user indicates their preference through an operating system setting (e.g. light or dark mode) or a user agent setting.
Read more in [MDN doc](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme).

### Implement Theme Change

Usually there are three strategy: `media` strategy, `class` strategy and `selector` strategy.

Simply said, Using JavaScript function to toggle theme.
- Use document elements `classList.toggle()` function to toggle `class` for toggling them.
- Or use CSS Attribute selector to toggle theme.

#### `class` strategy
Using `classList.toggle()` or `classList.add()`, `classList.remove()` to toggle `class` in element, e.g. `body`. Then the `class` which toggling will affect the whold wetsite.

The main structure:
```javascript
<style>
.dark-mode {
  background-color: black;
  color: white;
   p {
     color: red;
  }
}
</style>
<script>
function myFunction() {
   var element = document.body;
   element.classList.toggle("dark-mode");
}
</script>
```

#### `Selector` strategy

Use root element CSS Attribute Selector to toggle Dark Mode.

- First, use `variable` and `Attribute Selector` to implement website theme:
```css
:root {
  --font-size: 20px;
  --background: white;
  ...
}
html[data-theme="dark"] {
  --font-size: 20px;
  --background: black;
  ...
}
```

- Second, Using **set** and **delete** root element's `data-theme` attribute to toggle website theme: 
```javascript
document.documentElement.setAttribute("data-theme", "dark"); 
document.documentElement.removeAttribute("data-theme");
```

> This website is using Attribute Selector to toggle Dark Mode.

## Jekyll Gem-based Theme

Choosing different Theme, you can build different style website.

Jekyll has an **extensive theme system**. This means it allows you to leverage community-maintained templates and styles to customize your site’s presentation. And also allow you override any of your theme's defaults by editing the theme's files. 

The Jekyll themes specify plugins and package up assets, layouts, includes, and stylesheets in a way that can be overridden by your site’s content. 

**Gem-based themes** make it easier for theme developers to make updates available to anyone who has the theme gem. 

### Theme for new Jekyll site
When you create a new Jekyll site, Jekyll installs a site that uses a gem-based theme called **Minima**.
Some of the site’s directories (such as the assets, _data, _layouts, _includes, and _sass directories) are stored in the **theme’s gem**, **hidden** from your immediate view. Yet all of the necessary directories will be read and processed during Jekyll’s build process.

The goal of gem-based themes is to allow you to get all the benefits of a robust, continually updated theme without having all the theme’s files getting in your way and over-complicating what might be your primary focus: creating content.

However, you can override any of the theme defaults with your own site content.To replace layouts or includes in your theme, make a copy in your _layouts or _includes directory of the specific file you wish to modify, or create the file from scratch giving it the same name as the file you wish to override

### Converting gem-based themes to regular themes

You can get rid of the gem-based theme and convert it to a regular theme, where all files are present in your Jekyll site directory, with nothing stored in the theme gem.

There are some steps to convert, for example:
- copy the files from the theme gem’s directory into your Jekyll site directory. 
- tell Jekyll about the plugins that were referenced by the theme.
- remove references to the theme gem in Gemfile and configuration. 
- do `bundle update`. If you’re publishing on GitHub Pages you should update only your _config.yml as GitHub Pages doesn’t load plugins via Bundler

### Installing a gem-based theme

You can find gem-based themes online and incorporate them into your Jekyll project. Search for jekyll theme on RubyGems to find gem-based themes.

Steps:
- Add the theme gem to your site’s Gemfile
- Install the theme
```terminal
bundle install
```
- Add the following to your site’s _config.yml to activate the theme
```yml
theme: jekyll-theme-minimal
```
- Build your site

If you’re publishing your Jekyll site on GitHub Pages, note that GitHub Pages supports only some gem-based themes.

## How Jekyll handle style

How Jekyll implement style on post? 

**Kramdown** is the default Markdown renderer for Jekyll. You can also configure Kramdown options.
Kramdown parse markdown file into HTML, and embed CSS style class for each HTML tag. 

### Layout
**_layouts** directory play **main** role on style of each website page.
Each page contain layouts information at `front matter`. 

For example, Every blog post page usually use `post` layout. You can change layout style on your specific post by using different layout.

### CSS
All CSS file will locate at assets/css/. And Jekyll use SASS to manage CSS.
The **_sass** folder is for Sass partials. Every file in here should begin with an underscore, and it will compile into main.scss in the css folder.

### Syntax highlighting for code blocks

By default, code blocks on your site will be highlighted by Jekyll. Jekyll uses the **Rouge** highlighter.
But you can use another highlighter, such as highlight.js, you must disable Jekyll's syntax highlighting by updating your project's `_config.yml` file.

#### Some Syntax highlighting snippet exmaple
Jekyll create `.highlight .k .n` etc. CSS class for Syntax highlighting `<code>` part. 
- `.highlight` wrap whole `<code>` tag
- `.k` class refer to keyword:

```css
.highlight .k {
  font-weight: bold;
}
```

You can customize these classes as well!

