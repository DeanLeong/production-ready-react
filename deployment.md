# Deployment

## Environment variables & .env (20 min / 2:05)

The purpose of environment variables is to provide our application different
configurations depending on the environment that we're in. For example, we want
to maybe make requests to `localhost` when we're running on our laptop, but when
our application is deployed we need to make a request to a heroku url like
`slanderous-whale-8821.herokuapp.com`

We've already worked with this before, but how do we integrate these into deployed applications?

**.env format**

.env files (any file that begins with .env) follow a simple format of keys and values, separated by carriage returns. Put some fake values in there that we'll use just as examples.

CRA ignores any values that don't begin with `REACT_APP_` so we have to do that
in this case, but normally we can call the keys whatever we like..

```
REACT_APP_FAKE_ONE=thisisfake
REACT_APP_FAKE_TWO=haveAgoodDaySir
```

> Note: Normally you do NOT want to include your .env files in version control,
> especially if they contain sensitive information, like API keys or database
> passwords. So make sure you add .env to your .gitignore file. If we use the .env

Now anywhere in our app, we should have access to these values in the object:
`process.env`.

However, depending on our deployment platform of choice, we have to change our strategy.

## Deployments (rest of class)

When we deploy our React applications, we will either manually create a production build, and use a service such as [surge](http://surge.sh/) to deploy, or push it to a hosting service such as [Netlify](https://www.netlify.com/).

### Surge

To work with surge, which is an npm package, we must install it globally. We only need to do this once, as it is not a per-project dependency. We can install it globally with `npm install --global surge` (or `npm i -g surge` for short). After we install it, we can use it by running the `surge` command.

With surge, the deployment is dependent on the build we create in a folder. But how do we create a build?

Within your React app (at the level of your `package.json`), run `npm run build`. This will create a production-ready static build of your site. Once you do this, `cd` into the newly created `build` folder, and take a look at the files inside. Once you've finished taking a look, run `surge`, and create an account. After creating an account, you can choose a folder (we'll choose the current folder), and a url (surge will provide a random one by default). Choose a URL quickly, as the url may also be generated for someone else at the same time (this happens relatively commonly). After you've selected the url, surge will push up this folder to the newly created website, and you'll be able to view it.

#### Pros and Cons
Pros:
- Great for demonstrating quick prototypes to show clients or colleagues.
- Independent from Git, but [relatively easy to integrate](https://surge.sh/help/deploying-continuously-using-git-hooks).
- Unlimited deploys per person.

Cons:
- Environment variables are fickle, and won't necessarily show up.
- Unless you integrate with Git, you need to create a new build for any changes you make.
- Builds are almost always manual.

### Netlify

Netlify is a hosting service that allows you to connect a GitHub account and choose repositories (and branches!) to deploy from.

To deploy a site, open Netlify's main menu (once you're logged in), and click `New site from Git`. You'll first be directed to a menu to choose where you'd like to deploy from (choose GitHub). You'll then be directed to a menu where you can choose which repository you'd like to deploy. Once you select a repository, you'll be able to add build and deploy options (such as environment variables, and which directory to deploy from if you have a larger application). Netlify will then trigger a build, which will either publish, or fail.

Netlify also has built in continuous integration, so the site may fail to publish if there are warnings or errors in your React application.

#### Pros and Cons
Pros:
- Integrated with Git, so pushes to GitHub also deploy your site.
- Easy environment variable and deploy option setup.
- Free SSL certificates can be added via Let's Encrypt.

Cons:
- Automatic integration with GitHub can result in accidental builds.
- Does not integrate with multiple GitHub accounts.
- Will sometimes fail to publish if there are warnings or errors.
