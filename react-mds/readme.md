
## React 

React is a library. It is used to craft modern UI and create views for the front-end in web, client, and native applications. 

React was born out of FB's frustration with the traditional MVC model---re-rendering one thing meant re-rendering most of the page, so processing power and user experience was negatively affected causing glitchyness and lagginess. 

React can be thought of as the VIEWS layer. It's the visual template the user sees, populated with data from our models. React aims to just use data to render a UI, enabling it to co-exist with other Javascript frameworks. The other frameworks can handle models and controllers, and React can sort out the views. 



React overrides JQuery because it provides: 

1. A solution that has a built-in code organization strategy
1. A solution that has clean, elegant templating 


### Code Organization Strategy: 

React seperates everything into components. EVERYTHING is a component in React---even a comment. A comment can actually be nested inside a Post component. For templating, React uses JSX.

Here is what Craisglist's site looks like when the visual "components" of the website have been identified. 

https://maketea.co.uk/images/2014-03-05-robust-web-apps-with-react-part-1/wireframe_deconstructed.png

### Components

Components are functional elements that take in data and produce a dynamic UI.

Web Apps in React are organized according to small, reusable components that define their own content, presentation, and behavior.

This is what a simple component looks like: 


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

##### `class Hello`
This is the component we're creating. In this example, we are creating a "Hello" component.

##### `extends Component`
This is the React library class we inherit from to create our component definition.

##### `render()`
Every component has, at minimum, a render method. It generates a **Virtual DOM** node that will be added to the actual DOM.
* Looks just like a regular ol' DOM node, but it's not yet attached to the DOM.

##### `export default Hello`
This exposes the Hello class to other files which import from the App.js file. The `default` keyword means that any import that's name doesn't match a named export will default to this. Only one default is allowed per file.

And you don't need quotes around the h1 tag since JSX is a templating language that looks just like Javascript. Oh, and React can actually be written without JSX: 

http://blog.yld.io/2015/06/10/getting-started-with-react-and-node-js/#.V8eDk5MrJP


So, what's a virtual DOM node?

The virtual DOM is one of the major innovations of React, and it's a JS representation of the actual DOM. Because of this, React can keep track of changes in the actual DOM by COMPARING different instances of the virtual DOM. Then, it isolates the changes and only updates the necessary changes to the actual DOM. Here, React saves on processing poweer since it only makes necessary changes as opposed to re-rendering an entire view. Chack out this virtual DOM diagram and video on the virtual DOM below: 

https://docs.google.com/drawings/d/11ugBTwDkqn6p2n5Fkps1p3Elp8ZToIRzXzvM4LJMYaU/pub?w=543&h=229

https://www.youtube.com/watch?v=-DX3vJiqxm4


### Props and State

```js
import ReactDOM from `react-dom`
import Hello from './App.js'

ReactDOM.render(
  <Hello name={"Nick"} />,
  document.getElementById('root')
)
```

Here, we've added a greeting for the user, Nick. So, how did we feed a name to the "Hello" component WITHOUT hardcoding it into the render method?

First, the data was passed wherever the ***component*** was rendered. 

Then, inside the ***component definition***, there is a reference to the data which shows as a property on the props object. 

```js
class Hello extends Component {
  render () {
    return (
      <h1>Hello {this.props.name}</h1>
    )
  }
}
```

Notice, in the beginning we were in the src/index.js (ReactDOM.render). In this file we find the ***component***

```js
import ReactDOM from `react-dom`
import Hello from './App.js'

ReactDOM.render(
  <Hello name={"Nick"} />,
  document.getElementById('root')
)
```

Now, we are in another file with the ***component definition***, and we replace 'world' with {this.props.name}: 


```js
class Hello extends Component {
  render () {
    return (
      <h1>Hello {this.props.name}</h1>
    )
  }
}
```

Basically, we can pass mutiple properties to our component when it's rendered in the index file. Then, in our component definition, we have access to those values. 

So props takes care of data representation that will REMAIN THE SAME for the ENTIRE life of a given component. But what about application data that may CHANGE depending on user interaction?


### State

In state, stored values are MUTABLE. Just like properties we can access state values via this.state.val

But...

Setting up and modifying state is not as straightforward as properties. To set up and modify state you have to explicitly declare the mutation and then define methods in order to define how to update state


### React Forms

React forms are created using something called controlled components, so any form input is controlled by React. This means that React acts as a "single source of truth" and all the form's state is stored by, well, React. 

This opens up the following questions: 

How does React track state in a form?
How do you submit a form in React?
How do we fake a login and switch to another component?
How do we lift up state---passing it from a child component to a parent component? 

