---
layout: post
title: React State system
date: 2023-10-15
tags: React Hook
category: React
toc:
  - name: State Rules
    subsections: 
      - name: Rules for structuring state 
      - name: Sharing State Between Components
  - name: Understand State in React
    subsections: 
      - name: State is tied to a position in the render tree
      - name: State vs Ref
      - name: State behaves as snapshot
      - name: How React update states
  - name: useState Hook
    subsections: 
      - name: Syntax
      - name: How React implement `useState` Hook
      - name: How about `setState`
      - name: Set State to a function
      - name: Updating Objects in State
      - name: Updating Arrays in State
  - name: useReducer Hook
    subsections: 
      - name: Migrating from `useState` to `useReducer`
      - name: Comparing `useState` and `useReducer` 
      - name: Writing reducers well 
  - name: FQA
    subsections: 
      - name: Why React choose Immutable
      - name: the difference between key and state
---

State is the core concept of React component. Imagine you was asked to write web-apps with JavaScript without React or other framework. Using global variables to manage the whole app states, manage update DOM, that will be your first idea to write robust app. State in React is the similar idea of it.

## State Rules

- Treat all state in React as **immutable**. This help React run very fast. 
- State behaves as a **snapshot**. Setting state does not change the state variable you already have, but instead triggers a re-render.
- State actually “lives” in React itself outside of your function. 
- When triggering a render, React calls your component, it gives you a snapshot of the state for that particular render. 
- This new snapshot of the UI with a fresh set of props and event handlers in its JSX, all calculated using the state values from that render!
- React will ignore your update if the next state is equal to the previous state, as determined by an `Object.is` comparison. 
- You can store information from previous renders, but need to use condition, and also, the logic is hard to read. try to avoid.
- When you call the `set` function of useState hook during render, React will re-render that component immediately after your component exits with a `return` statement, and before rendering the children. 
- Unlike props, state is fully private to the component declaring it. If you render the same component twice, each copy will have completely isolated state! 
- React batches state updates, it will queue all set functions and execute all set functions one by one before re-render.
- React will reset all state of a component when it's `key` prop change! That means the `initializer function` of `setState function` will run again.
- We usually use `Array.map()` to list content component in React. This require mark `key` prop for every list component. When causing re-render, `Array.map()` will re-run and recaculate all the list components. In this situation, the List component still can use `useState()` to store it's states! It just like other components, in re-render, React won't remove it and create a new component, React still keep state snapshot of the list component.

### Rules for structuring state 

When you write a component that holds some state, you’ll have to make choices about how many state variables to use and what the shape of their data should be. There are a few principles that can guide you to make better choices:

- Group related state. 
- Avoid contradictions in state.
- Avoid redundant state. If you can calculate some information from the component’s props or its existing state variables during rendering, you should not put that information into that component’s state.
- Avoid duplication in state. 
- Avoid deeply nested state. Deeply hierarchical state is not very convenient to update. When possible, prefer to structure state in a flat way.

### Sharing State Between Components

The key point is **Lifting State Up**.

For example, you want the state of two components to always change together. To do it, remove state from both of them, move it to their closest common parent, and then pass it down to them via props. Usually, you will pass some functions down as well, these let child components change the parents' state.

## Understand State in React

### State is tied to a position in the render tree 

When you give a component state, you might think the state “lives” inside the component. But the state is actually held inside React. React associates each piece of state it’s holding with the correct component by where that component sits in the render tree.

React preserves a component’s state for as long as it’s being rendered at its position in the UI tree. If it gets removed, or a different component gets rendered at the same position, React discards its state.
But **Same component at the same position preserves state**, even its CSS style change. It’s the same component at the same position, so from React’s perspective, it’s the same counter.

> Remember that it’s the position in the UI tree, NOT in the JSX markup, that matters to React! 
{: .block-warning}

#### How React think about component position

As a rule of thumb, if you want to preserve the state between re-renders, the structure of your tree needs to **match up** from one render to another. If the structure is different, the state gets destroyed because React destroys state when it removes a component from the tree.

But How React think about component structure?

- Rendering a component in a way below, React will think **different** structure for each render and destroy `<Counter>` states: 
```ts
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA &&
        <Counter person="Taylor" />
      }
      {!isPlayerA &&
        <Counter person="Sarah" />
      }
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        Next player!
      </button>
    </div>
  );
}
```
- But if you rendering the way below, React will think it is **same** structure and keep states:
```ts
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        Next player!
      </button>
    </div>
  );
}
```

> React will take `{null}` as one child! it will take a position in the render tree. 
{: .block-warning}

So React will keep `<Form />` state for every render, because it is in same position all the time.
```ts
  const [showHint, setShowHint] = useState(false);
  return showHint?(
    <div>
      <p>Hint: Your favorite city?</p>
      <Form />
    </div>
  ):(
    <div>
      {null}
      <Form />
    </div>
  );
```

#### key and position

You can Reset component state with a `key`. Keys aren’t just for lists! You can use keys to make React distinguish between any components. 

> Specifying a key tells React to use the key itself as part of the position, instead of their order within the parent. 
> Remember that keys are not globally unique. They only specify the position within the parent.
{: .block-warning}

Resetting state with a key is particularly useful when dealing with **forms**. For example, Contact Manage App, Chat App, Using person information as key to manage form will be a good idea.

Also, use key prop to Presever state even the order of components change.

#### Practice Hint
- **Don't** nest component function definitions. Because every render will create new component, and React will reset it's states.
- **How to Reset state at the same position**. By default, React preserves state of a component while it stays at the same position. Usually, this is exactly what you want, so it makes sense as the default behavior. But sometimes, you may want to reset a component’s state.
  - Rendering "different" position of the component.
  - Rendering with a `key`.
- **How to Preserve state for removed components**
  - Don't remove it, hide the component with CSS.
  - Lift the state up.
  - Use a different source in addition to React state.
- How to Preserve state when change order of a list of components. Using `key` prop!

Maybe you can refer this as React instance concept.


### key and state

When you update a `state`, the component and the child components will be **re-render**. 
But When you update the `key` prop of a component, all it's child components will be **re-initialised**.

The example below is showing that, the `childState` of the `ChildComponent` will re-initialised when the `key` of  `ReRenderTest` change.
```js
function ChildComponent () {
  const [childState, setChileState] = useState(0);
  return (
    <div className="border border-orange-500 p-2 m-2">
      <button 
        onClick={() => setChileState(current => current+1)}
        className='w-40 border border-purple-500 rounded-md'
      >
        update child state: {childState}
      </button>
    </div>
  )
}
function ReRenderTest (){
  const [otherState, setOtherState] = useState(0);
  const [key, setKey] = useState(0)

  return (
    <div key={key} className="flex flex-col gap-1 border border-orange-500 p-2 m-2">
      <button 
        onClick={() => setOtherState(current => current+1)}
        className='w-40 border border-purple-500 rounded-md'
      >
        update state: {otherState}
      </button>
      <button 
        onClick={() => setKey(current => current+1)}
        className='w-40 border border-purple-500 rounded-md'
      >
        update key: {key}
      </button>
      <ChildComponent />
    </div>
  )
}
```

### State vs Ref

State and Ref are comparable. React use them for different purpose.

- `state` and `ref` could point to anything: a string, an object, or even a function. 
- `state` and `refs` both are live outside of your component.
- `state` and `refs` both are retained by React between re-renders. 
- Mutating `state` cause re-render. Mutating `ref` won't.
- `state` works as snapshot for each render, You can't get latest state from an asynchronous operation; But `ref` won't be affected by render, you can read the latest ref anytime.
- `state` is ”Immutable” — you must use the state setting function to modify state variables to queue a re-render; `ref` is mutable, it is a **plain** JavaScript object that you can read and modify.
- You shouldn’t read (or write) the `ref.current` value during rendering. But You can read `state` any time. 

### State behaves as snapshot

States are a component’s memory. State actually “lives” in React itself (as if on a shelf!) outside of your function. When triggering a render, React calls your component, it gives you a snapshot of the state for that particular render. 

This snapshot of the UI with a fresh set of props and event handlers in its JSX, all calculated using the state values from that render!

But how to understand this component:
```js 
export default function Counter() {
  const [counter, setCounter] = useState(0);
  async function handleClick() {
    setCounter(counter + 1);
    await delay(3000);
    setCounter(counter - 1);
  }
  return (
    <>
      <h3>
        counter: {counter}
      </h3>
      <button onClick={handleClick}>
        add it     
      </button>
    </>
  );
}
function delay(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

// It will display "counter: -1" when click the button.
```

It still make sense like this: 
When you click, cause a re-render, React return a second snapshot: `counter: 1` for the new render; After 3 seconds, `setCounter(counter - 1);` executed and trigger a new render as well but this code is executed based on the old snapshot:  `counter: 0`. It still trigger a render and create a new snapshot: `counter: -1`; 
In this case, React just manage one memory for this component because it is in the same place.

> So, Don't read the latest state from an asynchronous operation, like a timeout. It is confused!
{: .block-warning}

### How React update states

- If you have many states need to be updated. React **batches** state updates. That means State updates are queued. This lets you update multiple state variables without triggering too many re-renders.
- React waits until all code in the event handlers has run before processing your state updates. 

Understand **queue** the state udpate: 
```js 
export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        setNumber(n => n + 1);
      }}>You will get 6</button>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>You will get 1</button>
    </>
  )
}
```

#### flushSync()
you can force React to update (or call 'flush') the DOM **synchronously** according to state update. 
To do this, import flushSync from react-dom and wrap the state update into a `flushSync` call:
```js 
flushSync(() => {
  setTodos([ ...todos, newTodo]);
});
listRef.current.lastChild.scrollIntoView();
```

## useState Hook

### Syntax
```js
const [state, setState] = useState(initialState);
```
useState returns an array with exactly two items:
1. The current state of this state variable, initially set to the initial state you provided.
2. The set function that lets you change it to any other value in response to interaction.

Important to Know:
1. Calling the set function does not change the current state in the already executing code.
2. State is considered read-only, When state is objects or arrays, you should replace it rather than mutate your existing objects.
3. About the initial state, React saves it once and ignores it on the next renders. So don't do this: `const [todos, setTodos] = useState(createInitialTodos());`, because React run this function every render and means nothing. But you can do this: `const [todos, setTodos] = useState(createInitialTodos);`.

### How React implement `useState` Hook

It is helpful to know How `useState` works inside React. Internally, React holds an array of state pairs for every component. It also maintains the current pair index, which is set to 0 before rendering. Each time you call useState, React gives you the next state pair and increments the index. Hooks rely on a stable call **order** on every render of the same component. 
Roughly like this:
```js
let componentHooks = [];
let currentHookIndex = 0;
function useState(initialState) {
  let pair = componentHooks[currentHookIndex];
  if (pair) {
    // This is not the first render,
    // so the state pair already exists.
    // Return it and prepare for next Hook call.
    currentHookIndex++;
    return pair;
  }

  // This is the first time we're rendering,
  // so create a state pair and store it.
  pair = [initialState, setState];

  function setState(nextState) {
    // When the user requests a state change,
    // put the new value into the pair.
    pair[0] = nextState;
    updateDOM();
  }

  // Store the pair for future renders
  // and prepare for the next Hook call.
  componentHooks[currentHookIndex] = pair;
  currentHookIndex++;
  return pair;
}
```

### How about `set function` in useState

The set function returned by `useState` lets you update the state to a different value and trigger a re-render. 

You can pass the next **state value** directly, OR a **'updater function'** that calculates it from the previous state. 
Because **React batches state updates**, it will queue all these set functions and execute all these set functions one by one before re-render. React will treat updater function differently from direct value (`baseState` is current value; `queue` is setfunctions array) :
```js
export function getFinalState(baseState, queue) {
  let finalState = baseState;

  for (let updater of queue) {
    if (typeof updater === 'function') {
      // Apply the updater function.
      finalState = updater(finalState);
    } else {
      // Replace the next state.
      finalState = updater;
    }
  }

  return finalState;
}
```

### Some Rules for `useState` Hook
- The **order** is matter! React execute the setState functions in exact order.
- The set function only updates the state variable for the next render. If you read the state variable after calling the set function, you will still get the old value that was on the screen before your call. It is Async.
- If the new value you provide is identical to the current state, as determined by an `Object.is` comparison, React will skip re-rendering the component and its children. 
- Calling the set function during rendering is only allowed from within the currently rendering component. React will discard its output and immediately attempt to render it again with the new state. This pattern is rarely needed, but you can use it to store information from the previous renders.
- In Strict Mode, React will call your updater function twice in order to help you find accidental impurities. This is development-only behavior and does not affect production.
- The initialer of useState will just run at the first time.
- When you call the `set function` of useState hook during render, React will re-render that component immediately after your component exits with a `return` statement, and before rendering the childre.

### Set State to a function

Don't do this:
```js
const [fn, setFn] = useState(someFunction);

function handleClick() {
  setFn(someOtherFunction);
}
```
Because you’re passing a function, React assumes that someFunction is an initializer function, and that someOtherFunction is an updater function, so it tries to call them and store the result. To actually store a function, you have to put `()=>` before them in both cases. Then React will store the functions you pass.
```js
const [fn, setFn] = useState(() => someFunction);

function handleClick() {
  setFn(() => someOtherFunction);
}
```

> The different between `someFunction` and `()=>someFunction` is that, executing latter will return former.

### Updating Objects in State

> ##### Why changing object state directly won't work?
> 
> Becuase React use `Object.is` to comparation, which decide whether to cause re-render.

So, the Rule is that You should Treat state as read-only.

- Copying objects with the **spread syntax**.
- Updating a nested object!
For example, update 'city' to 'NewYork':
```js
const [person, setPerson] = useState({
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
});
```
You can update it like below:
```js
setPerson({
  ...person, // Copy other fields
  artwork: { // but replace the artwork
    ...person.artwork, // with the same one
    city: 'NewYork' // but in NewYork!
  }
});
```

> Nested Objects are not really nested.  “nesting” is an inaccurate way to think about how objects behave. There are just reference which point to another object.

### Updating Arrays in State
You can use Immer plus in! 
But I prefer do it by Knowing what you are doing.
Basic Rule is that you should treat arrays in React state as read-only.

#### prefer Array methods(returns a new array): 

- adding: `concat`, `[...arr]`
- inserting: `[...arr]` together with the `slice()` method: 
```js 
const nextArtists = [
      // Items before the insertion point:
      ...artists.slice(0, insertAt),
      // New item:
      { id: nextId++, name: name },
      // Items after the insertion point:
      ...artists.slice(insertAt)
    ];
```
- removing: `filter`, `slice`
- Transforming (change some or all items of the array): `map`
- replacing: `map`
- reversing or sorting: **copy the array first**
```js
const nextList = [...list];
nextList.reverse();
```
- When updating **nested** state, you need to create copies from the point where you want to update, and all the way up to the top level.
You don't have to deep copy all the objects for every update, but you need to create copies from the point where you want to update, and all the way up to the top level. 
Using `map` and spread `...` can make it. 
```js
const initialList = [
  { id: 0, title: 'Big Bellies', seen: false },
  { id: 1, title: 'Lunar Landscape', seen: false },
  { id: 2, title: 'Terracotta Army', seen: true },
];
const [myList, setMyList] = useState(initialList);
setMyList(myList.map(artwork => {
  if (artwork.id === artworkId) {
    // Create a *new* object with changes
    return { ...artwork, seen: nextSeen };
  } else {
    // No changes
    return artwork;
  }
}));
```

#### Avoid (mutates the array):
- adding:	`push`, `unshift`
- removing:	`pop`, `shift`, `splice`
- replacing:	`splice`, `arr[i] = ... assignment`
- sorting:	`reverse`, `sort`

## useReducer Hook

`useReducer` is build on top of `useState` technicially, and it provide a **framework** to manage state. This framework is more descriptive of the user’s intent, and make code easier to understand.

The basic idea of this framework is that, concluding user's intentions into objects, called action objects, which will be dispatched to a `reducer` function. `reducer` function is a pure function which consolidate a component’s state update logic and return the next `state`.

`useReducer` is build on top of `useState`. It behavior exactly like this:
```js
import { useState } from 'react';

export function useReducer(reducer, initialState) {
  const [state, setState] = useState(initialState);

  function dispatch(action) {
    setState((s) => reducer(s, action));
  }

  return [state, dispatch];
}
```

You can declare `reducer` function outside of your component. This decreases the indentation level and can make your code easier to read.

### Migrating from `useState` to `useReducer`

1. Move from setting state to dispatching actions.
2. Write a reducer function.
3. Use the reducer from your component.

#### Move from setting state to dispatching actions

Managing state with reducers is slightly different from directly setting state. Instead of telling React “what to do” by setting state, you specify “what the user just did” by dispatching “actions” from your event handlers. 

What is dispatching actions?

Dispatching actions are normal objects. It is common to give action object a string type that describes what happened, and pass any additional information in other fields.
```ts
function handleAddTask(text) {
  dispatch({
    type: 'added',
    id: nextId++,
    text: text,
  });
}
```

#### Write a reducer function 

To reduce this complexity and keep all your logic in one easy-to-access place, you can move that state logic into a single function outside your component, called a “reducer”.

A reducer function is where you will put your state logic. It takes two arguments, the current state and the action object, and it returns the next state.

```ts
function yourReducer(state, action) {
  // return next state for React to set
}
```
React will set the state to what you return from the reducer.

> Reducer concept is come from the reducer in `Array.reduce()`. You could even use the `reduce()` method with an `initialState` and **an array of actions** to calculate the final state by passing your reducer function to it.

#### Use `useReducer`

```ts
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
```
The useReducer Hook takes two arguments:
- A reducer function
- An initial state

And it returns:
- A stateful value
- A dispatch function (to “dispatch” user actions to the reducer)

Now every time you pass an `action object` to `dispatch` function, React can update your state.

### Comparing `useState` and `useReducer` 

- **Code size**: Generally, with useState you have to write less code upfront. With useReducer, you have to write both a reducer function and dispatch actions. However, useReducer can help cut down on the code if many event handlers modify state in a similar way.
- **Readability**: useState is very easy to read when the state updates are simple. When they get more complex, they can bloat your component’s code and make it difficult to scan. In this case, useReducer lets you cleanly separate the how of update logic from the what happened of event handlers.
- **Debugging**: When you have a bug with useState, it can be difficult to tell where the state was set incorrectly, and why. With useReducer, you can add a console log into your reducer to see every state update, and why it happened (due to which action). If each action is correct, you’ll know that the mistake is in the reducer logic itself. However, you have to step through more code than with useState.
- **Testing**: A reducer is a pure function that doesn’t depend on your component. This means that you can export and test it separately in isolation. While generally it’s best to test components in a more realistic environment, for complex state update logic it can be useful to assert that your reducer returns a particular state for a particular initial state and action.
- **Personal preference**: Some people like reducers, others don’t. That’s okay. It’s a matter of preference. You can always convert between useState and useReducer back and forth: they are equivalent!

### Writing reducers well 
Keep these two tips in mind when writing reducers:

- **Reducers must be pure**. Similar to state updater functions, reducers run during rendering! (Actions are queued until the next render.) This means that reducers must be pure—same inputs always result in the same output. They should not send requests, schedule timeouts, or perform any side effects (operations that impact things outside the component). They should update objects and arrays without mutations.
- **Each action describes a single user interaction, even if that leads to multiple changes in the data**. For example, if a user presses “Reset” on a form with five fields managed by a reducer, it makes more sense to dispatch one reset_form action rather than five separate set_field actions. If you log every action in a reducer, that log should be clear enough for you to reconstruct what interactions or responses happened in what order. This helps with debugging!

## FQA

### Why React choose Immutable

- Good for Debugging: If you use console.log and don’t mutate state, your past logs won’t get clobbered by the more recent state changes. 
- Common React optimization strategies rely on skipping work if previous props or state are the same as the next ones.
- Easy for some implements, like implementing Undo/Redo, showing a history of changes, or letting the user reset a form to earlier values.
- Make Implementation simpler. And that is the reason why you can set Object as State.  

> Important concept in Immutable: 
> 'Nested' Objects are not really nested. Nesting is an inaccurate way to think about how objects behave. 
{: .block-warning}

