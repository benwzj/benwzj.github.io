---
layout: post
title: Understand React Server Component
date: 2024-04-04
category: React
tags: React Next.js Router Rendering
toc: 
  - name: Foundational web concepts
  - name: React Server Components
  - name: Client Components
  - name: Composition Patterns
    subsections:
      - name: Server Component Patterns
      - name: Client Components Patterns
      - name: Interleaving Patterns
  - name: FQA
---

When we talk about Server Components and Client Components, it is from the view of Rendering. 

Rendering converts the code you write into user interfaces. React and Next.js allow you to create hybrid web applications where parts of your code can be rendered on the server or the client.

In Next.js App Route, all components in the App Router are Server Components **by default**! 

Router is the skeleton of the whole Next.js Application. It can decide HOW to **render** Server Components and Client Components.

## Foundational web concepts

- The **Environments** your application code can be executed in: the server and the client.
- The **Request-Response Lifecycle** that's initiated when a user visits or interacts with your application.
- The **Network Boundary** that separates server and client code.

### Rendering Environments

There are two environments where web applications can be rendered: the client and the server.

Historically, developers had to use different languages (e.g. JavaScript, PHP) and frameworks when writing code for the server and the client. With React, developers can use the same language (JavaScript), and the same framework (e.g. Next.js or your framework of choice). This flexibility allows you to seamlessly write code for both environments without context switching.

However, each environment has its own set of capabilities and constraints. Therefore, the code you write for the server and the client is not always the same. There are certain operations (e.g. data fetching or managing user state) that are better suited for one environment over the other.

### Network Boundary

In web development, the Network Boundary is a conceptual line that separates the different environments. For example, the client and the server, or the server and the data store.

In React, you choose where to place the client-server network boundary wherever it makes the most sense.

Behind the scenes, the work is split into two parts: the client module graph and the server module graph. The server module graph contains all the components that are rendered on the server, and the client module graph contains all components that are rendered on the client.

It may be helpful to think about module graphs as a visual representation of how files in your application depend on each other.

You can use the React `"use client"` convention to define the boundary. There's also a `"use server"` convention, which tells React to do some computational work on the server.

### Building Hybrid Applications

When working in these environments, it's helpful to think of the flow of the code in your application as **unidirectional**. If you need to access the server from the client, you send a new request to the server rather than re-use the same request. This makes it easier to understand where to render your components and where to place the Network Boundary.

In practice, this model encourages developers to think about what they want to execute on the server first, before sending the result to the client and making the application interactive.

## React Server Components


### Server Rendering
React Server Components allow you to write UI that can be rendered and optionally cached on the server. 
In short, Server Components allow you to do **Server Rendering**.

#### Benefits of Server Rendering
- Data Fetching: Server Components allow you to move data fetching to the server.
- Security: Server Components allow you to keep sensitive data and logic on the server.
- Caching: By rendering on the server, the result can be cached and reused on subsequent requests and across users. 
- Performance: Server Components give you additional tools to optimize performance.
- Initial Page Load and First Contentful Paint (FCP).
- Search Engine Optimization and Social Network Shareability: The rendered HTML can be used by search engine bots.
- Streaming: Server Components allow you to split the rendering work into chunks and stream them to the client as they become ready. This allows the user to see parts of the page earlier without having to wait for the entire page to be rendered on the server.

### How are Server Components rendered in Next.js

Next.js uses Server Components by default. This allows you to automatically implement server rendering with no additional configuration, and you can opt into using Client Components when needed.

On the server, Next.js uses React's APIs to orchestrate rendering. The rendering work is split into chunks: 
- by individual route segments and 
- Suspense Boundaries.

Each chunk is rendered in two steps:
1. React renders Server Components into a special data format called the React Server Component Payload (**RSC Payload**).
2. Next.js uses the RSC Payload and Client Component JavaScript instructions to render HTML on the server.

Then, on the client:
1. The HTML is used to immediately show a fast non-interactive preview of the route - this is for the initial page load only.
2. The RSC Payload is used to reconcile the Client and Server Component trees, and update the DOM.
3. The JavaScript instructions are used to **hydrate** Client Components and make the application interactive.

### What is Hydration

In React, “hydration” is how React “attaches” to existing HTML that was already rendered by React in a server environment. During hydration, React will attempt to attach event listeners to the existing markup and take over rendering the app on the client.

For example, Call `hydrate` to attach a React component into a server-rendered browser DOM node:
```js
import { hydrate } from 'react-dom';
hydrate(<App />, document.getElementById('root'));
```

### What is the React Server Component Payload (RSC Payload)?

The RSC Payload is a compact binary representation of the rendered React Server Components tree. It's used by React on the client to update the browser's DOM. The RSC Payload contains:
- The rendered result of Server Components
- Placeholders for where Client Components should be rendered and references to their JavaScript files
- Any props passed from a Server Component to a Client Component

### Server rendering strategies
In Next.js, the rendering work is further split by route segments to enable streaming and partial rendering.

There are three subsets of server rendering: Static, Dynamic, and Streaming.

#### Static Rendering (Default)
With Static Rendering, routes are rendered at build time, or in the background after data revalidation. The result is cached and can be pushed to a Content Delivery Network (CDN). This optimization allows you to share the result of the rendering work between users and server requests.

#### Dynamic Rendering

With Dynamic Rendering, routes are rendered for each user at request time.

Dynamic rendering is useful when a route has data that is personalized to the user or has information that can only be known at request time, such as cookies or the URL's search params.

In most websites, routes are not fully static or fully dynamic - it's a spectrum. 

In Next.js, you can have dynamically rendered routes that have both cached and uncached data. This is because the RSC Payload and data are cached separately. This allows you to opt into dynamic rendering without worrying about the performance impact of fetching all the data at request time.

During rendering, if a `dynamic function` or `uncached data request` is discovered, Next.js will switch to dynamically rendering the whole route.  

In Next.js, these dynamic functions are:
- cookies() and headers(): Using these in a Server Component will opt the whole route into dynamic rendering at request time.
- searchParams: Using the searchParams prop on a Page will opt the page into dynamic rendering at request time.

#### Streaming

Streaming enables you to progressively render UI from the server. Work is split into chunks and streamed to the client as it becomes ready. This allows the user to see parts of the page immediately, before the entire content has finished rendering.

Streaming is built into the Next.js App Router by default. This helps improve both the initial page loading performance, as well as UI that depends on slower data fetches that would block rendering the whole route.

### Use Server component

- You don't need to use `"use server"` to write Server component. Just ust `"use server"` to export server atction.

## Client Components

Client Components allow you to write interactive UI that is prerendered on the server and can use client JavaScript to run in the browser.

### Client Rendering
Benefits of Client Rendering
- Interactivity: Client Components can use state, effects, and event listeners, meaning they can provide immediate feedback to the user and update the UI.
- Browser APIs: Client Components have access to browser APIs, like geolocation or localStorage.

### Using Client Components
To use Client Components, you can add the React `"use client"` directive at the top of a file, above your `imports`.

`"use client"` is used to declare a boundary between a Server and Client Component modules. This means that by defining a `"use client"` in a file, all other modules imported into it, including child components, are considered part of the client bundle.

You can define multiple `"use client"` **entry points** in your React Component tree. This allows you to split your application into multiple client bundles.

However, `"use client"` doesn't need to be defined in every component that needs to be rendered on the client. For example, when you import a component into a client component, that component will be treated as client component. 
Once you define the boundary, all child components and modules imported into it are considered part of the client bundle.

> All components are considered as Server components first until you use `"use client"` to define the boundary. You should have a clear mind when you define the boundary because the client components are the less the better.
{: .block-warning}

#### Recap

- Once you define the boundary, all child components and modules **imported** into it are considered part of the client bundle. But Server component can live inside Client component as **children**. The classic example is that context component, which can be client component, can wrap server component. 
- All hooks, state management (Context API, Zustand, Redux) just use in Client components.
- `async/await `is not support in client components, only in server components in Next.js.
- client components don't just run in the client. They will run once in the server for the initial rendering! So if you visit Browser API, like `localStorage`, you may get errors. So it is better to use code like this: `if (typeof(window !=== undefined)) { window.localStorage()...}` to protect it. React won't run hook in initial rendering because React know that things.
- In server component, if you use a component which use Hooks or Browser API, and that component don't use `"use client"`, it will cause errors. In this case, you just need to add `'use client'`. If that component is 3rd lib, you can wrap it in a separate component and add `'use client'`.

### How are Client Components Rendered

There can be two ways:
- Full page load: To optimize the initial page load, Next.js will use React's APIs to render a static HTML preview on the server for both Client and Server Components. And then React will do a process which is called **hydration**.
- Subsequent Navigations: Client Components are rendered entirely on the client, without the server-rendered HTML.

### Going back to the Server Environment

Sometimes, after you've declared the "use client" boundary, you may want to go back to the server environment.

You can keep code on the server even though it's theoretically nested inside Client Components by **interleaving** Client and Server Components and Server Actions. 

## Composition Patterns

Need to have a clear mind what server component, client component should do, and can do!
Use Server Component when:
- Fetch data		
- Access backend resources (directly)		
- Keep sensitive information on the server (access tokens, API keys, etc)		
- Keep large dependencies on the server / Reduce client-side JavaScript

Use Client Component when:
- Add interactivity and event listeners (onClick(), onChange(), etc)		
- Use State and Lifecycle Effects (useState(), useReducer(), useEffect(), etc)		
- Use browser-only APIs		
- Use custom hooks that depend on state, effects, or browser-only APIs

### Server Component Patterns

Here are some common patterns when working with Server Components:

#### Sharing data between components

Instead of using React Context or passing data as props, you can use `fetch` or React's `cache` function to fetch the same data in the components. React extends `fetch` to automatically memoize data requests.

#### Keeping Server-only Code out of the Client Environment

You may keep some module just only available for server component. For example, the component is using some sensitive data, like `API_KEY`, to fetch data. You can use `import 'server-only'` to give other developers a build-time error if they ever accidentally import one of these modules into a Client Component.

#### Using Third-party Packages and Providers

When you use Third-party Packages in your server component, and these Packages are using some Client only API, you will get some error. The simply way to fix this error is telling Next.js that, this part of code is render as client component by wrap it into a file with `"use client"`. 

When use Context Providers in your server component, it is same way to deal with it: mark it as a Client Component:
Wrap these Providers into a file with `"use client"`.

### Client Components Patterns

To reduce the Client JavaScript bundle size, Next.js recommend moving Client Components down your component tree.
That means try to keep all your components as server components if it can be.

#### Passing props from Server to Client Components (Serialization)
If you fetch data in a Server Component, you may want to pass data down as props to Client Components. Props passed from the Server to Client Components need to be serializable by React.

If your Client Components depend on data that is not serializable, you can fetch data on the client with a third party library or on the server via a Route Handler.

### Interleaving Patterns

You should visualize your UI as a tree of components. Starting with the root layout, which is a Server Component, you can then render certain subtrees of components on the client by adding the "use client" directive.

Within those client subtrees, you can still nest Server Components or call Server Actions, however there are some things to keep in mind:
- During a request-response lifecycle, your code moves from the server to the client. If you need to access data or resources on the server while on the client, you'll be making a new request to the server - not switching back and forth.
- When a new request is made to the server, all Server Components are rendered first, including those nested inside Client Components. The rendered result (RSC Payload) will contain references to the locations of Client Components. Then, on the client, React uses the RSC Payload to reconcile Server and Client Components into a single tree.
- Since Client Components are rendered after Server Components, you cannot import a Server Component into a Client Component module (since it would require a new request back to the server). Instead, you can pass a Server Component as props to a Client Component.

> The pattern of "lifting content up" has been used to avoid re-rendering a nested child component when a parent component re-renders.
{: .block-warning}

## FQA

- How do Server and Client component communicate? What kind of protocal? and How?
- How do React know a component is Server component or Client component?
- How do Next.js know what codes need to be bundle and send to browser when Client and Server Components are interleaving together? 
- How do React and Next.js connect together?

### Update Server Component after data has been changed

The only way to update a Server Component is to reload the page. As it's sent to the browser as static HTML without any JavaScript attached to it to have interactivity.

To reload the page while keeping client side states, you could use `router.refresh()`, where router is the returned value by `useRouter()`. 

Server component:
```js
// app/page.tsx

import Todo from "./todo";
async function getTodos() {
  const res = await fetch("https://api.example.com/todos", { cache: 'no-store' });
  const todos = await res.json();
  return todos;
}

export default async function Page() {
  const todos = await getTodos();
  return (
    <ul>
      {todos.map((todo) => (
        <Todo key={todo.id} {...todo} />
      ))}
    </ul>
  );
}
```
client component:
{% raw %}
```js
// app/todo.tsx
"use client";

import { useRouter } from 'next/navigation';
import { useState, useTransition } from 'react';

export default function Todo(todo) {
  const router = useRouter();
  const [isPending, startTransition] = useTransition();
  const [isFetching, setIsFetching] = useState(false);

  // Create inline loading UI
  const isMutating = isFetching || isPending;

  async function handleChange() {
    setIsFetching(true);
    // Mutate external data source
    await fetch(`https://api.example.com/todo/${todo.id}`, {
      method: 'PUT',
      body: JSON.stringify({ completed: !todo.completed }),
    });
    setIsFetching(false);

    startTransition(() => {
      // Refresh the current route and fetch new data from the server without
      // losing client-side browser or React state.
      router.refresh();
    });
  }

  return (
    <li style={{ opacity: !isMutating ? 1 : 0.7 }}>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={handleChange}
        disabled={isPending}
      />
      {todo.title}
    </li>
  );
}
```
{% endraw %}

