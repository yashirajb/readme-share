![GA-Logo](https://camo.githubusercontent.com/6ce15b81c1f06d716d753a61f5db22375fa684da/68747470733a2f2f67612d646173682e73332e616d617a6f6e6177732e636f6d2f70726f64756374696f6e2f6173736574732f6c6f676f2d39663838616536633963333837313639306533333238306663663535376633332e706e67) 

# Intro to React.js: Part 2



### Learning Objectives

* Pass in data to a React component via `props`.
* Modify the `state` of a React component through events using `setState`.
* Describe the setup of "downward data flow" that will allow components to share information and maintain a single idea about the state of the app.

### Intro

Before, we learned that React is a powerful and efficient library that emphasized performance and reusability of _components_, and that it utilizes _JSX_ for these components. This is what we had for our `Hello` component:

```js
// bring in React and Component instance from react
import React, {Component} from 'react'

// define our Hello component
class Hello extends Component {
  // what should the component render
  render () {
    // Make sure to return some UI
    return (
      <h1>Hello World!</h1>
    )
  }
}

export default Hello
```

Now we'll see what we can do to make this more practical, and introduce the React concepts of `props` and `state`.

### Hello World: A Little Dynamic

Our `Hello` component isn't too helpful. Let's make it more interesting.
* Rather than simply display "Hello world", let's display a greeting to the user.
* So the question is, how do we feed a name to our `Hello` component without hardcoding it into our render method?

First, we pass in data wherever we are rendering our component, in this case in `src/index.js`:

```js
import ReactDOM from `react-dom`
import Hello from './App.js'

ReactDOM.render(
  <Hello name={"Nick"} />,
  document.getElementById('root')
)
```

Then in our component definition, we have a reference to that data via as a property on the `props` object...

```js
class Hello extends Component {
  render () {
    return (
      <h1>Hello {this.props.name}</h1>
    )
  }
}
```

In the above example, we replaced `world` with `{this.props.name}`.

&#x1F535; **Activity**
```
- Change your own React App to render a couple of variables dynamically. 
- Try out a few data types!
- Share any comments or questions you have in a thread below
- 10 min
``` 

#### Review: What is `this`?
![](https://78.media.tumblr.com/421b9294f4f9505f214f0666c9c8a15b/tumblr_mxwnmkiwuh1t6o9c1o1_500.gif)

#### What are `.props`?

Properties! Every component has `.props`
* Properties are immutable. That is, they cannot be changed while your program is running. Like `const`!
* We define properties in development and pass them in as attributes to the JSX element in our `.render` method.

First we can pass multiple properties to our component when its rendered in `src/index.js`..

```js
import ReactDOM from `react-dom`
import Hello from './App.js'

ReactDOM.render(
  <Hello name={"Nick"} age={24} />,
  document.getElementById('root')
)
```

Then in our component definition we have access to both values...

```js
class Hello extends Component {
  render () {
    return (
      <div>
        <h1>Hello {this.props.name}</h1>
        <p>You are {this.props.age} years old</p>
      <div>
    )
  }
}

```

> **NOTE:** The return statement in `render` can only return one DOM element. You can, however, place multiple elements within a parent DOM element, like we do in the previous example with `<div>`.



## State

![](https://i.imgur.com/IqrvQjs.gif)

So we know about React properties, and how they relate to our component's data.
* The thing is, `props` represent data that will be the same for the entire life of a given component. What about data in our application that may change depending on user action?
* That's where `state` comes in...

Values stored in a component's state are mutable attributes.
* Like properties, we can access state values, `val` for example, using `this.state.val`
* Setting up and modifying state is not as straightforward as properties. It involves explicitly declaring the mutation, and then defining methods to define how to update our state....

Lets implement state in our earlier `Hello` example by incorporating a counter into our greeting.

```js
class Hello extends Component {
  // when our component is initialized,
  // our constructor function is called
  constructor (props) {
    // make call to parent class' (Component) constructor
    super()
    // define an initial state
    this.state = {
      counter: 0 // initialize this.state.counter to be 0
    }
  }
  render () {
    return (
      <div>
        <h1>Hello {this.props.name}</h1>
        <p>You are {this.props.age} years old</p>
        <p>The initial count is {this.state.counter}
        </p>
      </div>
    )
  }
}
```

Ok, we set an initial state. But in order to change the state, we need to set up some sort of trigger event.

<details>
  <summary><strong>
    Let's do that via a button click event. Where do you think should we initialize that button click event?
  </strong></summary>

  > Inside the `JSX` of our return value! In Hello `render` method we can instantiate our event listeners.

</details>


```js
class Hello extends Component {
  constructor (props) {
    super()
    this.state = {
      counter: 0
    }
  }
  handleClick = (e) => {
    // setState is inherited from the Component class
    this.setState({
      counter: this.state.counter + 1
    })
  }
  render () {
    // can only return one top-level element
    return (
      <div>
        <h1>Hello {this.props.name}</h1>
        <p>You are {this.props.age} years old</p>
        <p>The initial count is {this.state.counter}
        </p>
        <button onClick={this.handleClick}>click me!</button>
      </div>
    )
  }
}
```

> Take a closer look at how this event is implemented. We use an attribute called `onClick` to define the behavior as to what happens when we click this particular button. As it's value, we're passing our handleClick, a function defined on this component.

Whenever we run `.setState`, our component does a "diff", comparing the Virtual DOM node with the updated state to the current DOM. It only replaces the current DOM with parts that have changed.

&#x1F535; **Activity**
```
- Create your own version of the code above in your local repo
- Share any comments or questions you have in a thread below
- 10 min
``` 

## Structuring an application

Imagine building a clone of YouTube in React. You might structure components something like this:

![image](https://user-images.githubusercontent.com/6520345/28842533-dd587240-76b2-11e7-966c-6f1eb402b4e7.png)

This structure could be reenvisioned as a "family tree" like this one:

![image](https://user-images.githubusercontent.com/6520345/28844948-3d1033fa-76bb-11e7-8958-d841a0671d2c.png)

Take some time to identify the necessary flow of data. Where does the information come into the application? What information do specific components need to share?


#### Downward data flow

In React, every component will have props. We can use props to take in data from "parent" components. 

Because state can change, we want to have as few components holding their state as possible. Only the most parent component (Container Component) should be responsible for fetching data and holding it in its state. That parent component (Container) should then distribute that data downward to its child components (often presentational components).

This is sensical. For example, with the video list and video list items... 

Probably the video list is coming from something like an AJAX call and is structured as an array of video objects. So the list takes this array of items, and iterates through it, passing each item to the VideoListItem component. THus passing the data downward into the nested components from the outer components. 

#### How data flows upward in React

It kind of doesn't. If you need to grab data from a sub-component, what you will do is pass a method down to the componenet. The result of that method will be accesible to the parent component that passed down the method. 

We'll dig into this more later! It sounds complex now, but its actually much simpler than what came in prior frameworks, which was called "Two way data binding". That was easier to use in some ways, but MUCH more complex behind the scenes and allowed for some of the biggest problems of AngularJS, Google's MVC framework's first iteration. Angular 2+ have adopted something similar to React's way of handling data flow. 

## Closing

React is a powerful web framework that allows fast rendering and is a front-end tool. It works mainly in the "views" layer. It is meant to maintain readability, reusability, and performance.

### Additional Reading

* [Tyler McGinnis' React.js Program](http://www.reactjsprogram.com/)
* [Raw React: No JSX, Webpack, ES6, etc.](http://jamesknelson.com/learn-raw-react-no-jsx-flux-es6-webpack/)
* [Integrating React with Backbone](https://blog.engineyard.com/2015/integrating-react-with-backbone)
* [React Tic-Tac-Toe (by Jesse Shawl)](https://github.com/jshawl/react-tic-tac-toe)


### What's Next?

* [Router](https://github.com/reactjs/react-router)
* [API/Axios](https://www.npmjs.com/package/axios)
* [Events](https://facebook.github.io/react/tips/dom-event-listeners.html)
* [Forms](https://facebook.github.io/react/docs/forms.html)
