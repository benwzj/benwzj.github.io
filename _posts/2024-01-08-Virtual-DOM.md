---
layout: post
title: Virtual DOM concept
date: 2024-01-08
category: React
tags: JavaScript DOM SPA Svelte
---


## DOM rendering
First, Let's get to know a bit about DOM rendering.
In traditional rendering, the Browser does the following tasks:
- The browser parses our HTML and stores it in memory as a **tree structure** of a document, which is also known as DOM (Document Object Model) or sometimes as Real DOM. DOM methods allow programmatic access to the tree. With them, you can change the document’s structure, style, or content.
- The browser uses DOM to create a render tree. **Render** Tree represents everything that will be rendered on the browser. 
- **Layout** Render Tree by calculating the geometry of all elements (sizes & positioning) and placing them.
- **painting** all individual nodes.

> ##### The Premise of Virtual DOM: 
>
> Real DOM updation is a slow process (due to reflow and repainting).
{: .block-warning}

## Why Virtual DOM help
Modern browsers are efficient enough to update only the required elements in the DOM. For example, if I have two 'p' tags and I change the text in one of the p tags using a button click, then only that p tag will be updated by safari (I have verified this using paint flashing). 
Then why we still need Virtual DOM?
Virtual DOM is not magic, but it make writing WebApp easier. For example, if there are many tags you need to update when a state change, you will have a headache to figure out what tags to change and how to change. Rebuild the whole DOM can be easier, but it is slow. Virtual DOM is one way to fix this.

There are two arguments for React's Virtual DOM being easier to build WebApp (not faster):
- It updates ONLY those elements that actually need to be updated (using **diff**).
- It **batches** the updates and hence we update the real DOM only a single time. Thus the repainting is also done only once which otherwise would have been done multiple times.

Here some points need to be clear: 
- You can use DOM api to directly modify every HTML elements. This way can be more efficient. But it can be too trivial to do that in really App.
- 

## What is React Virtual DOM

React renders JSX components to the Browser DOM, but keeps a copy of the actual DOM to itself. This copy is the Virtual DOM. We can think of it as the twin brother of the real or Browser DOM. The following actions take place in React:
- React stores a copy of Browser DOM which is called Virtual DOM.
- When we make changes or add data, React creates a new Virtual DOM and compares it with the previous one.
- Comparison is done by **Diffing Algorithm**. The cool fact is all these comparisons take place in the memory and nothing is yet changed in the Browser.
- After comparing, React goes ahead and creates a new Virtual DOM having the changes. It is to be noted that as many as 200,000 virtual DOM nodes can be produced in a second.
- Then it updates the Browser DOM with the least number of changes possible without rendering the entire DOM again. This changes the efficiency of an application tremendously.

It's important to understand that virtual DOM isn't a feature. It's a means to an end, the end being **declarative**, state-driven UI development. Virtual DOM is valuable because it allows you to build apps without thinking about state transitions, with performance that is generally good enough. That means less buggy code, and more time spent on creative tasks instead of tedious ones.

### Diffing Algorithm

Some concepts used by this Diffing Algorithm are:

1. Two elements of different types will produce different trees.
2. Breadth-First Search (BFS) is applied because if a node is found as changed, it will re-render the entire subtree hence Depth First Approach is not exactly optimal.
3. When comparing two elements of the same type, keep the underlying node as same and only update changes in attributes or styles.
4. React uses optimizations so that a minimal difference can be calculated in O(N) efficiently using this Algorithm.

The Diffing Algorithm for React is Fibre
In [Dan Abramov’s Youtube Video](https://www.youtube.com/watch?v=aS41Y_eyNrU), he explained the motivation of 2 Virtual DOM trees came from The Double Buffering Technique that was used in the earlier days for Game Development.


## Svelte

Many people reckon VDOM cut down the performance. Svelte is an example to ditch VDOM. 
{% include figure.html path="assets/img/svelte-VS-react.avif" class="img-fluid rounded z-depth-1" %}

Svelte regards [Virtual DOM is pure overhead](https://svelte.dev/blog/virtual-dom-is-pure-overhead). 

Firstly, The **diffing** isn't free. Svelte believe the case in the vast majority of updates — the basic structure of the app is unchanged. It would be much more efficient if we could skip the diffing. 
> Svelte is a compiler that knows at build time how things could change in your app, rather than waiting to do the work at run time.

Secondly, the greater overhead is in the **components themselves**. 
You'd be carelessly recalculating value on every update, regardless of whether props.foo had changed:
```js
function StrawManComponent(props) {
	const value = expensivelyCalculateValue(props.foo);

	return <p>the value is {value}</p>;
}
```
Or we're generating a new array of virtual `<li>` elements — each with their own inline event handler — on every state change, regardless of whether props.items has changed:
```js
function MoreRealisticComponent(props) {
	const [selected, setSelected] = useState(null);

	return (
		<div>
			<p>Selected {selected ? selected.name : 'nothing'}</p>

			<ul>
				{props.items.map((item) => (
					<li>
						<button onClick={() => setSelected(item)}>{item.name}</button>
					</li>
				))}
			</ul>
		</div>
	);
}
```

