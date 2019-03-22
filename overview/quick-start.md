# Geting Started with Engine

---

## Prerequisite Technologies

### Linux

- _Node.js_ - <a href="http://nodejs.org/download/">Download</a> and Install Node.js. We recommend node-8.x as the preferred node version to run Engine.
- _MongoDB_ - <a href="https://www.mongodb.org/downloads">Download</a> and Install mongodb - <a href="https://docs.mongodb.org/manual/">manual</a>.

```bash
wget https://nodejs.org/dist/v8.9.4/node-v8.9.4-linux-x64.tar.xz

sudo tar -C /usr/local --strip-components 1 -xf node-v8.9.4-linux-x64.tar.xz
```

- _Git_ - Get git using a package manager or <a href="http://git-scm.com/downloads">download</a> it.

### OSX

- _Node.js_ - <a href="http://nodejs.org/download/">Download</a> and Install Node.js or use the packages within brew or macports.
- _MongoDB_ - Follow the tutorial here - <a href="https://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/">Install mongodb on OSX</a>
- _git_ - Get git <a href="http://git-scm.com/download/mac">from here</a>.

---

## Prerequisite packages

- Engine currently uses gulp and webpack as a build tools and npm to manage javascript packages.

- Point NPM Config to Engine NPM registry, for more info check <a href="http://52.35.230.55">this</a> url

```
$ npm install -g gulp webpack@3.7.0
```

## Installation and Starting a new Project

To create a new UI-Engineun <code>git clone <Engine> </code>. Name for the application is optional.
If no name is provided, "Engine" is used. The Engine project will be cloned from Git into a directory of the application name.

```bash
$ git clone git@git.innovaccer.com:Engine-Engine/Engine.git [name]
$ cd [name] && npm install
```

  <p class="alert alert-info">Note: <a href="http://git-scm.com/downloads">git</a> must be installed for this command to work properly.</p>

### Invoke node with a task manager

Engine supports the npm wrapped `gulp` and `webpack` task runner for various services which are applied on the code.
To start your application run -

```bash
$ npm run dev
```

Alternatively, when not using `gulp` (and for production environments) you can run:

```bash
$ sudo docker-compose up
```

```bash
$ export NODE_ENV=production
$ npm run build
$ npm run start
```

Then, open a browser and go to:

```bash
http://localhost:3000
```

## Running on a different port

If you have a rails, node, or other Engine project already running, you may need to use a different port.
You can set the port and start your new Engine project with one command:

```bash
$ export PORT=3001 && npm run start
```

Then, open a browser and change the port number before you visit:

```bash
  http://localhost:3001
```

## Troubleshooting

During installation depending on your os and prerequisite versions you may encounter some issues.

Most issues can be solved by one of the following tips, but if you are unable to find a solution feel free to drop a line on #Engine-frontend

### Update NPM or Gulp

Sometimes you may find there is a weird error during install like npm's _Error: ENOENT_. Usually updating those tools to the latest version solves the issue.

- Updating NPM:

```bash
$ npm update -g npm
```

- Updating Gulp:

```bash
$ npm update -g gulp webpack@3.7.1
```

- Updating Webpack:

```bash
$ npm update -g webpack
```

### Cleaning NPM cache

NPM has a caching system for holding packages that you already installed.
We found that often cleaning the cache solves some troubles this system creates.

- NPM Clean Cache:

```bash
$ npm cache verify
```

## Technologies

<dl class="dl-horizontal">
<dt>MongoDB</dt>
<dd>Go through <a href="https://www.mongodb.com/">MongoDB Official Website</a> and proceed to its <a href="https://docs.mongodb.com/manual">Great Manual</a>, which should help you understand NoSQL and MongoDB better.</dd>
<dt>Express</dt>
<dd>The best way to understand express is through its <a href="http://expressjs.com/">Official Website</a>, particularly <a href="http://expressjs.com/">The Express Guide</a>; you can also go through <a href="http://stackoverflow.com/q/8144214/6115857">this StackOverflow thread</a> for more resources.</dd>
<dt>SingleSpa</dt>
<dd>Check out <a href="https://single-spa.js.org">SingleSpa</a> a small library which we use to maintain the lifecyle of single page applications inside our app shell</dd>
<dt>AngularJS</dt>
<dd>Angular's Official Website is a great starting point.</dd>
<dt>ReactJS</dt>
<dd>Official <a href="https://reactjs.org">ReactJS</a> website is a good place to start learning more about react </dd>
<dt>Node.js</dt>
<dd>Start by going through <a href="https://nodejs.org">Node.js Official Website</a> and the <a href="https://nodejs.org/en/docs/">documentation page</a> as well as <a href="http://stackoverflow.com/q/2353818/6115857">this StackOverflow thread</a>, which should get you going with the Node.js platform in no time.</dd>
</dl>

### Additional Tools

- <a href="http://mongoosejs.com/">Mongoose</a> - The mongodb node.js driver in charge of providing elegant mongodb object modeling for node.js
- <a href="http://passportjs.org/">Passport</a> - An authentication middleware for Node.js which supports authentication using a username and password, Facebook, Twitter, and more.
- <a href="https://webpack.js.org/">Webpack</a> - webpack is a module bundler for modern JavaScript applications. When webpack processes your application, it recursively builds a dependency graph that includes every module your application needs, then packages all of those modules into a small number of bundles - often only one - to be loaded by the browser.

## Packages

Everything in Engine is a package and when extending Engine with custom functionality make sure you create your own package and do not alter the core packages.

The Engine package system allows developers to create modular code that provides useful tools that other Engine developers can use. The packages, when published, are plug-and-play and are used in a way very similar to traditional npm packages.

The Engine package system integrates all the packages into the Engine project as if the code was part of Engine itself and provides the developers with all the necessary tools required to integrate their package into the host project.

## Registering a Package

In order for a Package to work it needs to be registered. By doing this you make the package system aware that you are ready and that other packages are able to depend on you. The packages are registered from within `app.js`.

When registering you are required to declare all your dependencies in order for the package system to make them available to your package.

```javascript
// Example of registering the MyPackage
MyPackage.register(function(app, auth, database) {
  // ...
});
```

Engine has 4 pre registered dependencies:

- `app` Makes the express app available .
- `passport` Includes some basic authentication functions
- `database` Contains the Mongoose database connection
- `datastore` Contains the Microservice Request instance for services specified in Engine config

> All dependencies specified must be registered in order to use them

## Dependency Injection

> An injection is the passing of a dependency (a service) to a dependent
> object (a client). The service is made part of the client's state.
> Passing the service to the client, rather than allowing a client to
> build or find the service, is the fundamental requirement of the
> pattern. [Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)

Dependency injection allows you to declare what dependencies you require and rely on the package system to resolve all dependencies for you.
Any package registered is automatically made available to anyone who would like to depend on them.
Looking again at the registration example we can see that `MyPackage` depends on the `Tokens` package and can make use of its full functionality, including overriding it.

```javascript
// Example of registering the tokens package
MyPackage.register(function(app, auth, database, tokens) {
  // I can make use of the tokens within my module
  tokens.someExampleFunction("some parameter");

  // I can override functions
  tokens.someExampleFunction = function(param) {
    //my custom logic goes here
  };
});
```

> Packages when in code are used in a capitalized form

## File Structure

The file structure is similar to that of the Engine project itself

Fundamental Files at the root of the package

```bash
├── app.js
├── build
├── Engine.json
    ├── menus.json
├── package.json
├── public
│   └── assets
├── pwa
│   ├── ngapp
│   └── starter
├── README.md
├── server
│   ├── routes
│   └── tests
├── ui
│   ├── ngRoot.js
│   ├── packages
│   └── src
└── webpack.starter.config.js
```

### Server

Packages are registered in the `app.js` Defines package name, version and Engine=true in the `package.json`

All of the Server side code resides in the /server directory.

server/
--- config # Configuration files
--- controllers # Server side logic goes here
--- models # Database Schema Models
--- gateways # Relay routes registered for datastore services
--- routes # Rest api endpoints for routing
--- views # Ejs based html rendering

### Client

All of the Client side code resides in the /ui directory.

ui/
--- assets # JavaScript/CSS/Images (not aggregated)
--- controllers # Angular controllers
--- config # Contains routing files
--- services # Angular services (also directive and filter folders)
--- views # Angular views

All JavaScript within ui is transpiled through webpack, and placed in `public` directory of the package.

Files within the `public` directory of the package can be accessed externally at `/[package-name]/path-to-file-relative-to-public`.

### Angular Modules and Dependencies

> This will be deprecated in future release

Every package registration automatically creates a corresponding angular module of the form `Engine.[package-name]`

The package system injects this information into the Engine init functions and allows developers to base their controllers, services, filters, directives etc on either an existing module or on their own one.

In addition you are able to declare which angular dependencies you want your angular module to use.

Below is an example of adding an angular dependency to our angular module.

```javascript
// Example of adding an angular dependency of the fooBar to the
MyPackage.angularDependencies(["fooBar"]);
```

## Settings Object

The settings object is a persistence object that is stored in the packages collection and allows for saving persistent information per package such as configuration options or admin settings for the package.

Receives two arguments the first being the settings object the second is a callback function

```javascript
MyPackage.settings({ someSetting: "some value" }, (err, settings) => {
  // You will receive the settings object on success
});

// Another save settings example this time with no callback
// This writes over the last settings.
MyPackage.settings({ anotherSettings: "some value" });

// Get settings. Retrieves latest saved settings
MyPackage.settings((err, settings) => {
  // You now have the settings object
});
```

> Each time you save settings you overwrite your previous value.
> Settings are designed to be used to store basic configuration options
> and should not be used as a large data store

## Express Routes

All routing to server side controllers is handled by express routes. The package system uses the typical express approach. The package system has a route function that passes along the package object to the main routing file typically `server/routes/myPackage.js`

By default the Package Object is passed to the routes along with the other arguments
`MyPackage.routes(app, auth, database);`

Example from the `server/routes/myPackage.js`

```javascript
// The Package is past automatically as first parameter
module.exports = function(MyPackage, app, auth, database) {
  // example route
  app.get("/myPackage/example/anyone", function(req, res, next) {
    res.send("Anyone can access this");
  });
};
```

## Menu System

Packages are able to hook into an existing menu system and add links to various menus integrated within Engine.

Each link specifies its `title`, `template`, `menu` and `role` that is allowed to see the link. If the menu specified does not exist, a new menu will be created. The menu object is made accessible within the client by Engine from a _menu angular service_ that queries the menu controller for information about links.

Below is an example how to add a link to the main menu from `app.js`

```javascript
//We are adding a link to the main menu for all authenticated users
MyPackage.menus.add({
  title: "myPackage example page",
  link: "myPackage example page",
  roles: ["authenticated"],
  menu: "main"
});
```

## HTML View Rendering

The packages come built in with a rendering function allowing packages to render static html.
The default templating Engine is _ejs_.
The views are found in `server/views` of the package and should end with the `.html` suffix

Below is an example rendering some simple html

```javascript
app.get("/myPackage/example/render", function(req, res, next) {
  MyPackage.render("index", { packageName: "myPackage" }, function(err, html) {
    //Rendering a view from the Package server/views
    res.send(html);
  });
});
```

Every Package also provides the ability to do very efficient template inheritance and render content on server side through good old `ejs` templating Engine.

In order to compose a server side page, each package provides a `viewer` method, which is a bounded function to access ejs views located in the current module only.

```javascript
const view = Ingraph.viewer();
app.use(function(req, res, next) {
  res.locals._ = _;
  res.locals.loggedin = req.isAuthenticated();
  res.locals.foo = Ingraph.viewer();
  res.locals.genIngraphUrl = genUrl;
  res.locals.dateFormat = dateFormat;
  next();
});
const common = {
  main: view("pages/std"),
  top: view("partials/layout/top"),
  bottom: view("partials/ingraph/bottom"),
  nav: view("partials/layout/nav")
};
const pages = {
  common: common,
  ingraph: _.extend({}, common, {
    top: view("partials/layout/top"),
    panel: view("partials/ingraph/panel"),
    appnav: view("partials/ingraph/appnav"),
    bottom: view("partials/ingraph/bottom")
  }),
  dashboards: _.extend({}, common, {
    top: view("partials/layout/top"),
    panel: view("partials/dashboards/panel"),
    appnav: view("partials/ingraph/appnav"),
    bottom: view("partials/ingraph/bottom")
  }),
  statics: {}
};
```

We can now pass this `pages` object to our `Package.routes` function

```javascript
Ingraph.routes(app, datastore, database, admin, pages);
```

Inside our `routes` directory, we things organised in following manner

```javascript
"use strict";

const Engine = require("Engine-core");
module.exports = function(Module, app, datastore, database, admin, pages) {
  const index = require("../controllers").ingraphManager(
    Module,
    datastore,
    pages,
    admin
  );
  const ingraphRouter = require("express").Router();
  const esiPageRouter = require("express").Router();
  const dashboardRouter = require("express").Router();
  const config = Module.config;

  esiPageRouter.route("/:page").get(
    admin.authFilter,
    (req, res, next) => {
      req.query.submenu = true;
      return next();
    },
    admin.controllers.settings,
    admin.controllers.menus,
    index.ingraphLoggedIn,
    index.ingraphRender
  );

  /**
   * Get dashboards list
   */
  dashboardRouter.route("/").get(
    admin.authFilter,
    (req, res, next) => {
      req.query.submenu = true;
      return next();
    },
    admin.controllers.settings,
    admin.controllers.menus,
    index.ingraphLoggedIn,
    index.getIngraphDashboardsPage
  );

  ingraphRouter
    .route("/settings")
    .get(admin.authFilter, admin.controllers.settings, admin.getSettings)
    .post(admin.authFilter, admin.controllers.settings, admin.updateSettings);

  app.use("/app/ingraph", esiPageRouter);
  app.use("/app/dashboards", dashboardRouter);
  app.use(`/api/${config.gatewayPrefix}/ingraph`, ingraphRouter);
};
```

We have two server side rendered pages, `/app/ingrpah/` and `/app/dashboards/`
powered by their respective `controllers`

```javascript
const Engine = require("Engine-core");

/**
 * Context menu for dashboards
 */
const CONTEXT_MENU = {
  drillInto: "drill into",
  drill: "drill"
};

/**
 * Filter drill down dashboards
 * @param {Object} dash dashboard
 */
const filterDrillDashboards = dash => {
  return (
    dash.title.toLowerCase().indexOf(CONTEXT_MENU.drill) === -1 &&
    dash.title.toLowerCase().indexOf(CONTEXT_MENU.drillInto)
  );
};

const ingraphManager = (Module, datastore, pages, admin) => {
  /**
   * Render ingraph panel
   */
  const ingraphRender = (req, res, next) => {
    let page = req.params.page ? `app/${req.params.page}` : `app/main#/home`;
    req.esiOptions = {
      rejectUnauthorized: true,
      headers: {
        Cookie: req.ingraphToken
      }
    };

    let args = {
      locals: {
        config: Module.config
      },
      partials: pages.ingraph,
      page: page,
      care: req.resolved.care ? req.resolved.care : [],
      baseref: req.query.base ? req.query.base : "/app"
    };

    Module.viewHelpers.htmlResponse(req, res, pages.ingraph, args, next);
  };

  /**
   * Get ingraph dashboards panel
   */
  const getIngraphDashboardsPage = (req, res, next) => {
    let args = {
      locals: {
        config: Module.config.clean
      },
      partials: pages.dashboards,
      ingraphDashboards: {},
      care: req.resolved.care ? req.resolved.care : [],
      baseref: req.query.base ? req.query.base : Module.config.app.base
    };

    Promise.all([
      datastore.service("ingraph").get("/api/v1/navver", {
        headers: {
          Cookie: req.ingraphToken
        }
      }),
      Module.getSettings()
    ])
      .then(([result, settings]) => {
        result.dashboards = result.dashboards.filter(filterDrillDashboards);
        args.ingraphDashboards = result;
        args.settings = settings || {};
        Module.viewHelpers.htmlResponse(req, res, pages.dashboards, args);
      })
      .catch(next);
  };

  return {
    ingraphRender,
    getIngraphDashboardsPage
  };
};

module.exports = ingraphManager;
```

We need to pass an object to ejs render function, with all the variables that we want to access inside our html template in the following manner

```js
let args = {
  locals: {
    config: Module.config
  },
  partials: pages.ingraph,
  page: page,
  care: req.resolved.care ? req.resolved.care : [],
  baseref: req.query.base ? req.query.base : "/app"
};
```

Every module has a viewHelpers property wich exposes `htmlResponse` `render`, `renderAll` methods.

```js
Module.viewHelpers.htmlResponse(req, res, pages.ingraph, args, next);
```

Using `htmlResponse` method to render our templates, it gives us ability to do partial rendering of any part of the html template over the ajax call in the following way `http://Engine.jupiter.internal.tech/app/dashboards/?&partials[]=panel` , which returns

```js
  {"panel": "<div>.....rendered html of panel</div>"}
```

where `?partials[]=` can contain the components defined in page object

```js
// ?partials[]=top&partials[]=panel&partials[]=bottom
const pages = {
  dashboards: _.extend({}, common, {
    top: view("partials/layout/top"),
    panel: view("partials/dashboards/panel"),
    appnav: view("partials/ingraph/appnav"),
    bottom: view("partials/ingraph/bottom")
  })
};
```

## Creating your own package
To create your own package and scaffold its initial code, you need to install 

```bash
$ npm i -g Engine-generator
```
run the following command:

```bash
➜  ~ create-Engine-app shinyproduct

  warning: creating app 

   create : Engine-shinyproduct/
   create : Engine-shinyproduct/public/
   create : Engine-shinyproduct/public/assets/
   create : Engine-shinyproduct/package.json
   create : Engine-shinyproduct/Engine.json

   change directory:
     $ cd Engine-shinyproduct

  Ensure to be at Engine root path for installing dependency and runnig app

   install dependencies:
     from Engine root path run:
       $ npm run postinstall

   build ui:
       $ npm run build

   run the app:
     from Engine root path run:
     $ npm run dev

➜  ~ 

```

Follow the instructions provided by the tool to get you new module up and running inside `Engine-Engine`


## Contributing your package
Once your package is in good shape and you want to share it with the world you can start the process of contributing it and submitting it so it can be included in the package repository.

## Config
All the configuration is specified in the [config](/config/) folder,
through the [env](config/env/) files, and is orchestrated through the [Engine-core]() NPM module.



## Environmental Settings

There is a shared environment config: __all__

* __root__ - This the default root path for the application.
* __http.port__ - This sets the default application port.
* __https__ - These settings are for running HTTPS / SSL for a secure application.
* __port__ - This sets the default application port for HTTPS / SSL. If HTTPS is not used then is value is to be set to __false__ which is the default setting. If HTTPS is to be used the standard HTTPS port is __443__.
* __ssl.key__ - The path to public key.
* __ssl.cert__ - The path to certificate.

There are three environments provided by default: __development__, __test__, and __production__.
Each of these environments has the following configuration options:

* __db__ - This is where you specify the MongoDB / Mongoose settings
* __url__ - This is the url/name of the MongoDB database to use, and is set by default to __UI-Engine-dev__ for the development environment.
* __debug__ - Setting this option to __true__ will log the output all Mongoose executed collection methods to your console.  The default is set to __true__ for the development environment.
* __options__ - These are the database options that will be passed directly to mongoose.connect in the __production__ environment: [server, replset, user, pass, auth, mongos] (http://mongoosejs.com/docs/connections.html#options) or read [this] (http://mongodb.github.io/node-mongodb-native/driver-articles/mongoclient.html#mongoclient-connect-options) for more information.
* __app.name__ - This is the name of your app or website, and can be different for each environment. You can tell which environment you are running by looking at the TITLE attribute that your app generates.
* __emailFrom__ - This is the from email address displayed when sending an email.
* __mailer__ - This is where you enter your email service provider, username and password.

To run with a different environment, just specify NODE_ENV as you call gulp:
```bash
$ NODE_ENV=test gulp
```
If you are using node instead of gulp, it is very similar:
```bash
$ NODE_ENV=test node server
```
To simply run tests
```bash
$ npm test
```
> NOTE: Running Node.js applications in the __production__ environment enables caching, which is disabled by default in all other environments.

## Logging

A control over the logging format has been delegated to the env configuration files.
The formats and implementation are done using the morgan node module and it's [predefined format](https://github.com/expressjs/morgan#predefined-formats)
Within each configuration file (config/env/development.js) for instance you state the format in the 'logging' object.
```
'use strict';

module.exports = {
  db: 'mongodb://' + (process.env.DB_PORT_27017_TCP_ADDR || 'localhost') + '/Engine-dev',
  debug: true,
  logging: {
    format: 'tiny'
  },
```

The default for the development environment uses [tiny format](https://github.com/expressjs/morgan#tiny)
```
GET /system/views/index.html 304 2.379 ms - -
GET /admin/menu/main 304 8.687 ms - -
GET /system/assets/img/logos/UI-Enginelogo.png 304 2.803 ms - -
GET /system/assets/img/backgrounds/footer-bg.png 304 4.481 ms - -
GET /system/assets/img/logos/Engine.png 304 3.001 ms - -
```

The production uses the widely used [combined format](https://github.com/expressjs/morgan#combined).
```
:1 - - [22/Mar/2015:13:13:42 +0000] "GET /modules/aggregated.js HTTP/1.1" 200 - "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36"
::1 - - [22/Mar/2015:13:13:42 +0000] "GET /modules/aggregated.js?group=header HTTP/1.1" 200 0 "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36"
::1 - - [22/Mar/2015:13:13:42 +0000] "GET / HTTP/1.1" 200 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36"
::1 - - [22/Mar/2015:13:13:42 +0000] "GET /modules/aggregated.css HTTP/1.1" 200 - "http://localhost:3000/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36"
```

## Staying up to date
After initializing a project, you'll see that the root directory of your project is already a git repository. Engine uses git to download and update its own code. To handle its own operations, Engine creates a remote called `upstream`. This way you can use git as you would in any other project.

To update your Engine app to the latest version of Engine

```bash
$ git pull upstream master
$ npm install
```

To maintain your own fork and add your repository as remote. See here for information on [adding an existing project to GitHub](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/).

```bash
$ git remote add origin <remote repository URL>
$ git push -u origin master
```


## Hosting Engine

```bash
  $ docker-compose build # tag the image
  $ docker-compose push # point and push to private docker registry
```
Go to target server, docker and docker-compose are essentials and should be installed on the target servers.

```bash
  $ git clone some:git@//gitserver.com/somenewproject.git 
  $ cd somenewproject 
  $ sudo docker-compose up -d # to run docker in detached mode
```

This will start a docker image at `localhost:3031` by default, now this could be proxies behind `nginx` to render the frontend on `https://www.xyz.innovaccer.com`
