![GALOGO](https://camo.githubusercontent.com/6ce15b81c1f06d716d753a61f5db22375fa684da/68747470733a2f2f67612d646173682e73332e616d617a6f6e6177732e636f6d2f70726f64756374696f6e2f6173736574732f6c6f676f2d39663838616536633963333837313639306533333238306663663535376633332e706e67) 

# Intro to React.js: Part 1

### Why is this important?
*This workshop is important because:*

JQuery is cool! 

But as you could imagine, creating a large, complex UI in jQuery while maintaining readable, maintainable code is going to take: 
 - A. Excellent code organization
 - B. Really excellent code organization
 - C. Either a lot of messy HTML strings, or the use of a templating engine. 
 
React is an amazing solution that has a **built in code-organization strategy**, as well as a **clean, elegant templating solution**. 

Regarding code organization, React seperates everything into **components**. Everything is a component in React. A blog Post is a component. A comment is also a component, which can be nested in the Post component. (Much more on this later). 

For templating there is JSX. More on that later too. 

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

* Explain what ReactJS is and where it fits in our applications' stack.
* Explain the component model of web development.
* Create and render React components in the browser.

## What is ReactJS?

React is a library used to craft modern day UI and create views for the front-end in web, client and native applications.

### Some History

The first thing most people hear about React is "Facebook uses it."
* First used by Facebook in 2011. Then Instagram in 2012.
* Went open source in May 2013.

React was born out of Facebook's frustration with the traditional MVC model and how...
  * Re-rendering one thing meant re-rendering much of the page.
  * That had negative implications on processing power and ultimately user experience, which at times became glitchy and laggy.

> If you want to get a taste of what React's all about, [here's an introduction from React.js Conf 2015](https://www.youtube.com/watch?v=KVZ-P-ZI6W4&feature=youtu.be&t=510). Recommend starting around the 8:35 mark and watching until 16:30.

### React in MVC

React can be thought of as the "Views" layer.

<details>
  <summary><strong>What is the role of a "view" in a front-end Javascript application?</strong></summary>

  > The visual template the user sees, populated with data from our models.

</details>

React's role is just to use data to render a UI. This means that React can also coexist with other Javascript frameworks. Let them handle the models and controllers, and have React sort out the views.


## Initial Setup

In order to create a new project and to get our development environment setup, we are going to use the Terminal command `create-react-app`. It will create a new folder in your current directory for the in-class application. Here's an example of the setup commands:

```bash
$ npm i -g create-react-app
$ create-react-app react-sample-app
$ cd react-sample-app
$ atom .
```

**Explore the app:** Open the new scaffolded application and write down 3 observations and one question you have about the files that you find!

Once you're ready to run the server:

```bash
$ npm run start
```

After running `$ npm run start`, we can view the app at `http://localhost:3000`

`create-react-app` provides us with all the necessary tools and configuration necessary to start writing React. `npm run start` refers to an included script that starts up the development server.

Along with installing the necessary dependencies such as React, ReactDom, Babel and Webpack, it creates a initial app skeleton that looks like this...

```bash
├──README.md
├──  favicon.ico
├──  index.html
├──  node_modules
├──  package.json
└──  src
    ├──  App.css
    ├──  App.js
    ├──  index.css
    ├──  index.js
    └──  logo.svg
```

Most of the important files and primarily where you'll be working in the `/src` directory.

&#x1F535; **Activity**
```
- Explore the files we just created
- Check out the different naming conventions we are seeing here
- Check out the boilerplate code `create-react-app` gave us
- Share any comments or questions you have in a thread below
- 7 min
```  

## Components

One of the comments made about react when it was first open sourced was that it was "rethinking established best practices". At the time, devs were used to an MVC approach for separation of concerns. In React, we want to move towards more of a component based separation of concerns. On Facebook, you could think of each status post as a mini-component in react. A list  updates is a component that contains several of those mini-components. You could take that one step further and think of the Facebook app as one giant component with several components within it. (Things like the list of status updates, the friends list, the header, etc...)

&#x1F535; **Activity**
```
- Break into pairs and take a look at CraigsList. 
- Identify the visual "components" the website is comprised of. We suggest using markers to draw these out on your table!
-  Should look something like the diagram below.
- As you're drawing this out, think about the following questions...
- Where do you see "nested components"? Where do you not?
- Are there any components that share the same structure?
- Of these similar components, what is different about them?
- 10 min
```  

![Component diagram](https://maketea.co.uk/images/2014-03-05-robust-web-apps-with-react-part-1/wireframe_deconstructed.png)


Take a picture of your work and Slack it to the classroom channel before the exercise is over.


### Hello World - A Very Basic Component

> No need to follow along with this Hello World example. You will have the chance to implement this yourself.

The basic unit you'll be working with in ReactJS is a **component**.

* Components can be thought of as functional elements that takes in data and as a result produce a dynamic UI.

Throughout class we have separated HTML, CSS and Javascript.

* With components, the lines among those three become a bit blurry. "DEAR GOD NO!", shout many developers at first... but most come around when they see how logical it ends up feeling.

* With React, we organize our web apps according to small, reusable components that define their own content, presentation and behavior.

What does a component look like? Let's start with a simple "Hello World" example...

To start, in our `/src/App.js` file, let's remove the contents and in its place add this component definition...

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
*note:* note the lack of quotes around the h1! These are not HTML strings! It's JSX, a templating language that looks just like JavaScript!

Ok let's recap what's going on.

#### What's that HTML doing in my Javascript?

Often times we write out React components in **JSX**.
* JSX is [an alternate Javascript syntax](http://blog.yld.io/2015/06/10/getting-started-with-react-and-node-js/#.V8eDk5MrJPN) that allows us to write code that strongly resembles HTML. It is eventually transpiled to lightweight JavaScript objects.

* React then uses these objects to build out a "Virtual DOM" (more on that soon).

> For the true "purist", React can be written without JSX. If you want to learn more, [check out this blog post](http://jamesknelson.com/learn-raw-react-no-jsx-flux-es6-webpack/).  

Let's break down the things we see here...

 

##### `class Hello`
This is the component we're creating. In this example, we are creating a "Hello" component.

##### `extends Component`
This is the React library class we inherit from to create our component definition.

##### `render()`
Every component has, at minimum, a render method. It generates a **Virtual DOM** node that will be added to the actual DOM.
* Looks just like a regular ol' DOM node, but it's not yet attached to the DOM.

##### `export default Hello`
This exposes the Hello class to other files which import from the App.js file. The `default` keyword means that any import that's name doesn't match a named export will default to this. Only one default is allowed per file.

**Virtual DOM? How is that different from the usual DOM?**

The Virtual DOM is one of the major innovations of React. It is a Javascript representation of the actual DOM. (The actual DOM being the language-neutral interface provided by the browser). 

* Because of that, React can keep track of changes in the actual DOM by comparing different instances of the Virtual DOM.
* React then isolates the changes between old and new instances of the Virtual DOM and then only updates the actual DOM with the necessary changes.
* By only making the "necessary changes," as opposed to re-rendering an entire view altogether, we save up on processing power.
* This is not unlike Git, with which you compare the difference -- or `diff` -- between two commits.

![Virtual DOM Diagram](https://docs.google.com/drawings/d/11ugBTwDkqn6p2n5Fkps1p3Elp8ZToIRzXzvM4LJMYaU/pub?w=543&h=229)

> If you're interested in learning more about the Virtual DOM, [check this video out](https://www.youtube.com/watch?v=-DX3vJiqxm4).

So we've created the template for our component. Now let's use `/src/index.js` to load in our new component and render it on the DOM. change the third import to import `Hello` instead of App. Also change the `<App />` tag to `<Hello />`...

```js
import React from 'react'
import ReactDOM from 'react-dom'
import Hello from './App.js'

ReactDOM.render(
  <Hello />,
  document.getElementById('root')
)
```

> In place of `ReactDOM.render` some tutorials will use React.renderComponent, which has been phased out. Change outlined [here](http://bit.ly/1E81Whs).

`ReactDOM.render` takes the Virtual DOM node created by `extends Component` and adds it to the actual DOM. It takes two arguments...
  1. The component.
  2. The DOM element we want to append it to.

What language is `<Hello />` written in? **JSX.**

* Similar to XML.
* When we say `<Hello />`, in plain Javascript we are actually saying `React.DOM.div( null, "Hello world.")`
* Basically, a string of React methods that create a virtual DOM node.

> **NOTE:** Whenever you use a self-closing tag in JSX, you **MUST** end it with a `/` like `<Hello />` in the above example.

## Build Hello World + Modify it a bit 

&#x1F535; **Activity**
```
- Modify your App as create-react-app made it to render Hello World as we did above
- Once this is working, update this component to have a h1, p, and img elements
- You may build it wrong and get a look at some super helpful React errors! 
- Try to debug on your own, but there is a hint below.
- 15 min
```  

<details>
  <summary><strong>Hint:</strong></summary>

  Your component must have one parent element. That is, you must have one "root" or top level HTML element that holds all of the rest of the HTML that defines your component.

</details>

## Closing

So far, we've learned what React is, what the virtual DOM is in React, and made our first React component (yay!). It's time for a break - in React Intro part 2, we will move on to understand two very essential React topics: `props` and `state`.  
