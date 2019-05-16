[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Production Ready React

## Objectives

By the end of this, developers should be able to:

* Explain some of the advanced React and JavaScript features that help with
  building a larger React application
* Return to old code and notice what could be improved.

## Introduction

So far in this class we've seen a couple different React applications. In some
cases, we simplified the code or used non-optimal practices in order to get
through the material efficiently.

Code is never perfect and there are constantly new technologies and practices
coming out that you may want to integrate into your project.

In this class, we will review some new JavaScript and React practices and
discuss how you could implement these into one of the React applications you
built recently.

### Best practices

Here are a few practices that you should (or sometimes have to) follow when
deploying an application on the internet.

#### Shared Constants

If we have a set of `constants` (really, any type of value) that we're going to
use in multiple places (like maybe a URL for an API that we're communicating
with), we can put them in one file and just import them everywhere that we need
them. It's easier than redefining them every time.

#### Functional vs Class Components

We talked about this very briefly. The main reason we might convert a class to
a functional component is because it gives a slight performance boost to React.
This really adds up when working on a large app.

Functional components also make thinking/reasoning about our component tree
easier and make managing state easier. Your goal should be to have as few
components managing state as possible.

#### Linting

Linting means using a tool (like Prettier, Standard JS, or ESLint) to keep your
code clean.

This matters especially when multiple people are working on the same project.
You want every developer to follow the same standards, because it makes your
project easier to read, easier to change, and it's easier to review commits when
the only changes are related to functionality, instead of functionality +
formatting all mixed together.

#### Snippets

Snippets are typically bundled into a package or library for your code editor
and help you in generating code. The [Reactjs Code
Snippts](https://github.com/xabikos/vscode-react) plugin is a really great one
for VS Code.

If you type `rcc` in VS Code and then hit `tab`, the snippet library will
generate the following code for you:

```js
import React, { Component } from 'react';

class Header extends Component {
  render() {
    return (
      <div>

      </div>
    );
  }
}

export default Header;
```

If you type `rsc` and hit `tab`, you'll generate the following code:

```js
import React from 'react';

const test = () => {
  return (
    <div>

    </div>
  );
};

export default test;
```


```sh
git clone git@git.generalassemb.ly:dc-wdi-react-redux/flashcards.git
cd flashcards
code .
git checkout solution
```

With the person next to you, go though the code in the
[Flashcards](https://git.generalassemb.ly/dc-wdi-react-redux/flashcards/tree/solution)
solution branch.

What stands out to you?

A couple things to think about when reading over the code:

1.  Which component is the biggest? Would you organize it differently? How?
2.  Where is the application getting data from? What kind of data is it
    returning?
3.  How does the data flow down into other components?
4.  What kind of component is Definition? What about FlashcardDetail? Why use
    one type over the other?
5.  Look in `FlashcardDetail.js` and find the `decrementTimer` method. Look at
    the `setState` call in it - how is it different from what you've seen
    before?


These snippets are really helpful for generating boilerplate code and helping
you work through a feature or idea quickly!

#### Prop Types

React has an extra feature called `propTypes` that we can install and use in our
components.

Adding prop types is really something that we do for consistency internally. It
has no effect on the functionality of our application, it's simply to signal to
various developers working on your team what each component is allowed to
receive as props.

You can imagine that this is especially useful when working on larger
applications, in an area of the codebase that you're unfamiliar with.

## Linting & Code Formatting

There are a lot of options out there when it comes to checking your code for
consistency and clarity. This process is called linting. If you've previously
installed something you probably have noticed some red squiggly lines in places.
This is the linter showing you where your code doesn't meet standards.

Here are a few:

- [ESLint](https://eslint.org/)
- [Standard JS](https://standardjs.com/)
- [JSHint](http://jshint.com/)

You **do not want** to have more than one of these installed at a time. Best
case scenario, you start to ignore all the highlighting. Worst case, it actually
creates extra work for you to figure out what the problems are.

The one I prefer though is called
[prettier](https://prettier.io/docs/en/install.html).

I like it for a number of reasons:

- You can install it globally (using npm install -g) or locally for each
  project.
- It's opinionated, which means it believes that doing things a specific way is
  the right way. That also means that you don't spend a ton of time configuring
  it (like ESLint).
- It can format your code automatically every time you save. This saves some
  thinking, but can also be annoying.
- It doesn't highlight your code with a bunch of red squigglies. It just formats
  when you want it to.

If you want to install prettier, you have to do two things:

1.  `npm install -g prettier`
2.  Install the editor extension for your editor. If you're using VSCode use
    [this extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode).

To run prettier, open the command palette: `cmd-shift-p` on mac or
`ctrl-shift-p` on linux.

Then choose `Format Document` by typing the first few letters and selecting it.
If you have the default shortcut set up it might already work: `ctrl-shift-i` or
`cmd-shift-i`

You can also enable format on save by opening the vscode config file (`ctrl-,`
or `cmd-,`) and searching for "Format on save". Check the box to enable it.

If you want to edit the config file manually:

```json
{
  "editor.formatOnSave": true
}
```

## Functional vs Class components (15 min / 0:45)

In react, there are several ways we can write components. The way we've been
doing it has been to make a Class component for everything. But we don't
necessarily want that on larger applications.

Look at the `Definition.js` component:

```js
import React, { Component } from "react"

const COLORS = ["#673ab7", "#2196f3", "#26a69a", "#e91e63"]

class Definition extends Component {
  constructor(props) {
    super(props)

    this.styles = {
      color: "white",
      padding: "10px",
      backgroundColor: COLORS[props.idx]
    }
  }

  render() {
    return (
      <div className="card text-center" style={this.styles}>
        <h5>Definition {this.props.idx + 1}</h5>
        <p>{this.props.def.definitions[0]}</p>
      </div>
    )
  }
}

export default Definition
```

Notice that there's no state here. We're simply taking some inputs (props) and
returning something (html). If we don't have any state, and we're not using
lifecycle methods, we can convert this whole component to a function!

The advantage to doing this is mostly in performance, which matters on larger
applications. Since react doesn't have to instantiate the class, it saves some
overhead and helps the app run better.

Start by rewriting the line with `class` in it:

```js
const Definition = props => {
  /* */
}
```

Note that we have to provide `props` as an argument. Under the hood, react is
just turning all components into functions. So to keep our props, we give the
function an argument.

A couple things that we have to adjust now:

- Change all the props references from `this.props` to just `props`
- Get rid of `this.styles` and make it just `styles`
- Remove the `{ Component }` from the import at the top of the file. Leave
  `React` though!

Once you've done that, make sure all your variables are properly scoped so you
don't run into any errors. Check the solution branch if you get stuck.

> When creating a new functional component, you can use the `rsc` snippet
> instead of `rcc`.

You can also enable format on save or on paste in your VS Code settings!

### You Do: Install and Configure Prettier

Install Prettier and spend some time setting it up in VS Code. If you like it,
keep it. If you don't, uninstall it.

> For your upcoming group project, you and your group will want to get on the
> same page about formatting your code. Prettier is by far the easiest way to do
> that - just have everyone install it and set it up and you're done.

## Proptypes

Let's talk about Proptypes in React. Open up the [documentation for
PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html) and spend
some time reading through it.

When you finish, we'll discuss the following questions:

- What are Proptypes? What do we use them for?
- How do they make our code more stable?

<details>
  <summary>Solution</summary>
  <ul>
    <li>Proptypes loosely enforce type checking in React components</li>
    <li>They make our code more stable because we know the data types of our props!</li>
  </ul>
</details>

## Default Props

React also has the ability to set [Default
Props](https://reactjs.org/docs/typechecking-with-proptypes.html#default-prop-values).
This is really helpful when you're working with an API!

```js
FlashcardDetail.propTypes = {
  onTimerEnd: PropTypes.func,
  card: PropTypes.object
}
```
* How do you set Default Props?

<details>
<summary>Solution</summary>

There are two ways:

```js
FlashcardDetail.propTypes = {
  onTimerEnd: PropTypes.func.isRequired,
  card: PropTypes.object
}
```

React will yell at you if you have a propType marked as required and you don't
give it a value when rendering that component.

### You Do: Adding PropTypes to `Definition` (5 min / 0:55)

Add on proptypes to the `Definition` component. Consult the documentation to see
what kinds of values they should be set to.

## Default Props (5 min / 1:00)
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// Specifies the default values for props:
Greeting.defaultProps = {
  name: 'Stranger'
};

// Renders "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);```

or:

```js
Definition.defaultProps = {
  def: { definitions: ["test definition"] },
  idx: 0
}
```

We shouldn't need defaultProps for the function in `FlashcardDetail` -- if we
don't get the needed function our app should throw an error!
class Greeting extends React.Component {
  static defaultProps = {
    name: 'stranger'
  }

  render() {
    return (
      <div>Hello, {this.props.name}</div>
    )
  }
}
```
</details>

## Destructuring Props (5 min / 1:05)

While we are working with improving our props, let's also destructure them so
that they are easier to work with in our app.

Instead of:

```js
let def = props.def
let idx = props.idx
//...
```

We can change the props variables in the `Definition` component to:

```js
let { def, idx } = props
```

Now we have `def` and `idx` as variables in scope, but its a nicer syntax!
Another advantage to doing it this way is if we end up adding props later, it's
easier to destructure and refer to them.

## Break (10 min / 1:15)

## Constants (10 min / 1:25)

One thing we can do to better scale our application is move any values that are
going to be reused into a shared location. That way we can just import them when needed.

This is most commonly used for things like shared URLs, but this pattern can apply to anything.

* Create a file called `constants.js`
* Cut and paste the urls that axios is using into it
* Make a variable and export it
* Import that variable back into the original file, and put it back into the axios call

## Moving the API communication (30 min / 1:55)

We've talked a lot about keeping our code DRY and separating concerns in our
applications - you've already done some of this if you've worked with redux, but
we can also do so with our API calls using our existing knowledge.

We could make a `requests.js` file next to our `constants.js` file.

In that file, we could add the following:

```js
// requests.js
import axios from "axios"
import { CLIENT_URL } from "./constants"

let getRequest = axios
  .get(`${CLIENT_URL}/api/words/`)
  .catch(err => console.err(err))

export { getRequest }
```

Now, in our components, let's require that `request` function.

```js
import { getRequest } from "../requests"
// get rid of the { CLIENT_URL } import since we dont need it anymore
// also get rid of axios import
```

Now we can replace the axios call with our nicely extracted function from the
other file!

```js
componentDidMount() {
  //...
  getRequest().then(response => this.setState({ whatever: response.data }))
  //...
}
```

## Expanding `setState`

This has already been implemented in this app, but you may have seen elsewhere
that we called `setState` with an object instead of a function, like so:

```js
// FlashcardDetail.js
toggleShow() {
  this.setState({
    show: !state.show
  })
}
```

> This is a great shortcut for toggling a boolean value

While in most cases this will work, the `setState` method is asynchronous. So
this can lead to unintended consequences. Instead, call `setState` with a
function so that the previous state is preserved.

```js
this.setState(prevState => ({
  show: !prevState.show
}))
```

Also, if you want to call a function after the state is updated, do so with a
callback instead of just calling the function after calling the `setState`
method, like so:

```js
this.setState(
  prevState => ({
    show: !prevState.show
  }),
  () => console.log("hello world")
)
```

## Conclusion

As with anything code related, there are a million ways to solve a problem and they
all have trade-offs. You will find developers who will say that [insert
anything] is by far the best way to solve a problem, the reality is that there
is no one, absolutely correct way to do something - there are best practices
(plural), but no best practice (singular).

## Additional Resources

* [Our Best Practices for Writing React Components](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8)


## Deployment

 (Write more here ) 
The main ideas are:

* Make a static build
* Deploy that build to a host
* Verify your urls all work!

## LICENSE

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
1. All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
