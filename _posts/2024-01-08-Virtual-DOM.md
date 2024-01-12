---
layout: post
title: Virtual DOM Concept
date: 2024-01-08
category: React
tags: JavaScript DOM SPA Svelte
---

## DOM rendering

Let's get to know a bit about DOM rendering first.
In traditional rendering, the Browser does the following tasks:
- The browser parses our HTML and stores it in memory as a **tree structure** of a document, which is also known as DOM (Document Object Model) or sometimes as Real DOM. DOM methods allow programmatic access to the tree. With them, you can change the document’s structure, style, or content.
- The browser uses DOM to create a render tree. **Render** Tree represents everything that will be rendered on the browser. 
- **Layout** Render Tree by calculating the geometry of all elements (sizes & positioning) and placing them.
- **painting** all individual nodes.

> ##### The Premise of Virtual DOM: 
>
> Real DOM updation is a slow process (due to reflow and repainting).
{: .block-warning}

## Why Virtual DOM can help
Modern browsers are efficient enough to update only the required elements in the DOM. For example, if I have two 'p' tags and I change the text in one of the p tags using a button click, then only that p tag will be updated by safari (I have verified this using paint flashing). 

Then why we still need Virtual DOM?
Virtual DOM is not magic, but it make writing WebApp easier. For example, if there are many tags you need to update when a state change, you will have a headache to figure out what tags to change and how to change. Rebuild the whole DOM can be easier, but it is slow process. Virtual DOM is one way to fix this.

There are two arguments for React's Virtual DOM being **easier** to build WebApp (not faster):
- It updates ONLY those elements that actually need to be updated (using **diff**).
- It **batches** the updates and hence we update the real DOM only a single time. Thus the repainting is also done only once which otherwise would have been done multiple times.

Here some points need to be clear: 
- You can drop into raw DOM operations and DOM API calls and beat React if you wanted to. This way can be more efficient. But it can be too trivial to do that in real App. 
- Batching updates is actually the basic for every WebApp. Virtual DOM just make batching is much easier for us.

## What is React Virtual DOM

React renders JSX components to the Browser DOM, but keeps a copy of the actual DOM to itself. This copy is the Virtual DOM. We can think of it as the twin brother of the real or Browser DOM. The following actions take place in React:
- React stores a copy of Browser DOM which is called Virtual DOM.
- When we make changes or add data, React creates a new Virtual DOM and compares it with the previous one.
- Comparison is done by **Diffing Algorithm**. The cool fact is all these comparisons take place in the memory and nothing is yet changed in the Browser.
- After comparing, React goes ahead and creates a new Virtual DOM having the changes. It is to be noted that as many as 200,000 virtual DOM nodes can be produced in a second.
- Then it updates the Browser DOM with the least number of changes possible without rendering the entire DOM again. This changes the efficiency of an application tremendously.

It's important to understand that virtual DOM isn't a feature. It's a means to an end, the end being **declarative**, state-driven UI development. Virtual DOM is valuable because it allows you to build apps without thinking about state transitions, with performance that is generally good enough. That means less buggy code, and more time spent on creative tasks instead of tedious ones.



### Reconciliation

To get a better understanding, we need to discuss some terminologies before discussing the whole Reconciliation process.

Reconciliation is the process of keeping 2 DOM Trees in sync by a library like ReactDOM. It is done by using **Reconciler** and a **Renderer**.

**Reconciler** uses Diffing Algorithm to find differences between Current Tree and Work in Progress Tree and sends computed changes to the Renderer.

The **Renderer** is the one that updates the app’s UI. Different devices can have different Renderers while sharing the same Reconciler.

Before React 16, React used to work on Call Stack to keep track of the program’s execution. Hence old reconciler has been given the name Stack Reconciler. 
In React 16, they created a new Reconciler from scratch which uses a new data structure called fiber. Hence it is called **Fiber Reconciler**. The main aim was to make the reconciler asynchronous and smarter by executing work on the basis of priority.

In [Dan Abramov’s Youtube Video](https://www.youtube.com/watch?v=aS41Y_eyNrU), he explained the motivation of 2 Virtual DOM trees came from The Double Buffering Technique that was used in the earlier days for Game Development.

## Svelte

Many people reckon VDOM cut down the performance. Svelte is an example to ditch VDOM. 

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

Svelte achieve a similar programming model without using virtual DOM.
{% include figure.html path="assets/img/svelte-VS-react.avif" class="img-fluid rounded z-depth-1" %}

