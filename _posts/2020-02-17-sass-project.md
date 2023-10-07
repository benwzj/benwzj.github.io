---
layout: post
title: "Implement SASS in a project"
date: 2020-02-17
categories: CSS
tags: SASS Jekyll HTML
---


You can organize Sass into your project. Instead of having one huge CSS file that becomes hard to maintain, you can compartmentalize all the components of your site into multiple files known as partials, and compile them into one, minified CSS file. Any change made to these partials will be detected, and the main file will be updated.

Jekyll use SASS!
Bootstrap use SASS

## Let's make a simple example

There is any number of ways you can organize your project - compiled css in a dist folder, and scss source code in a src folder is one way. I'm going to create a css folder and a sass folder inside of one project for simplicity.

It's good to organize and separate your content, but I also wouldn't go overboard with creating so many files and directories that it becomes a task to find where something is. Everything should have a utility and be obviously named.

> **Important**:  Every file in your project will start with an underscore except for the main scss file.
{: .block-warning }

- css/
  - main.css
- sass/
  - main.scss
  - base/
    - _variables.scss
    - _mixins.scss
    - _reset.scss
  - components/
    - _typography.scss
    - _grid.scss
    - _buttons.scss
    - _navigation.scss
    - _sections.scss


In this example, I'm putting all my global variables and mixins in the base directory, along with any resets I might need. In the components directory, I'll put all the styles for grid, navigation, typography, etc. I'll use ___sections.scss__ to put my code for individual sections of the page. If your project is very large, you might create a more specific directories.

The __main.scss__ file will import all the partials from all the directories. You won't need to add the underscore or extension when importing the filenames - instead of "_variables.scss" you will simply write "variables".

Here's how the __main.scss__ file will look for this particular project.

```javascript
// Base

@import 'base/variables';
@import 'base/mixins';
@import 'base/reset';

// Components

@import 'components/typography';
@import 'components/grid';
@import 'components/buttons';
@import 'components/navigation';
@import 'components/sections';

```
Now I can watch this entire project and compile everything into one, minified CSS file with a single command.

```console
$sass --watch sass:css --style compressed
```

With this command, I'm watching the entire sass directory for changes, and telling it compile into the css directory, and compress the output.
```
> > > Sass is watching for changes. Press Ctrl-C to stop.

  write css/main.css
  write css/main.css.map

Any change in the directory will be registered.

> > > Change detected to: sass/components/\_grid.scss

  write css/main.css
  write css/main.css.map
```