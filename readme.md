# Production Ready React

## Learning Objectives

- Implement React best practices in an application.
- Return to old code and notice what should be improved.
- Deploy a React application.

## Framing (5 min / 0:05)

So far in this class we've seen a couple different React applications. In some cases, we simplified the code or used non-optimal practices in order to get through the material efficiently.

Code is pretty much never perfect, and there are constantly new technologies coming out that you may want to integrate into your project. In this class, we will revisit an application we built earlier in this class and refactor it to fit best practices in React and implement cutting edge features.

## Code Review (15 min / 0:20)

> 10 min exercise, 5 min review

Clone down the application, and switch to the solution branch.

```sh
git clone git@git.generalassemb.ly:dc-wdi-react-redux/flashcards.git
cd flashcards
code .
git fetch
git checkout solution
```

With the person next to you, go though the code in the [Flashcards](https://git.generalassemb.ly/ga-wdi-exercises/flashcards/tree/solution) application. Discuss what about it could be improved upon to meet the best practices we've learned in class. Also discuss which best practices the code already follows.

<details>
<summary>Solution</summary>
<br>
<b>Best Practices in Place</b>
<ul>
    <li>Functional components for stateless components</li>
    <li>URLs in a constants file</li>
</ul>

<b>Where to improve</b>

<ul>
    <li>Move more to constants!</li>
    <li>Add prop types and default props</li>
    <li>Use consistent linting throughout</li>
</ul>
</details>

A couple things to think about when reading over the code!

1.  Which component is the biggest? Would you organize it differently? How?
2.  Where is the application getting data from? What kind of data is it returning?
3.  How does the data flow down into other components?
4.  What kind of component is Definition? What about FlashcardDetail? Why use one type over the other?

## Linting & Code formatting (10 min / 0:30)

There are a lot of options out there when it comes to checking your code for consistency and clarity. This process is called linting. If you've installed something already you probably have noticed these red squiggly lines in places.

Here are a few:

- [ESLint](https://eslint.org/)
- [Standard JS](https://standardjs.com/)
- [JSHint](http://jshint.com/)

The one I prefer though is called [prettier](https://prettier.io/docs/en/install.html).

I like it for a number of reasons.

- You can install it globally (using npm install -g) or locally for each project.
- It's opinionated, which means it believes that doing things a specific way is the right way. That also means that you don't spend a ton of time configuring it (like ESLint).
- It can format your code automatically, every time you save. This saves a ton of time, but can also be annoying.

If you want to install prettier, you have to do two things:

> **Note: MAKE SURE YOU UNINSTALL OTHER CODE FORMATTERS BEFORE INSTALLING PRETTIER OR YOU'LL HAVE A BAD TIME**

1.  `npm install -g prettier`
2.  Install the editor extension for your editor. If you're using VSCode use [this extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode).

To run prettier, open the command palette: `cmd-shift-p` on mac or `ctrl-shift-p` on linux.

Then choose `Format Document` by typing the first few letters and selecting it. If you have the default shortcut set up it might already work: `ctrl-shift-i` or `cmd-shift-i`

You can also enable format on save by opening the vscode config file (`ctrl-,` or `cmd-,`) and adding

```json
{
  "editor.formatOnSave": true
}
```

## Proptypes (20 minutes / 0:50)

Let's talk about proptypes in react.

- What are proptypes? What do we use them for?
- How do they make our code more stable?

<details><summary>Solution</summary>
    <ul>
        <li>Proptypes loosely enforce type checking in React components</li>
        <li>They make our code more stable because we know the data types of our props!</li>
    </ul>

</details>

Let's get them installed in our application.

### We Do: Adding PropTypes to `FlashcardDetail`

> [Documentation](https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes) for reference.

Let's first install the 'prop-types' package so we can import it.

`npm install prop-types`

Then we'll require the proptypes in our file.

`import PropTypes from 'prop-types'`

> Note the capitalization!

Then, within the class, let's add a static property:

```js
FlashcardDetail.propTypes = {
  onTimerEnd: PropTypes.func
}
```

> **Note the capitalization!!!!!**

This tells react that this prop should be a function.

We can also make props required!

```js
FlashcardDetail.propTypes = {
  onTimerEnd: PropTypes.func.isRequired
}
```

React will yell at you if you have a propType marked as required and you don't give it a value when rendering that component.

### You Do: Adding PropTypes to `Definition` (5 min)

Add on proptypes to the `Definition` functional component. Consult the documentation to see what kinds of values they should be set to.

## Default Props (5 min / 0:55)

While we are at it, let's add in the `defaultProps` just in case our parent component doesn't pass us the needed props.

We don't have to import anything for this one, which is great.

In the `Definition` component, let's add the following:

```js
Definition.defaultProps = {
  def: { definitions: [] },
  idx: 0
}
```

We shouldn't need one for the function in `FlashcardDetail` -- if we don't get the needed function our app should throw an error!

## Destructuring Props (5 min / 1:00)

While we are working with on improving our props, let's also destructure them so that they are easier to work with in our app.

Instead of:

```js
let def = props.def
let idx = props.idx
```

We can change the props variables in the `Definition` component to:

```js
let { def, idx } = props
```

Now we have `def` and `idx` as variables in scope, but its a nicer syntax! Another advantage to doing it this way is if we end up adding props later, it's easier to extract them.

## Break (10 min / 1:10)

## Moving the API communication (30 min / 1:40)

We've talked a lot about keeping our code DRY and separating concerns in our applications -- we will see how to do that with state later this week with Redux, but we can also do so with our API calls using our existing knowledge.

Let's make a requests.js file next to our constants.js file.

`$ touch requests.js`

In that file add the following:

```js
import axios from "axios"
import { CLIENT_URL } from "./constants"

export default axios
  .get(`${CLIENT_URL}/api/words`)
  .catch(err => console.log(err))
```

Now, in the `FlashcardContainer`, let's require that request function.

```js
import { getRequest } from "../requests"
// get rid of the { CLIENT_URL } import also since we dont need it anymore
// also get rid of axios import
```

Now we can replace the axios call with our nicely extracted function from the other file!

```js
componentDidMount() {
  //...
  getRequest.then(response => this.setState({ flashcards: response.data }))
  //...
}
```

## Expanding setState (5 min / 1:45)

This has already been implemented in this app, but you may have seen elsewhere that we called `setState` with an object instead of a function, like so:

```js
this.setState({
  show: !state.show
})
```

While in most cases this will work, the `setState` method is asynchronous. So this can lead to unintended consequences. Instead, call `setState` with a function so that the previous state is preserved.

```js
this.setState(prevState => ({
  show: !prevState.show
}))
```

Also, if you want to call a function after the state is updated, do so with a callback instead of just calling the function after calling the `setState` method, like so:

```js
this.setState(
  prevState => ({
    show: !prevState.show
  }),
  () => console.log("hello world")
)
```

# Deployment

## Environment variables & .env (20 min / 2:05)

The purpose of environment variables is to provide our application different configurations depending on the environment that we're in. For example, we want to maybe make requests to `localhost` when we're running on our machine, but when our application is deployed we need to make a request to a heroku url like `slanderous-whale-8821.herokuapp.com`

It would be really great if we could do this automatically, without having to rewrite our code!

**Introducing Environment variables**

If you remember back to the olden days, when we did our first heroku deployment, we did something like this in the command line:

```sh
EXPORT MLAB_URL="whatever.com"
```

Then we were able to access that MLAB_URL variable inside of our node application!

> How did we do that??

<details>
  <summary>Answer</summary>

```js
const MLAB_URL = process.env.MLAB_URL
```

</details>

Instead of having to type out this export command every time, we can put it all in a file.

Because we're using Create React App, it handles this for us. All we have to do is create a file. If we were on a back end app like express, or not using CRA, we could use the fantastic [dotenv package](https://github.com/motdotla/dotenv).

Create a file in the root folder called `.env`

**.env format**

.env files follow a simple format of keys and values, separated by returns. Put some fake values in there that we'll use just as examples.

CRA ignores any values that don't begin with `REACT_APP_` so we have to do that in this case, but normally we don't.

```
REACT_APP_FAKE_ONE=thisisfake
REACT_APP_FAKE_TWO=haveAgoodDaySir
```

> Note: Normally you do NOT want to include your .env files in version control, especially if they contain sensitive information, like API keys or database passwords. So make sure you add .env to your .gitignore file.

Now anywhere in our app, we should have access to these values in the object: `process.env`.

Heroku has [their own way](https://devcenter.heroku.com/articles/config-vars) of managing environment variables, but they're still accessible through `process.env` in our application.

### You do: Display the fake values (5 min)

Figure out two different locations for where you'd want these fake values to render. Then render them.

## Quick References

- [Deploying a Node-Express-Mongoose App/API with Heroku & MLab](https://git.generalassemb.ly/ga-wdi-lessons/express-mongoose-mlab-deploy)

## Multi-Server Approach

In this class, we have used a multi-server or "micro-service" based deployment strategy for our MERN applications. This means that we will have a separate front-end and API running on two different servers. You will follow the mongoose and mlab instructions for creating your API. Make sure you send `res.json()` responses and include `cors`! Our front and back ends will not be physically attached via code, rather they will communicate through AJAX requests. We've already seen this in several lessons!

Our React application will be not be deployed using heroku - heroku only handles things on the backend. Since the code is static and doesn't interface with a database or need to be run inside of a `node` environment, we can use a static host.

Choose between one of two options for deployment: one is github pages, the other is called [surge](http://surge.sh/).

Your task for the rest of class is to deploy this flashcard app to either one of these sites. There are lots of tutorials! Use your google-fu to find the resources you need to deploy to.

## Extra Resources

- [Our Best Practices for Writing React Components](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8)
- [Building A MERN App](https://git.generalassemb.ly/ga-wdi-lessons/building-a-mern-app/blob/master/readme.md)
