![GA-Logo](https://camo.githubusercontent.com/6ce15b81c1f06d716d753a61f5db22375fa684da/68747470733a2f2f67612d646173682e73332e616d617a6f6e6177732e636f6d2f70726f64756374696f6e2f6173736574732f6c6f676f2d39663838616536633963333837313639306533333238306663663535376633332e706e67) 


## React Forms 

#### Controlled Components

- React forms are created using what we call controlled components. What this basically means
is any input form that is controlled by react.  What do we mean by controlled with React? It means 
react acts as the `Single Source of Truth`.  This means all the state of the form is stored by react.

#### What we are going to learn 

- How react tracks state in a form 
- How to submit a form 
- Create a way in which we can fake a login, and switch to another component
- Lift up state (how to pass state from a child component to a parent componet)


##### First lets create a login component 

1. We'll keep the ```App``` component as our ```Container``` component
2. Create a folder called ```Login``` and inside of it ```index.js```
3. Create the form in ```index.js``` and export it 
4. require the ```Login``` component in the ```App``` component 

- Your code should now look like the following 

- In ```App``` Component 
```javascript
import React, { Component } from 'react';
import Login from './Login';

class App extends Component {

  render() {
    return (
      <div className="App">
        <Login/>
      </div>
    );
  }
}

export default App;
```

- In ```Login``` component 

```javascript
import React, { Component } from 'react';



class Login extends Component {
  render(){
    return (
      <form>
        <input type='text' name="username" placeholder="username" />
        <input type='password' name="password" placeholder="password" />
        <input type='submit' value="Submit" />
      </form>
      )
  }
}


export default Login;
```

- Go Ahead and test that out make sure everything works.

#### Using State with our forms

- As said before we will use state as the 'single source of truth' for the value of our inputs
so we will need to create a function that updates our state value, as well as create a state to represent the value of the inputs at any given time, so our ```Login``` component should now look like the following.


```javascript
import React, { Component } from 'react';



class Login extends Component {
  constructor(){
    super();

    this.state = {
      username: '',
      password: '',
    }
  }

  handleChange = (e) => {
    this.setState({[e.currentTarget.name]: e.currentTarget.value});
  }
  render(){
    return (
      <form>
        <input type='text' name="username" placeholder="username" value={this.state.username} onChange={this.handleChange} />
        <input type='password' name="password" placeholder="password" value={this.state.password} onChange={this.handleChange} />
        <input type='submit' value="Submit" />
      </form>
      )
  }
}


export default Login;

```

- We are using one function in the above to update our state based on the variable ```e.currentTarget.name``` that gets its value from the ```name``` attribute on the form.  
We are using [computed Property Names](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names), which basically allows you to assign variables that can be calculated when updating the value of a property. Same thing as the es5 version below
```
const someObject = {};
someObject[name] = value;
// name is a variable that is calculated
```

Notice also we are using each state property in the value of our input, so that the input is consistently represented by the state, or 'single source of truth'.

##### Next lets handle submitting the form

- forms have an ```onSubmit``` attribute that gets triggered when we submit a form from a button or input.  We'll also create a function that will be called in the attribute, which will call a function we have yet to be defined that will be passed down from the ```App``` component.  Our ```Login``` component should now look like the following


```javascript
import React, { Component } from 'react';



class Login extends Component {
  constructor(){
    super();

    this.state = {
      username: '',
      password: '',
    }
  }
  handleSubmit = (e) => {
    // stop page from refreshing
    e.preventDefault();

    this.props.login();

  }
  handleChange = (e) => {
    this.setState({[e.currentTarget.name]: e.currentTarget.value});
  }
  render(){
    return (
      <form onSubmit={this.handleSubmit}>
        <input type='text' name="username" placeholder="username" value={this.state.username} onChange={this.handleChange} />
        <input type='password' name="password" placeholder="password" value={this.state.password} onChange={this.handleChange} />
        <input type='submit' value="Submit" />
      </form>
      )
  }
}


export default Login;
```

- As you can see we are using the ```login``` method attached to ```this.props``` inside 
of our ```handleSubmit``` function that is passed down from the app component.  Let's pass that down now, and lets actually invoke the function and pass whatever the user's username is to the function, so our ```Login``` component will now look like the following, 

```javascript
import React, { Component } from 'react';

class Login extends Component {
  constructor(){
    super();

    this.state = {
      username: '',
      password: '',
    }
  }
  handleSubmit = (e) => {
    e.preventDefault();

    this.props.login(this.state.username);

  }
  handleChange = (e) => {
    this.setState({[e.currentTarget.name]: e.currentTarget.value});
  }
  render(){
    return (
      <form onSubmit={this.handleSubmit}>
        <input type='text' name="username" placeholder="username" value={this.state.username} onChange={this.handleChange} />
        <input type='password' name="password" placeholder="password" value={this.state.password} onChange={this.handleChange} />
        <input type='submit' value="Submit" />
      </form>
      )
  }
}


export default Login;
```

and our ```App``` Component will now look like the following 

```javascript
import React, { Component } from 'react';
import './App.css';
import Login from './Login';

class App extends Component {
  constructor(){
    super();

    this.state = {
      logged: false,
      username: ''
    }
  }
  login = (username) => {
    this.setState({
      logged: true,
      username: username
    })
  }
  render() {
    console.log(this.state)
    return (
      <div className="App">
        <Login login={this.login}/>}
      </div>
    );
  }
}

export default App;
```


 -  As you can see, we are updating our state in the ```login``` method, that accepts the username passed up from the ```login``` component (this is called lifting State), we are then updating our state to let our application know if we are logged in and the what our user's username is.  

#### Lets set up our conditional render

1.  Create a folder called ```MainContainer``` which will be the container component for the main
part of the app, and inside of it another ```index.js```

2.  Go ahead and setup the Component and render a message that says hello 'username'
Your code should look like the following now.

```javascript
import React, { Component } from 'react';



class MainContainer extends Component {
  render(){
    return (
    <div>
      <h1>Helloooo {this.props.username}</h1>
    </div>
      )
  }
}


export default MainContainer;
```

-  Then to get this componet to work we require it in the ```App``` component
and set up our conditional render based on our ```logged``` variable using a 
[ternary statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator), ternaries are often used instead of if/else in react. So what the following code is saying if ```this.state.logged``` is ```true``` render the ```MainContainer``` otherwise render the ```Login``` component.  Also notice we are passing down ```this.state.username``` as a prop stored under the attribute ```username``` to the ```MainContainer``` component.

```javascript
import React, { Component } from 'react';
import './App.css';
import Login from './Login';

class App extends Component {
  constructor(){
    super();

    this.state = {
      logged: false,
      username: ''
    }
  }
  login = (username) => {
    this.setState({
      logged: true,
      username: username
    })
  }
  render() {
    console.log(this.state)
    return (
      <div className="App">
        {this.state.logged ? <MainContainer username={this.state.username}/> : <Login login={this.login}/>}
      </div>
    );
  }
}

export default App;
```
