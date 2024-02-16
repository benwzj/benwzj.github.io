---
layout: post
title: React State system
date: 2023-10-15
tags: React Hook
category: React
toc:
  - name: State Rules
  - name: Understand State in React
    subsections: 
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
  - name: FQA
---

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

## Understand State in React

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

### How about `setState`

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

- The **order** is matter! React execute the setState functions in exact order.
- The set function only updates the state variable for the next render. If you read the state variable after calling the set function, you will still get the old value that was on the screen before your call. It is Async.
- If the new value you provide is identical to the current state, as determined by an `Object.is` comparison, React will skip re-rendering the component and its children. 

- Calling the set function during rendering is only allowed from within the currently rendering component. React will discard its output and immediately attempt to render it again with the new state. This pattern is rarely needed, but you can use it to store information from the previous renders.
- In Strict Mode, React will call your updater function twice in order to help you find accidental impurities. This is development-only behavior and does not affect production.

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

> Why changing object state directly won't work?
Becuase React use `Object.is` to comparation, which decide whether to cause re-render.

So, the Rule is that You should Treat state as read-only.

- Copying objects with the **spread syntax**.
- Updating a nested object!
For example, update city to NewYork:
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
You can update like below:
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

prefer Array methods(returns a new array): 

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

Avoid (mutates the array):
- adding:	`push`, `unshift`
- removing:	`pop`, `shift`, `splice`
- replacing:	`splice`, `arr[i] = ... assignment`
- sorting:	`reverse`, `sort`


## FQA

### Why React choose Immutable

- Good for Debugging: If you use console.log and don’t mutate state, your past logs won’t get clobbered by the more recent state changes. 
- Common React optimization strategies rely on skipping work if previous props or state are the same as the next ones.
- Easy for some implements, like implementing Undo/Redo, showing a history of changes, or letting the user reset a form to earlier values.
- Make Implementation simpler. And that is the reason why you can set Object as State.  

> Important concept in Immutable: 
> 'Nested' Objects are not really nested. Nesting is an inaccurate way to think about how objects behave. 
{: .block-warning}


### Do React have component instance concept? When placing a component in different place, all of them keep their own states. That means React manage defferent instances of the component.
