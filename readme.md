[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Production Ready React

## Objectives

By the end of this, developers should be able to:

- Explain some of the advanced React and JavaScript features that help with
  building a larger React application

## Introduction

So far in this class we've seen a couple different React applications. In some
cases, we simplified the code or used non-optimal practices in order to get
through the material efficiently.

Code is never perfect and there are constantly new technologies and practices
coming out that you may want to integrate into your project.

In this class, we will review some new JavaScript and React practices and
discuss how you could implement these into one of the React applications you
built recently.

## Snippets

Snippets are typically bundled into a package or library for your code editor
and help you in generating code. The
[Reactjs Code Snippts](https://github.com/xabikos/vscode-react) plugin is a
really great one for VS Code.

If you type `rcc` in VS Code and then hit `tab`, the snippet library will
generate the following code for you:

```js
import React, { Component } from "react"

class Header extends Component {
  render() {
    return <div />
  }
}

export default Header
```

If you type `rsc` and hit `tab`, you'll generate the following code:

```js
import React from "react"

const test = () => {
  return <div />
}

export default test
```

These snippets are really helpful for generating boilerplate code and helping
you work through a feature or idea quickly! There are a lot of other options for
snippets in there, check out the docs.

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

## Destructuring Props (5 min / 0:50)

While we are working with improving our components, let's also destructure our
props so that they are easier to work with in our app.

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

This is useful when working with a whole bunch of props, or for when you're
rendering a lot of complicated html. It saves a fair amount of typing. You can
do this with any object of course, not just props.

## Constants (10 min / 1:30)

One thing we can do to better scale our application is move any values that are
going to be reused into a shared location. That way we can just import them when
needed.

This is most commonly used for things like shared URLs, but this pattern can
apply to anything.

- Create a file called `constants.js`
- Cut and paste the urls that axios is using into it
- Make a variable and export it
- Import that variable back into the original file, and put it back into the
  axios call


## Using `prevState`

This has already been implemented in this app, but you may have seen elsewhere
that we called state updating functions with an object instead of a function, like so:

```js
  let [show, updateShow] = useState(true)
  updateShow(!show)
```

> This is a great shortcut for toggling a boolean value

While in most cases this will work, the `updateShow` method is asynchronous. So
this can lead to unintended consequences. Instead, call `updateShow` with a
function so that the previous state is preserved.

```js
updateState(prevState => {
  return !show
})
```

## Linktree

- https://surge.sh/
- https://reactjs.org/docs/typechecking-with-proptypes.html
- https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment


## LICENSE

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
1. All software code is licensed under GNU GPLv3. For commercial use or
   alternative licensing, please contact legal@ga.co.
