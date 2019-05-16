# Deployment

## Environment variables & .env (20 min / 2:05)

The purpose of environment variables is to provide our application different
configurations depending on the environment that we're in. For example, we want
to maybe make requests to `localhost` when we're running on our laptop, but when
our application is deployed we need to make a request to a heroku url like
`slanderous-whale-8821.herokuapp.com`

It would be really great if we could do this automatically, without having to
rewrite our code!

**Introducing Environment variables**

If you remember back to the olden days, when we did our first heroku deployment,
we did something like this in the command line:

```sh
EXPORT MLAB_URL="whatever.com"
```

Then we were able to access that MLAB_URL variable inside of our node
application!

> How did we do that??

<details>
  <summary>Answer</summary>

```js
const MLAB_URL = process.env.MLAB_URL
```

</details>

Instead of having to type out this export command every time, we can put it all
in a file.

Because we're using Create React App, it handles this for us. All we have to do
is create a file. If we were on a back end app like express, or not using CRA,
we could use the fantastic [dotenv package](https://github.com/motdotla/dotenv).

Create a file in the root folder called `.env`

**.env format**

.env files follow a simple format of keys and values, separated by returns. Put
some fake values in there that we'll use just as examples.

CRA ignores any values that don't begin with `REACT_APP_` so we have to do that
in this case, but normally we can call the keys whatever we like..

```
REACT_APP_FAKE_ONE=thisisfake
REACT_APP_FAKE_TWO=haveAgoodDaySir
```

> Note: Normally you do NOT want to include your .env files in version control,
> especially if they contain sensitive information, like API keys or database
> passwords. So make sure you add .env to your .gitignore file.

Now anywhere in our app, we should have access to these values in the object:
`process.env`.

Heroku has [their own way](https://devcenter.heroku.com/articles/config-vars) of
managing environment variables, but they're still accessible through
`process.env` in our application. Therefore, as long as we define the same
variables in both places, we don't have to write any additional code.

### You do: Display the fake values (5 min / 2:10)

Figure out two different locations for where you'd want these fake values to
render. Then render them.

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
