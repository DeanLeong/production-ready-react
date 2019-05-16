# Production Ready React

## Learning Objectives

- Learn some react & JS features that will help you when building larger
  applications.
- Return to old code and notice what should be improved.
- Deploy a React application.

## Framing (5 min / 0:05)

So far in this class we've seen a couple different React applications. In some
cases, we simplified the code or used non-optimal practices in order to get
through the material efficiently.

Code is pretty much never perfect, and there are constantly new technologies
coming out that you may want to integrate into your project. In this class, we
will look at the flashcards application refactor it to fit best practices in
React and implement cutting edge features.

### Best practices

Here are a few practices that you should (or sometimes have to) follow when
deploying an application on the internet.

#### Shared Constants

If we have a set of `constants` (really, any type of value) that we're going to
use in multiple places (like maybe a URL for an API that we're communicating
with), we can put them in one file and just import them everywhere that we need
them. It's easier than redefining them every time.

#### Functional vs class components

We briefly talked about this, but the main reason we might convert a class to a
functional component is because it gives a slight performance boost to react.
This really adds up when working on a large app.

#### Linting

Linting means using a tool (like Prettier, Standard JS, or ESLint) to keep your
code clean.

This matters especially when multiple people are working on the same project.
You want every developer to follow the same standards, because it makes your
project easier to read, easier to change, and it's easier to review commits when
the only changes are related to functionality, instead of functionality +
formatting all mixed together.

#### Prop Types

React has an extra feature called `propTypes` that we can install and use in our
components.

Adding prop types is really something that we do for consistency internally. It
has no effect on the functionality of our application, it's simply to signal to
various developers working on your team what each component is allowed to
receive as props.

You can imagine that this is especially useful when working on larger
applications, in an area of the codebase that you're unfamiliar with.

## Code Review (15 min / 0:20)

> 10 min exercise, 5 min review

Clone down the application, and switch to the solution branch.

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

## Linting & Code formatting (10 min / 0:30)

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

## Proptypes (10 minutes / 0:55)

Let's talk about proptypes in react.

- What are proptypes? What do we use them for?
- How do they make our code more stable?

<details>
  <summary>Solution</summary>
  <ul>
    <li>Proptypes loosely enforce type checking in React components</li>
    <li>They make our code more stable because we know the data types of our props!</li>
  </ul>
</details>

Let's get them installed in our application.

### We Do: Adding PropTypes to `FlashcardDetail`

> [Documentation](https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes)
> for reference.

Let's first install the 'prop-types' package so we can import it.

> the `prop-types` package used to be built into React, but now it's a separate
> module so we have to install it.

`npm install prop-types`

Then we'll require the proptypes in our file.

`import PropTypes from 'prop-types'`

> Note the capitalization!

Then, within the file let's add a static property. Put this outside the class,
before the export.

```js
FlashcardDetail.propTypes = {
  onTimerEnd: PropTypes.func,
  card: PropTypes.object
}
```

> **Note the capitalization!!!!!**

This tells react that this prop should be a function.

We can also make props required!

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

While we are at it, let's add in the `defaultProps` just in case our parent
component doesn't pass us the needed props.

We don't have to import anything for this one, which is great.

In the `Definition` component file, let's add the following:

```js
Definition.defaultProps = {
  def: { definitions: ["test definition"] },
  idx: 0
}
```

We shouldn't need defaultProps for the function in `FlashcardDetail` -- if we
don't get the needed function our app should throw an error!

## Destructuring Props (5 min / 1:05)

While we are working with on improving our props, let's also destructure them so
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
applications -- you've already done some of this if you've worked with redux,
but we can also do so with our API calls using our existing knowledge.

Let's make a requests.js file next to our constants.js file.

`$ touch requests.js`

In that file add the following:

```js
// requests.js
import axios from "axios"
import { CLIENT_URL } from "./constants"

let getRequest = axios
  .get(`${CLIENT_URL}/api/words/`)
  .catch(err => console.err(err))

export { getRequest }
```

Now, in the `FlashcardContainer`, let's require that request function.

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
  getRequest.then(response => this.setState({ flashcards: response.data }))
  //...
}
```

## Expanding setState (5 min / 1:45)

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

## Deployments (rest of class)

Our React application will be not be deployed using heroku - heroku only handles
things on the backend. Since the code is static and doesn't interface with a
database or need to be run inside of a `node` environment, we can use a static
host.

Choose between one of two options for deployment: one is github pages, the other
is called [surge](http://surge.sh/).

Your task for the rest of class is to deploy this flashcard app to either one of
these sites. There are lots of tutorials! Use your google-fu to find the
resources you need to deploy to.

The main ideas are:

* Make a static build
* Deploy that build to a host
* Verify your urls all work!

## Extra Resources

- [Deploying a Node-Express-Mongoose App/API with Heroku & MLab](https://git.generalassemb.ly/ga-wdi-lessons/express-mongoose-mlab-deploy)
- [Our Best Practices for Writing React Components](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8)
- [Building A MERN App](https://git.generalassemb.ly/ga-wdi-lessons/building-a-mern-app/blob/master/readme.md)
