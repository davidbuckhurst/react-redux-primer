# React

React is at least the following things
- a framework to abstract JS DOM interactions (through APIs like `document.getElementById` and `document.createElement`)
- a framework that utilises [JSX](https://jsx.github.io/) to make the javascript you write look more like HTML
- a framework that adopts a uni-directional data flow to make life simple

It takes the tedium out of creating HTML in your JS, and provides a whole heap of benefits...

## Step 1 - JS to JSX and back again (and introducing React)

- Start this step of the tutorial **from the root of this repo**
  - ```STEP=react npm start```
  - This will start (via npm) the Webpack dev-server, and point it at the `react.js` script in this folder
  - It will output `bundle.js` which is referenced in the `index.html` document in the root of the repo
  - Note that all the tutorial steps from now on use the same HTML document!
- Open `react.js` in your editor, observe the nasty vanilla JS needed to create the desired HTML markup

Now it's time to rewrite that native JS into JSX, and let React render it to the DOM for you
- Uncomment the require statements at the start of `react.js` (Webpack will take care of bundling these into your app!)
- Uncomment the code block at the end of `react.js`
- Start writing some [JSX](https://facebook.github.io/react/docs/introducing-jsx.html) to replace the creation of DOM elements in native JS
  - An example of a JSX statement is `<div>Hi there!</div>`
  - Observe the Webpack dev server output! *Module parse failed: You may need an appropriate loader to handle this file type.*

JSX is not real code! If we try and execute in our browser it will fail. We need to *transpile* it into normal JS that the browser understands
- Uncomment the *module* key in the Webpack config to turn on JSX transpilation
  - This uses the [babel-loader](https://github.com/babel/babel-loader) Webpack plugin to perform the transpilation
- restart the Webpack dev server
- use your dev tools to see how your JSX markup has been transformed into real code (generally into `React.createElement()` statements)

Now finish knocking up the target HTML in JSX/React...

## Step 2 - Passing props

Those JSX statements create React elements, and React comes with some elements built in already (`<div>`, `<img>`, etc). You can also define your own "components" - the simplest (but not the *only* way) is to define a javascript function that returns **a single (JSX) component**:

```javascript
function MyLabel (props) {
  return <div><span>{props.text}</span></div>
}
```

Here, `props` is an object passed by React to your function, with keys/values equal to the *attributes* given when you use your component:

```javascript
<MyLabel text="this is the label text" />

// or using variables
const youCanUseVariablesToo = "this is also some label text"
<MyLabel text={youCanUseVariablesToo} />
```

- Edit react.js and play around with declaring your own components and passing them props

## Step 3 - Adding styles

React elements can have [inline styling added](https://facebook.github.io/react/docs/dom-elements.html#style) through the special `style` attribute

```javascript
const styling = {
  fontSize: 20
}
<div style={styling}>this is the label text</div>
```

*Inline styling works surprisingly nicely, especially given how easy it becomes to scope your styles to your components (compared to using CSS)*

Alternatively, styling via CSS is achieved by
- Add classes to your React elements via the `className` attribute
- Write your CSS using class based selectors (I like [BEM](http://getbem.com/introduction/))

Making your CSS available by either:
- adding `<style>` or `<link>` tags to your HTML document
- `require`ing your CSS files into your JS and letting Webpack bundle it into your build!
  - this requires some extra [*Webpack loaders* to handle the CSS](https://webpack.github.io/docs/stylesheets.html)

**Go on, add some styling to your app!**

## Step 4 - Triggering actions

React elements support attributes, such as `onClick`, that can be assigned [functions to handle events](https://facebook.github.io/react/docs/handling-events.html)

- Bring in the `Clickable` component (from `clickable.js`) to your app and click it!
- Note how this uses the ES6 [class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) syntax to define a React Component
- Note how the action can be passed in as a prop, try it!

## Step 5 - State

Previously our React components where simple functions that were passed props and rendered themselves. These are **super simple** to reason about, but limited in what they can do - they have no state!

### Task
Turn `Clickable` into a simple counter that displays the number of times it has been clicked
- set some initial state in the constructor via `this.state = {}`
  - note your components state is just a simple object
- access that state in your render function via `this.state` (the same way you access `this.props`)
- update that state in your action handler
  - **DO NOT MUTATE `this.state` directly**
  - instead use pass an object with any updated state to `this.setState()`

What's super nice here is **React will take care of re-rendering your component after a state change!**. Make sure you read the official docs on [using React component state correctly](https://facebook.github.io/react/docs/state-and-lifecycle.html#using-state-correctly)

*Declaring your Components as classes also allows you to hook into other [React lifecycle events](https://facebook.github.io/react/docs/react-component.html) which is useful for optimizations (e.g. avoiding re-renders) and other advanced usage*

## Step 6 - Uni-directional flow

![React uni-directional data flow diagram](https://rawgit.com/crosslandwa/react-redux-primer/master/react/ReactUnidirectional.svg)

Within a hierarchy of React components
- Starting with some state, React **renders** all your components (passing that state via props down through your component hierarchy)
- When components are interacted with **actions** are invoked
- These actions call **setState** which merges any passed state with that component's current state
- In response to setState being called, React **re-renders** your component (and it's children) with the new state

### Task
Build a UI that:
- displays a row of four "Tiles", each containing a name, that sit over a "Watching Bar"
- the Watching Bar should initially contain the text "You are watching: nothing"
- when you click the Tiles, update the Watching Bar to read "You are watching: {name}"
- for bonus points, have some sort of visual indicator to highlight the currently selected Tile

An example solution to this is given in react-solution.js
 - see it with `STEP=react-solution npm start`
 - it uses CSS flex box for the layout. I found [Flexbox Froggy](http://flexboxfroggy.com/) a good resource for learning flex
 - note how the solution [lifts state up](https://facebook.github.io/react/docs/lifting-state-up.html) out of the Tile/WatchingBar components

*The key learning from this task is that when components start influencing the state of other components, things start to become complicated...*

## More info

The official [React Quick Start](https://facebook.github.io/react/docs/hello-world.html) is really good
