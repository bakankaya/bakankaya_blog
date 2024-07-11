---
title: Converting JavaScript to React Component
description: This is the second post.
date: 2024-07-10
tags:
  - React
  - JavaScript
  - Vite
eleventyNavigation:
  key: Converting JavaScript to React Component
  parent: Posts
---


Now that I've finished writing a weather applet. I want to use it in other webpages with more functionality.
Since I have a hammer(React), now everything looks like a nail(a react component).

Joke aside, this feels like a good oppurtunity to remember the differences between vanilla js and react, to see the main differences and etc.

This is a small project to work on, there is one or two function to be translate into jsx. And when I manage to make it a reusable component, I can use it in a meaningful way.

I am going to write it step by step. So lets start with basics.


### Step 1 - Creating a React app

I'll create a react prject with vite with following prompts:

```node
npm create vite@latest 
    //choosing react
    //then in the folder:
npm install
npm run dev
 ```

Now that I've created a react project in the latest fashionable and easy way, lets clear out everything we don't need.

I've cleared out everything from /assets and /public folders. in the index.html file, I've changed the logo and title content.

Then I cleared /src file to leave with these three;

to `main.jsx`;
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```
to `index.css`;
```css
:root {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.5;
  font-weight: 400;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```
and to `App.jsx`;
```jsx
import React from "react";

function App() {

  return (
    <>

    </>
  )
}

export default App
```

Now we can begin to write our app.

### Step 2 - migrating HTML elements and CSS

React needs one parent element to render. This can be done with a `<></>` fragment when necessary but we already made our whole app in single div so, this wasn't a problem for us.

in React, we need to use `className` instead of `class`. So every class property needs to change `className`.

on first glance last thing we need to correct is the `onClick="window.location.reload()` part. This will translate into a function so erase this with the inital `value=" "`  from our input and button elements.

For CSS, I just copied our css file to src file, named it same with the App.css to easily identify with our app and import it to App.jsx with this line:

```jsx
import './App.css';
```
Not in this project but in later on, I might handle dependencies like icons and fonts, be careful to import them accordingly to your app.

So far so good. No major problem.

### Step 3 - Getting the Date

First part of our js file was to get the days date, convert it to a readible format and instert it to DOM.
```js
const date = new Date();
const optionsDate = {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric',
//    dayPeriod: 'short'
  };
const optionsTime = {
    timeStyle: 'short'
}

document.querySelector(".date span").textContent = date.toLocaleDateString('en-EN', optionsDate);
```

I'm going to add it as it is to the start of our App function with one change:

last line was for JS and all `document.querySelector` part won't work in react. Because nothing triggers it.(I guess?) So instead i will make it a variable named 'todaysDate' and in our return part where we render the page I'm going to call the variable with bracelets.

```js
const todaysDate = date.toLocaleDateString('en-EN', optionsDate);

return (
    <div className="weather-thingy">
      <div className="date">
      <span>{todaysDate}</span>
      </div>
```
I could have made another function to get the date and call it like a HTML property instead of call it as variable. ie. `<TodaysDate />`, this is the common react way, to make it a component. but I dont need another component. I want to make the weather app a standalone component.

Now our getting the date part is working, and we get to show using a variable in react.

### Step 4 - Implementing useRef to the API call

Now the whole `fetch()` part is working fine. But we have problem at getting the city name for our API call. Because initally we are reading from DOM, and use it as a variable. But now, react uses Virtual DOM and because of that.
```js
const cityQuery = document.querySelector(".location input");
const city = cityQuery.value;
```
Part is not reading any value. while it is still possible to use it, react encourages the usage of `useRef`. So after importing the `useRef` I changed the following lines:
```jsx
const cityQuery = useRef('ankara');
const city = cityQuery.current;

...

return (

...

<input type="text" placeholder="Enter city" ref={cityQuery} />
)

```
instead of reading the value, we are referancing the input.

With the inital value, before making a request it still returns a city name.

and in our city variable, which we use in URL, instead of value, we are using `.current` to read the initail value, or the changed value.

### Step 5 - Search Button and changing the refresh

On our JS app, we simply overwrite the default input value then refresh the page on HTML with
```js
<button onClick="window.location.reload()">Search</button>
```
But it is an inefficient hack at best. And not a reasonable way to do things in react.

*to be continued...*