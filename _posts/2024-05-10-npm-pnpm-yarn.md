---
layout: post
title: NPM YARN and PNPM
date: 2024-05-10
category: Programming
tags: npm Node.js
toc:
  - name: NPM
  - name: YARN
  - name: PNPM
  - name: npm vs yarn vs pnpm
  - name: FQA
---

## NPM

NPM stands for Node Package Manager. 
npm is a package manager for the JavaScript programming language maintained by Microsoft's npm, Inc. npm is the default package manager for the JavaScript runtime environment Node.js and is included as a recommended feature in the Node.js installer.

### Packages

A package in Node.js contains all the files you need for a module.
Modules are JavaScript libraries you can include in your project.
A package is registered in npmjs.com registry.

### About scopes

When you sign up for an npm user account or create an organization, you are granted a scope that matches your user or organization name. You can use this scope as a namespace for related packages.

> A scope allows you to create a package with the same name as a package created by another user or organization without conflict.
{: .block-warning}

When listed as a dependent in a package.json file, scoped packages are preceded by their scope name. The scope name is everything between the `@` and the slash `/`:

- "npm" scope:
`@npm/package-name`
- "npmcorp" scope:
`@npmcorp/package-name`

#### Scopes and package visibility
Unscoped packages are always public.
Private packages are always scoped.
Scoped packages are private by default; you must pass a command-line flag when publishing to make them public.

### CommonJS

CommonJS is a project to **standardize the module ecosystem** for JavaScript outside of web browsers (e.g. on web servers or native desktop applications).

CommonJS's specification of how modules should work is widely used today for server-side JavaScript with Node.js. It is also used for browser-side JavaScript, but that code must be packaged with a transpiler since browsers don't support CommonJS.

The other major module specification in use is the ECMAScript (ES) modules specification (**ES6 modules** aka ES2015 modules). CommonJS can be recognized by the use of the `require()` function and `module.exports`, while ES modules use `import` and `export` statements for similar (though not identical) functionality.

### Use npm command

npm manages downloads of dependencies of your project.

- If a project has a package.json file, by running `npm install`, it will install everything the project needs, in the `node_modules` folder, creating it if it's not existing already.
- You can also install a specific package by running `npm install <package-name>`.
- Updating packages `npm update`
- Running Tasks `npm run <task-name>`

### npx
Run packages without downloading using npx.


## YARN

YARN stands for Yet Another Resource Negotiator. It is an alternative package manager for JavaScript that was created in 2016 by Facebook, Google, Exponent, and Tilde. It was designed to address some of the issues and limitations of NPM, such as speed, reliability, and security.

YARN has a similar command-line interface as NPM, but with some differences and improvements. It also uses the same package.json file as NPM, but it adds another file called yarn.lock that locks the exact versions of your dependencies. It also creates a node_modules folder where it stores the installed packages.

### Advantages
- It is faster and more efficient than NPM when installing or updating packages
- It uses a flat dependency structure that avoids duplication and nesting of packages
- It supports offline installation of packages from a local cache
- It has a better resolution algorithm that ensures consistent and deterministic versions of packages across different environments
### Disadvantages
- It is not as widely used or supported as NPM by the JavaScript community
- It may not be compatible with some NPM packages or features
- It may have some bugs or issues that are not yet fixed or resolved

## PNPM

When using npm, if you have 100 projects using a dependency, you will have 100 copies of that dependency saved on disk. With pnpm, the dependency will be stored in a content-addressable store, so:

- If you depend on different versions of the dependency, only the files that differ are added to the store. For instance, if it has 100 files, and a new version has a change in only one of those files, pnpm update will only add 1 new file to the store, instead of cloning the entire dependency just for the singular change.
- All the files are saved in a single place on the disk. When packages are installed, their files are hard-linked from that single place, consuming no additional disk space. This allows you to share dependencies of the same version across projects.

As a result, you **save a lot of space** on your disk proportional to the number of projects and dependencies, and you have a lot faster installations!

### Boosting installation speed
pnpm perfoms installation in three stages:

- Dependency resolution. All required dependencies are identified and fetched to the store.
- Directory structure calculation. The `node_modules` directory structure is calculated based on the dependencies.
- Linking dependencies. All remaining dependencies are fetched and hard linked from the store to `node_modules`.

### pnpm Creating a non-flat node_modules directory

`npm` and `Yarn` create flat node_modules directory. 

But by default, `pnpm` uses **symlinks** to add only the direct dependencies of the project into the root of the modules directory.  pnpm Creating a non-flat node_modules directory

## npm vs yarn vs pnpm

| Feature	      | NPM	      | YARN	    | PNPM
| :-----------  | :-------- | :---------| :--------
| Speed	        | Slow	    | Fast	    | Faster
| Disk Space	  | High	    | Low	      | Lower
| Security	    | Low	      | High	    | Higher
| Compatibility	| High	    | Medium	  | Medium
| Popularity	  | High	    | Medium	  | Low
| Ecosystem	    | Rich	    | Medium	  | Medium
| CLI	          | Simple	  | Complex	  | Similar to NPM
| directory	    | flattened	| flattened	| symlinks


## FQA

### Is it a problem if mix using them in a project?
You can switch between them if you want, as long as you delete the existing node_modules folder and lockfile before installing with a different package manager.

### lockfile?
Use a lockfile to ensure reproducible installs across different machines and environments. A lockfile is a file that records the exact versions and sources of the packages that your project depends on, so that you can install them consistently every time. NPM uses a package-lock.json file, YARN uses a yarn.lock file, and PNPM uses a pnpm-lock.yaml file. You should commit these files to your version control system and update them whenever you add, remove, or update a package
