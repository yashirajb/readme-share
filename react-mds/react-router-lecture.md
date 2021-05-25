# React Router!

### Learning Objectives
- Describe how React Router makes our SPA "internet-ready"
- Use React Router to create a single-page app that replicates the functionality of a multi-page site
- Use `Router`, `Link`, `Route`, `Redirect` and `Switch` to allow for navigation and URL manipulation

## How can we organize our apps as they grow?

Let's think about the standard webpage. A lot of sites follow a structure sort of like this:

- Home
- About
- Services
- Blog / Gallery / Etc.
- Contact

Each of those pages might have multiple subpages, which might have subpages of their own. How might we accomplish something like this with React? And how can we make it so we can link to each one of those pages?

# Enter React Router!

Up to this point, our React applications have been limited in size, thus allowing us to use basic conditional logic in our components' render methods for changing component views. However, as our React applications grow in size and scope, we will want an easier and more robust way to set up navigation to different component views. Additionally, we will want the ability to set information in the url parameters to make it easier for users to identify where they are in the application.

React Router, while not the only, is the most commonly-used routing library for React. It is relatively straightforward to configure and integrates with the component architecture nicely (itself being nothing but a collection of components). Once configured, it serves as the root component in a React application and renders other application components within itself depending on the path in the url.

## Intro to React Router

Just like we had different dependencies in express that we imported using `require`, we can import modules into our React apps. Here's the steps for setting up React Router:

- `npm i react-router-dom`
- Add the following JS to `index.js`

```js
import { BrowserRouter as Router} from 'react-router-dom';
```

You will have to import things from `react-router-dom` in a couple of other places as well. We'll talk about those in a bit.

## The parts of React Router

In this example, we're hitting the `/about` route and then saying "Okay, render the about page with some data." React Router works along similar lines.

### `Router`

React Router first provides us the `Router` component. It talks to the browser and allows us to create "history" (the ability to use the forward/back buttons) with our app, even though we are still on a single-page app.  It also provides us the ability to update the URL to redirect by updating the history object.

As with a components `render()` method the `Router` component also expects to receive only one child element.  The child element can have routes defined or those routes could be even nested further.  

### `Route`

Much as in our Express routes, the React Router `Route` component allows us to define a URL endpoint and describe what should load on the page at that point. They can be created anyplace inside of `Router`.

Let's get this setup at our most top level App component in  `index.js`:

```
ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById('root')
);
```


#### `Route Props`

A `route` has defined props that tell it what route to look for but also what to do once that route has been matched.   Below are the props used most often:

- `path` - the url that should be matched
- `component` - the component that should be rendered
- `render` - jsx that is rendered directly in the route

### Let's put both of these in action.


In `App.js`:
at the top include the following import:
```
import { Route } from 'react-router-dom';
```

```
  render() {
    return (
        <div className="app">
          <main>
            <Route path="/" component={Home} />
            <Route path="/house" component={House} />
          </main>
        </div>
    );
  }
```

Let's talk a little bit about what's going on here.

```
<Route path="/" component={Home} />
```

We're telling the route what path it should expect, and which component it should render at that point. However, there's one thing we need to fix. The path to `/house` also shows our Home component. That shouldn't happen! We fix it by using `exact` as we define the path.

```
  render() {
    return (
        <div className="app">
          <main>
            <Route exact path="/" component={Home} />
            <Route path="/house" component={House} />
          </main>
        </div>
    );
  }
```

This tells the route to only render that component if the route matches exactly `/`.

If you ever need to test paths you can use this nifty [router tester](https://pshrmn.github.io/route-tester/#/) app to do so.

### Lab 1 - Add Router and Routes To House-Example-Begin 

Do the following:

1) "Run `npm i react-router-dom`. Confirm that `package.json` now references the installed package

2) Update `index.js` to import `BrowserRouter as Router` from `react-router-dom`.

3) Use the `Router` component to provide routing functionality to the `App` component.

4) In `App.js` add the following routes to the `<main>` section:

| Route       | Component To Render           |
| ------------- |:-------------:|
| /      | Home |
| /house      | House      |  

5) In the `Components` folder edit the `Home` and `House` files to include `stateless` components that return a `p` tag with text that includes their component name.

6) Test both routes and make note of what happens when you go to either route, specifically the `/house` route.

7) Add the `exact` prop to the default route and test the `/house` route again.

### Lab 1 Review - 10min

Review the previous lab

### `Switch` and `Redirect`

As we've seen thus far it's possible for multiple routes to execute simultaneously without including `exact`.  The `Switch` component can be used to group routes and will only render the first matching route.  If the routes are added in the order of most to least specific then it's possible to avoid using `exact`  altogether.

`Redirect` is used as the last route to render when no others have been matched.

```
  render() {
    return (
        <div className="app">
          <main>
            <Switch>
              <Route exact path="/" component={Home} />
              <Route exact path="/house" component={House} />
              <Redirect to="/" />
            </Switch>
          </main>
        </div>
    );
  }
```

### Using params

Just like with Express, we can use parameters in our URL structure to render pages differently. Parameters are defined using a colon and the parameter name.  Let's add a new route to our app that will be used to capture a specific room type in a house.

```
  render() {
    return (
	    <main>
	      <Route exact path="/" component={Index} />
	      <Route exact path="/house" component={House} />
	      <Route path="/house/:room" component={Room} />
	    </main>
    );
  }
```

Once the parameter is captured it's stored within the `this.props.match.params` object as a key\value pair.  

In `Room.jsx`:

```
  render() {
    return (
      <div>
        <p>This is the {this.props.match.params.room}.</p>
      </div>
    );
  };
```

### Redirect Using the Router History Object

Sometimes the need arrises to change the URL based on some event and force a redirect to another route.  This can be done by pushing the new route into the routers `history` property which is done using it's `push` method.  The `history` property is made available to the rendered component via `this.props.history`.

In `Room.jsx`:

```
  render() {
    return (
      <div>
        <p>This is the {this.props.match.params.room}.</p>
        <button onClick={this.props.history.push('/')}>Back to home</button>
      </div>
    );
  };
```

### Linking to routes

So we've used `Router` and `Route` thus far to control routing and `this.props.history.push()` to force a redirect. Another way to force the redirect based on an `anchor` tag is to use the `Link` component. This provides us the ability to link to other pages in your app.  The `Link` component is rendered as an anchor tag.  Let's create a new component called `Header` add a pseudo-navigation to our app.

In `Header.js`:

at the top of `Header.js` include the following import

```
import { Link } from 'react-router-dom';
```

```
render() {
  return (
    <nav>
      <ul>
        <li><Link to="/">Index</Link></li>
        <li><Link to="/house">House</Link></li>
        <li><Link to="/house/kitchen">Kitchen</Link></li>
        <li><Link to="/house/porch">Porch</Link></li>
      </ul>
    </nav>
  );
}
```

### Final App Component

Importing the new `Header` component into `App` will complete our demo.

At the top of `App.js` iport the following components from 'react-router-dom'

```
import { Switch, Route, Redirect } from 'react-router-dom';
```

```
class App extends Component {
  render() {
    return (
        <div className="app">
          <Header />
          <main>
            <Switch>
              <Route exact path="/" component={Home} />
              <Route exact path="/house" component={House} />
              <Route path="/house/:room" component={Room} />
              <Redirect to="/" />
            </Switch>
          </main>
        </div>
    );
  }
}
```


## 🚀 Lab 2 - Using Switch, Redirect, Params, History and Link to

Do the following:

1) Add `Switch` and `Redirect`.

2) Add a `room` paramter to `house` and use `this.props.match.params` to display the room name.

3) Add a button that will redirect the user back to the home page using  `this.props.history.push('/')`.

4) Add `Link to` to all the links in the `nav` element and configure them to point to the corresponding route.  

5) Importing the new `Header` component into `App` and make sure it all works.


### Lab #2 Review - 10min

Review the previous lab.


## 🚀 Lab - Adding a new route to our quotes app! - (45 min)

Using routes to mimic a multi-page effect is good and cool, but the example we just walked through is just the tip of the iceberg of what's possible when it comes to routes. Try adding routes to the Quotes React app in the lab folder.

As we do so, we'll do the following things:

- Set up the core of our app with a router that allows us to visit three pages: `Home`, `QuotesList`, and `SingleQuote`.
- Add a navigation that provides links to `Home` and `QuotesList`.
- Add a link to each individual quote in `QuotesList` that takes the user to a `SingleQuote` page.
- Make API calls based on what route we're on and what params have been passed.

For this lab you will add another route and another component.

- `npm i` to install the necessary dependencies.
- Add the necessary code to make the the `li's` of `Home` and `Quotes` in the `Header` component to render those pages accordingly.  
- Add a new route called `/about`.
- The about component should just have some basic info about your cohort and maybe a picture of it as well.
- After adding the component, add a link to the component in the header next to our other links.
- After adding the link, add a `<Route />` component that will match that route.

The final product should be the same as the app from the lecture with a full working for all the links in Header.

# References

- [A Simple React Router Tutorial](https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf)
