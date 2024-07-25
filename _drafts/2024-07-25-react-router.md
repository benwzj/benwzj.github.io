---
layout: post
title: Router in React
date: 2024-07-25
category: React
tags: React Router Remix Next.js
---

Routing is the skeleton of every web application. Many React framework come with router, like Next.js. 
But if you are using Create React App, It doesn't include page routing. And React Router is the most popular solution.

Of course, you can write your own Router if your appllication is small and simple. 
## Write your own Router


## React Router

### What is React Router
React Router is a collection of navigational components within your application. It enables the navigation among views of various components in a React Application, allows changing the browser URL, and keeps the UI in sync with the URL.

### Whyt React Router popular in the past?
In the past, Create React App (CRA) was the simplest way to bootstrap a React application. It handled and hid the complexity of setting up webpack and Babel, and when new features were added to React, `react-scripts` supported them from the beginning.

However, CRA didn't provide a router, a data fetching solution, or any of the features listed above. That wasn't the purpose of CRA. It was a simple React app starter that allowed you to use whatever libraries from the React ecosystem you liked. Many projects bootstrapped with CRA use React Router for routing, which means there are a large number of "CRA apps" that are also "React Router apps".

### How about now
These days CRA is no longer a recommended way to create React applications. And the React docs don't even recommend using it.

At the same time, Vite has risen substantially in popularity, offering a fast dev experience, optimized builds, and a rich plugin ecosystem and authoring experience.

Now The React Router team create a React Framework call Remix. You can even think `Remix = React Router + Vite`.

### The React Router package
The `react-router` package is the heart of React Router and provides all the core functionality for both `react-router-dom` and `react-router-native`.

### Basic Usage
```js
import React from "react";
import {
    BrowserRouter as Router,
    Routes,
    Route,
    Link,
    useNavigate,
    Outlet,
} from "react-router-dom";
const Home = () => {
    const navigate = useNavigate();
    return (
        <div>
            <h2>Home Page</h2>
            <button onClick={() =>
                 navigate("/contact")}>Go to Contact</button>
        </div>
    );
};
const About = () => (
    <div>
        <h2>About Page</h2>
        <nav>
            <ul>
                <li>
                    <Link to="team">Our Team</Link>
                </li>
                <li>
                    <Link to="company">Our Company</Link>
                </li>
            </ul>
        </nav>
        <Outlet />
    </div>
);
const Contact = () => <h2>Contact Page</h2>;
const Team = () => <h2>Team Page</h2>;
const Company = () => <h2>Company Page</h2>;
function App() {
    return (
        <Router>
            <nav>
                <ul>
                    <li>
                        <Link to="/">Home</Link>
                    </li>
                    <li>
                        <Link to="/about">About</Link>
                    </li>
                    <li>
                        <Link to="/contact">Contact</Link>
                    </li>
                </ul>
            </nav>
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/about" element={<About />}>
                    <Route path="team" element={<Team />} />
                    <Route path="company" element={<Company />} />
                </Route>
                <Route path="/contact" element={<Contact />} />
            </Routes>
        </Router>
    );
}
export default App;
```

## References

- [React Router website](https://reactrouter.com/)
- [Remix](https://remix.run/)