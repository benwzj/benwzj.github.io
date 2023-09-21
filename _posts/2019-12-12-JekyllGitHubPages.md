---
layout: post
title: " Introduce Jekyll and GitHub Pages "
date: 2019-12-12
featured: true
categories: Website
tag: gitHub pages
tag: Jekyll
---

## **What is Jekyll**

In short, Jekyll 
- is ruby software
- is a static website generator software
- can transfer some kind of files(like md, html, sass etc.) into static website files(like html, css, etc.)
- automtically update your website when you update your files (cool thing!)
- can run in your own computor or GitHub pages

## **Jekyll Main Points**

- Use Front Matter
- Use Liquid templating language
- Support Markdown which is less verbose than raw HTML
- Support Layouts which are templates that can be used by any page in your site and wrap around page content. They are stored in a directory called `_layouts`


## **Jekyll file system**

### General Rules:
- SCSS (Sass) files into CSS (.scss -> .css), and Markdown into HTML (.md -> .html)
- The "distribution" folder is called _site. This is what the static site generator generates! Never place any files in that folder; they will be deleted and overwritten.
- The _sass folder is for Sass partials. Every file in here should begin with an underscore, and it will compile into the css folder.
- Any file or folder placed into the main directory will compile into the _site directory as-is.

### Configuration

- In the main directory, there's a file called ***_config.yml***.
- This file contain all configuratin for the website.
- Like **url**, **baseurl** etc.

### Customizing Jekyll Theme

- **_includes** are files that should show up on every page - header, footer, etc.
- **_layouts**: The layout that content will conform to.
- **_pages**: It can contain all markdown files here so the main directory stays clean.
- **_posts** : This is where blogs go to.
- **blog** folder: contain an index.html. the main blog page that will contain all posts.
- **_sass** the sass partials

---
<br>

## **Front Matter**

The front matter is the first thing in the file and must take the form of valid YAML set between triple-dashed lines.
Here is a basic example:

```yml
---
layout: page
title: "PAGE-TITLE"
permalink: /URL-PATH
---
```

or

```yml
---
layout: post
title: "POST-TITLE"
date: YYYY-MM-DD hh:mm:ss -0000
categories: CATEGORY-1 CATEGORY-2
---
```

Any file that contains a YAML front matter block will be processed by Jekyll as a special file.

## **Liquid**

Liquid is a templating language which has three main components:
- objects
- tags
- filters

### Objects
Objects tell Liquid to output predefined variables as content on a page. Use double curly braces for objects.
For example, `{ { page.title } }` displays the `page.title` variable.

### Tags
Tags define the logic and control flow for templates. Use curly braces and percent signs for tags. 
For example:
```
{ % if page.show_sidebar % }
  <div class="sidebar">
    sidebar content
  </div>
{ % endif % }
```
### Filters
Filters change the output of a Liquid object. They are used within an output and are separated by a `|`
For example:
```
{ { "hi" | capitalize } }
```
---
<br>


## **GitHub Pages**

GitHub Pages is a static site hosting service that takes HTML, CSS, and JavaScript files straight from a repository on GitHub, optionally runs the files through a build process, and publishes a website.

It support User site(http(s)://<username>.github.io) or project site (http(s)://<username>.github.io/<repository>).
You can publish your site when changes are pushed to a specific branch, GitHub Pages will use Jekyll to build your site by default.
GitHub Pages does not support server-side languages such as PHP, Ruby, or Pythoy.

---
<br>

## **Setup GitHub Pages Steps**

<h3>Step 1: Enable GitHub Pages</h3>

1. Open a new browser tab, and work on the steps in your second tab while you read the instructions in this tab.
1. Under your repository name, click **Settings**.
1. Click **Pages** in the **Code and automation** section.
1. Ensure "Deploy from a branch" is selected from the **Source** drop-down menu, and then select `main` from the **Branch** drop-down menu.
1. Click the **Save** button.
1. Wait about _one minute_, then refresh this page for the next step.
   > Turning on GitHub Pages creates a deployment of your repository. GitHub Actions may take up to a minute to respond while waiting for the deployment. Future steps will be about 20 seconds; this step is slower.
   > **Note**: In the **Pages** of **Settings**, the **Visit site** button will appear at the top. Click the button to see your GitHub Pages site.

<h3>Step 2: Configure your site</h3>

1. Browse to the `_config.yml` file in the `my-pages` branch.
1. In the upper right corner, open the file editor.
1. Add a `theme:` set to **minima** so it shows in the `_config.yml` file as below:
   ```yml
   theme: minima
   ```
1. (optional) You can modify the other configuration variables such as `title:`, `author:`, and `description:` to further customize your site.
1. Commit your changes.
1. (optional) Create a pull request to view all the changes you'll make throughout this course. Click the **Pull Requests** tab, click **New pull request**, set `base: main` and `compare:my-pages`.
1. Wait about 20 seconds then refresh this page for the next step.

<h3>Step 3: Customize your homepage</h3>

_Nice work setting the theme! :sparkles:_

You can customize your homepage by adding content to either an `index.md` file or the `README.md` file. GitHub Pages first looks for an `index.md` file. Your repository has an `index.md` file so we can update it to include your personalized content.

1. Browse to the `index.md` file in the `my-pages` branch.
1. In the upper right corner, open the file editor.
1. Type the content you want on your homepage. You can use Markdown formatting on this page.
1. (optional) You can also modify `title:` or just ignore it for now. We'll discuss it in the next step.
1. Commit your changes to the `my-pages` branch.
1. Wait about 20 seconds then refresh this page for the next step.

<h3>Step 4: Create a blog post</h3>

**What is _frontmatter_?**: The syntax Jekyll files use is called YAML frontmatter. It goes at the top of your file and looks something like this:

```yml
---
title: "Welcome to my blog"
date: 2019-01-20
---
```

1. Browse to the `my-pages` branch.
1. Click the `Add file` dropdown menu and then on `Create new file`.
1. Name the file `_posts/YYYY-MM-DD-title.md`.
1. Replace the `YYYY-MM-DD` with today's date, and change the `title` of your first blog post if you'd like.
1. Type the following content at the top of your blog post:
   ```yaml
   ---
   title: "YOUR-TITLE"
   date: YYYY-MM-DD
   ---
   ```
1. Replace `YOUR-TITLE` with the title for your blog post.
1. Replace `YYYY-MM-DD` with today's date.
1. Type a quick draft of your blog post. Remember, you can always edit it later.
1. Commit your changes to your branch.
1. Wait about 20 seconds then refresh this page for the next step.

<h3>Step 5: Merge your pull request</h3>

1. Merge your changes from `my-pages` into `main`. If you created the pull request in step 2, just open that PR and click on **Merge pull request**. If you did not create the pull request earlier, you can do it now by following the instructions in step 2.
1. (optional) Delete the branch `my-pages`.
1. Wait about 20 seconds then refresh this page for the next step.

---

<br>

## **FQA**

### How to update posts

- All posts is located in `_posts` directory. You can add and update posts from there.
- Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file.

### baseurl issue

When you setup github page in github, it sets this baseurl to your repo name, for example 'blogs', if your repo is 'blogs'.
You can set this environment variable in \_config.yml file.

```yml
baseurl: "/blogs" # the subpath of your site
```

Seting up this will be helpful if you want to test the website in your local machine.
