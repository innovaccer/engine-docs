# Engineering resilient Micro Frontends

Over the last decade, AngularJS has been one of the "coolest kid on the block”, substituting several of the weaknesses that came with jQuery-based net engineering solutions. Many enterprises were extraordinarily happy building their fashionable  net portals with AngularJS, and with the exponential growth of net businesses, AngularJS created several developers’ lives easier.

Everyone was happy in the web development world, but this happiness didn’t last long.

## A Big Ball of Mud and JavaScript

>While much attention has been focused on high-level software architectural patterns, what is, in effect, the de-facto standard software architecture is seldom discussed. This paper examines this most frequently deployed of software architectures: the BIG BALL OF MUD. A BIG BALL OF MUD is a casually, even haphazardly, structured system. Its organisation, if one can call it that, is dictated more by expediency than design. Yet, its enduring popularity cannot merely be indicative of a general disregard for architecture.- http://www.laputan.org/mud/

We started Developing Innovaccer's healthcare data platform, during November 2015, at that time our application architecture consisted of a Single Page Application, written in Angular1.3 as during that time React was relatively new to build out a complex product for healthcare and everybody in the team was mostly well versed with AngularJs, also it was relatively easier to find developers experienced with the technology at that time.
During the time of its creation, our application had a well-defined architecture. The relentless onslaught of adjusting needs that any eminent system attracts will step by step undermine its structure. Systems that were once tidy become overgrown as piecemeal growth gradually allows elements of the system to sprawl in an uncontrolled fashion.
This system started showing unmistakable signs of unregulated growth, and repeated, expedient repair.
Information is shared promiscuously among distant components of the system, usually to the purpose wherever nearly all the necessary info becomes world or duplicated. The overall structure of the system was well defined, it was eroded beyond recognition.

> Continuously refactoring toward deeper insight

As with our decaying system, a downward spiral ensues. Since the system has become harder and harder to understand, maintenance has become more expensive, and more difficult. The way to arresting entropy in the software package is to refactor it. A sustained commitment to refactoring can keep a system from subsiding into a chaotic state.
If such sprawl continues intense, the structure of the system will become so badly compromised that it should be abandoned.

Rewriting the whole frontend in React or Vue was not an option for us, specially in the modern JavaScript ecosystem , which is highly volatile and ever changing trends we wanted to create an architecture that can be agnostic of frontend framework being used by a particular team to build their web interface, and provide a scaffolding to include any of the existing frontend frameworks or if something better comes down the line, without shredding down the existing application completely.

In our endeavour to refactor our existing single page monolith into a more elegant and performant architecture that is nimble in nature, we ended up creating UI Engine, which solves the complexity of engineering large scale javascript applications, that offers a flexible yet strict enforcement of certain essential rules, that are compulsory to follow as a precursor to building resilient web applications, that critical business like healthcare can rely on and are easier to test, maintain and change and secure.

> TLDR: In recent years, thanks to Node.js, JavaScript has become the “lingua franca” of the web for both front and backend applications. This has given rise to awesome projects like Angular, React and Vue which improve developer productivity and enable the construction of fast, testable, and extensible frontend applications. However, while plenty of superb libraries, helpers, and tools exist for Node (and server-side JavaScript), none of them effectively solve the main problem - architecture.[1]

Engine is an [Inversion of Control Container](http://martinfowler.com/articles/injection.html "Inversion of Control Containers and the Dependency Injection pattern") that solves the problem of architecture for Large Scale Complex Javascript Applications.

> Loose coupling allows you to make changes to one module without affecting the others.

Writing JavaScript is very easy, almost anyone can learn and start developing User Interface with JavaScript or jQuery, AngularJS, React , Vue etc, however, the difficult part is writing maintainable JavaScript.

We deployed this our refactored frontend application, by migrating each angularjs applications as a small micro-frontends, inside App Shell architecture provided by UI Engine, and all the network calls which were initially being triggered as cross origin from the browser to our backend services, was now proxied through API gateway registered in UI engine.

After some more tweaking, enabling HTTP/2 on nginx and compression middleware on node.js layer to compress all the json and static resources, below are some screenshots of the first deployment on staging.innovaccer.com that we did in April 2018 compared to our legacy SinglePage AngularJS application on qa.innovaccer.com.

<img src="./micro-frontends/performance.png" alt="drawing" height="320" width="600"/>

<div style="page-break-after: always;"></div>

# Microfrontends

>Software architecture should never be an end goal, but a means to an end

The economy is powered by the bytes today and in byte economy, the focus is on quickly bringing product to market.
In competitive and disrupting decade of startups where we see software companies becoming some of the world's most valuable companies ever created, startups spawns and dies every day. To stay alive, sustain and gain substantial chunk of market share we want the factory running at top speed to produce software. These factories consist of sentient human coders, who are working relentlessly to churning out feature after feature to deliver a user story which is a composite part of total structure of software product.

## Microservices and Distributed Big Balls of Mud

In the beginning....

[![Evolution](./micro-frontends/evo.png)](link)

We have ancient monolithic systems, where everything is bundled up inside a single deployable unit.
This is in all probability wherever most of the trade is. Caveats apply, however monoliths may be designed quickly and area unit simple to deploy, but they provide limited agility because even tiny changes require a full redeployment. We additionally understand that monoliths usually find yourself trying sort of a huge ball of mud as a result of the means that software system usually evolves over time. For example, several monolithic systems are engineered employing a stratified design, and it's comparatively simple for stratified architectures to be abused (e.g. skipping "around" a service to access the repository/data access layer directly).

The application we are working on is a big client facing web application. Since the initial conception of the product, we have identified a couple of self-contained features and created micro services to provide each functionality. We have carved out bare essentials for providing the user interface, which is our public facing web frontend. This micro service only has one functionality which is providing the user interface. It may be scaled and deployed become independent from the composite backend services.

>If you can't build a monolith, what makes you think microservices are the answer?

If we talk about micro services in technical sense, compute, storage and network has become dirt cheap today, and the cost is rapidly declining, this trend has led to rise of development of tiny, independent full stack software which is simply the evolution of light-weight Service Oriented Architectures if done right.

Microservices have rejuvenated the age old idea of building smaller, loosely coupled , reusable piece of software that does one thing and one thing well, emphasising on shortest time to market and minimal cost. Again, caveats apply however, if done well, service-based architectures buy you a lot of flexibility and agility because each service can be developed, tested, deployed, scaled, upgraded and rewritten separately, particularly if the services area unit decoupled via asynchronous electronic messaging. The draw back is raised complexness as a result of your software currently has more moving elements than a stone.

> The complexity is still there, you're just moving it somewhere else.

Thus the same old concept just replace all in-memory function calls or shared library calls with remote network calls, now we can independently build, change, deploy and scale them with independent teams, who don’t have to be compelled to understand the existence of different teams.

When you have an enormous monolithic frontend that can’t be split simply, you have to think about making it smaller. You can decompose the frontend into separate parts developed separately by completely different groups.

>When you are implementing a microservices architecture you want to keep services small. This should also apply to the frontend. If you don't, you will only reap the benefits of microservices for the backend services. An easy solution is to split your application up into separate frontends.

We have multiple teams that work on different applications. However, you're not quite there yet: The frontend is still a monolith that spans the different backends. This means on the frontend you still have some of the same problems you had before switching to microservices. The image below shows a simplification of the current architecture.

<img src="./micro-frontends/fe-backend.png" alt="drawing" style="width:600px;height:350px"/>

Backend teams can't deliver business value without the frontend being updated since an API without a user interface doesn't do much. More backend groups suggests that a lot of new options, and therefore more pressure is put on the frontend team(s) to integrate new features.

To compensate for this it is possible to make the frontend team bigger or have multiple teams working on the same project. Because the frontend still has to be deployed in one go, teams cannot work independently. Changes have to be integrated in the same project and the whole project needs to be tested since a change can break other features.This would basically mean that the teams are not working independently.

<img src="./micro-frontends/fe-required.png" alt="drawing" style="width:600px;height:350px"/>

With a monolithic frontend you never get the pliability to scale across groups as assured by microservices. Besides not being able to scale, there is also the classical overhead of a separate backend and frontend team. Each time there is a breaking change in the API of one of the services, the frontend has to be updated. Especially when a feature is added to a service, the frontend has to be updated to ensure your customers can even use the feature.

If you have a frontend small enough it can be maintained by a team which is also responsible for one or more services which are coupled to the frontend. This means that there is no overhead in cross team communication. But because the frontend and the backend can not be worked on independently, you are not really doing microservices.

<img src="./micro-frontends/mfe-one-company.png" alt="drawing" style="width:240px;height:250px"/>

If you do have multiple teams working on your platform, but you were to have multiple smaller frontend applications there would have been no problem. Each frontend would act as the interface to one or more services. Each of these services will have their own persistence layer. This is known as vertical decomposition.
Now the major problem with achieving this kind of architecture with frontend is User Experience
End users of the modern application product today, have this perception of one company means one website.
However, as we discussed above, this approach becomes a development bottleneck and doesn't scale efficiently.

We will discuss some of the most popular ways to do the vertical decomposition for the frontend in order to achieve the following objectives:

* Team Ownership
* Develop Independently
* Run Independently
* Technology Agnostic
* Fast Loading
* Native Support
* Sharing Basics
* Modular
* Corporate Identity
* Smooth User Interaction

<!-- <div style="page-break-after: always;"></div> -->

## Hardcore Nginx Based Routing

What we can do if we want to get started with splitting our monolithic frontend Single Page Application , into multiple standalone Single Page Applications served behind the nginx, running independently.  

[![Evolution](./micro-frontends/mfe-nginx.png)](link)

We can hyperlink different applications, however each application would require to maintain similar Base Application templates in there code in order to achieve brand identity.

As you can see, this approach is okay to start with however four of the very critical cases are failing here.

| Passed              | Failed            |
| :-------------        |:-------------        |
| Team Ownership        | Sharing Basics       |
| Develop Independently | Modular              |
| Run Independently     | Corporate Identity   |
| Technology Agnostic   | Smooth User Interface|
| Fast Loading          |                      |
| Native Support        |                      |
|                       |                       |

So what other options we have ?

<!-- <div style="page-break-after: always;"></div> -->

## Server Side Includes

There is another intersting apporach we can use to achieve this, most popularly known as Edge Side Includes or [ESI](https://en.wikipedia.org/wiki/Edge_Side_Includes)

> Edge Side Includes or ESI is a small markup language for edge level dynamic web content assembly.

[![Evolution](./micro-frontends/mfe-esi.png)](link)

| Pass                  | Failed               |
| :-------------        |:-------------         |
| Team Ownership        |   Fast Loading        |
| Develop Independently |   Native Support      |
| Run Independently     | Smooth User Interface |
| Technology Agnostic   |                       |
| Sharing Basics        |                       |
| Modular               |                       |
| Corporate Identity    |                       |
|                       |                       |

<!-- <div style="page-break-after: always;"></div> -->

## Integration on Code Level

Well, this is how our existing frontend monolith is working, where we do code level integration of multiple angular modules into a final SPA build.

<img src="./micro-frontends/mfe-integrate.png" alt="drawing" style="width:640px;height:350px"/>

| Pass                  | Failed               |
| :-------------        |:-------------         |
| Team Ownership        |   Fast Loading        |
| Develop Independently |   Technology Agnostic |
| Native Support        |   Run Independently   |
| Smooth User Interface |                       |
| Sharing Basics        |                       |
| Modular               |                       |
| Corporate Identity    |                       |
|                       |                       |

Obviously, we have some workarounds which could help but this approach also fails to be sustianable in the long run.

<div style="page-break-after: always;"></div>

## App Shell

> Lets change the perspective a bit, and think outside the box

There is a good intro about this approach [here](https://developers.google.com/web/fundamentals/architecture/app-shell) , which should set the context about this concept.

[![Evolution](./micro-frontends/mfe-appshell.png)](link)

> The app "shell" is the minimal HTML, CSS and JavaScript required to power the user interface and when cached offline can ensure instant, reliably good performance to users on repeat visits. This means the application shell is not loaded from the network every time the user visits. Only the necessary content is needed from the network.

This approach lends ability to instantly load our application shell on first visit and the minimal amount of static resources required is cached on the browser.

Now, we can lazy load independent single page applications knowns as micro frontends into our shell as per user demand or intent.

[![Evolution](./micro-frontends/mfe-appshell-load.png)](link)

We can do this by providing routing information for each micro frontend.

[![Evolution](./micro-frontends/mfe-appshell-routing.png)](link)

Followed by providing manifest json for each micro-frontend.

[![Evolution](./micro-frontends/mfe-appshell-manifest.png)](link)

Once, we have loaded all the necessary resources for the application we can initialize the micro-frontend application in following way

[![Evolution](./micro-frontends/mfe-appshell-init.png)](link)

If we evaluate this apporach on our test cases

<!-- [![Evolution](./micro-frontends/app_shell_cases.png)](link) -->
| Pass                  |  Challenges           |
| :-------------        |:-------------         |
| Team Ownership        |  Modular              |
| Develop Independently |  Technology Agnostic  |
| Native Support        |  Sharing Basics       |
| Smooth User Interface |  Run Independently    |
| Super Fast Loading    |                       |
| Corporate Identity    |                       |
|                       |                       |

With this, App shell felt like most appropriate approach to solving our frontend problem.

Engine is designed from the ground up to to leverage Application Shell architecture, We achieve this by incorporating Design Pattern known as Inversion Of Control or IOC containers on Browser and Nodejs layer, which help are applications to do Dependency Injection instead of doing direct source-code imports, this pattern helps us to build applications that provide low coupling and high cohesion.

Hence with UI Engine, developers can build their micro frontends and each application can be coupled with a server part that provides view level RESTful APIs or exposes certain downstream services via API gateways that power applications registered in App Shell.

<div style="page-break-after: always;"></div>

# UI Engine

> Loose coupling allows you to make changes to one module without affecting the others.

Engine is a pluggable component based application composition layer, it provides a well-defined place for creating, configuring, and *non-invasively* connecting together the components of an application, or sections of an application.

> **Web Application Module**  - an independent unit of functionality that is a part of total structure of web application.

With Engine, you focus on coding the application logic of components and let Engine handle the bootstrapping and the glue that connects them together.You write simple, declarative Javascript Modules that describes how components should be composed together, and wire will load, configure, and connect those components to create an application, and will clean them up later.

Engine is designed to take care of the connection points between existing popular frameworks and solve common integration problems that arises with engineering large scale complex Javascript WebApplications, thus decoupling the whole application with implementation details of each application vertical, giving freedom to choose UI stack from the likes of Angular, React, Vue, Mithril etc.

Engine incorporates an [Inversion of Control Container](http://martinfowler.com/articles/injection.html "Inversion of Control Containers and the Dependency Injection pattern") for Javascript apps.

> TLDR: In recent years, thanks to Node.js, JavaScript has become the “lingua franca” of the web for both front and backend applications. This has given rise to awesome projects like Angular, React and Vue which improve developer productivity and enable the construction of fast, testable, and extensible frontend applications. However, while plenty of superb libraries, helpers, and tools exist for Node (and server-side JavaScript), none of them effectively solve the main problem - architecture.[1]

## Features

Engine provides:

* Simple, declarative dependency injection
* A flexible, non-invasive connection infrastructure
* Application lifecycle management
* Powerful core tools and plugin architecture for integrating popular frameworks and existing code.
* Application shell Architecture and pluggable Micro Frontends.
* Support for both browser and server environments.

Apps constructed with Engine:

* Have a high degree of modularity
* Can be unit tested easily, because they inherently separate application logic from application composition
* Allow application structure to be refactored independently from application logic
* Have no explicit dependencies on DOM Ready, DOM query engines, or DOM event libraries
* It is designed to give you a quick and organised way to start developing micro-frontends inside PWA shell.
* Encourages age-old idea of building smaller, loosely coupled, reusable piece of software that does one thing and one thing well for quicker time to market and cheaper cost of change.
* The engine package system allows developers to create modular code that provides useful tools that other engine developers can use. The packages, when published, are plug-and-play and are used in a way very similar to traditional npm packages.
* The engine package system integrates all the packages into the engine project as if the code was part of the engine itself and provides the developers with all the necessary tools required to integrate their package into the host project.
* Setup can be fan out to run as **Distributed Frontend** architecture.

> engine provides an out-of-the-box application architecture which allows for effortless creation of highly testable, scalable, loosely coupled, and easily maintainable largescale frontend web applications as set of independent micro frontends.

Engine was developed as a very light and elegant layer which permitted us to migrate our existing Frontend monolith(Angular1.x) into separately installable packages. Each package can now be installed separately into engine , each package can provide a complete Frontend along with Rest-APIs for that engine application into a plug and play application framework.

If any module in Engine, depends on any other functionality module in engine there will be no explicit source code level dependency but we use Dependency Injection to use the functionality exposed by a particular module.

Code snippet attached below , describes how to define a Package in engine.

```js
const engine = require('engine-core');
const Module = engine.Module;
const InGraph = new Module('ingraph');//  Defining the Package
const ESI = require('nodesi').middleware;
/*
 * All engine packages require registration
 * Dependency injection is used to define required modules
 */
InGraph.register((app, datastore, database, gateway, admin, sources, worksets) => {
  app.use(ESI(config.esiSettings));
  InGraph.menus.add({
    title: 'InGraph',
    link: '/app/ingraph/main#/home',
    weight: 19,
    name: 'ingraph',
    menu: 'care'
  });
  InGraph.routes(app, datastore, database, admin);
  return InGraph;
});
```

Engine Provides us ability to do sort of vertical decomposition without completely abandoning our existing system, rather than improving the performance of the existing Angular application, along with the ability to develop new features and rewrite existing features to more modern and performance oriented engine Library such as React , Preact, Vue, Svelte etcs.

## Engine Test Cases

<!-- [![Evolution](./micro-frontends/app_shell_engine.png)](link) -->

| Passed                |  Failed               |
| :-------------        |:-------------         |
| Team Ownership        |  Run Independently    |
| Develop Independently |                       |
| Native Support        |                       |
| Smooth User Interface |                       |
| Super Fast Loading    |                       |
| Corporate Identity    |                       |
| Sharing Basics        |                       |
| Modular               |                       |
| Sharing Basics        |                       |
| Technology Agnostic   |                       |
|                       |                       |  

Engine provides a nice and familiar ecosystem to every JavaScript developer to build, publish and install their micro-frontends into any engine based projects using natively provide NPM cli tool in a true plug and play format.

[![Evolution](./micro-frontends/engine_npm.png)](link)

All the applications created for engine along with any JavaScript module which needs to be reused or plug n play are published to a private NPM registry hosted inside our network.

<div style="page-break-after: always;"></div>

## A flexible and powerful yet simple architecture

[![Evolution](./micro-frontends/stiching-layer.png)](link)

So far, we have been able to break down our large legacy UI monolith into standalone micro applications that can be used like traditional npm packages, as each engine package is a web application middleware.

The application shell provided by UI engine, works as a stitching layer, as it composes the seamless UI from individual packages and a dockerized image is published for the UI.

In order to run each engine packages as standalone micro application, thus fanning out in a distributed fashion, we need to understand the main components which answer the essential requirements of micro frontend architecture stated below.

### Client-Side

* Orchestration
* Routing
* Isolation of micro applications
* App to app communication
* Consistency between micro applications UIs

### Server-side

* Server Side rendering
* Routing
* Dependency management

In order to address the requirements of the client side, we have three essential structures provided by UI engine: PWAManager, Loader, Router and one extra UI Engine Store.

[![Evolution](./micro-frontends/pwamanager.png)](link)

### PwaManager

  PwaManager is the core of the client side micro application orchestration. The main functionality of the PwaManager is to create a dependency treee. Once all the depdencies of the micro application are resolved, PwaManager starts the micro application.

### Loader

  Loader is one of the very essential parts of client side solution offered by UI engine, it is the responsibility of the loader to fetch unresolved micro applications from the server.

### Router

  In order to solve the client side routing problem, UI engine provides a Router, the router is primarily used to resolve micro applications, by handling top level routing for each application and delegating the further process to respective micro application. Let's say we have an application with the URL like `/sources/view/123`  and an app named SourcesApp. In this scenario, UI engine Router will resolve the URL upto `/sources/*` and it will call SourcesApp with `/view/123` part.

### Store

  The store is used to solve the problem of communication between multiple applications on the client side, this store is modeled along the lines of Redux.

### Microappserver

[![Evolution](./micro-frontends/microappserver.png)](link)

The Microappserver is responsible for initializing and  serving the micro application.
Whenever a micro application server is spawned, the first thing it does is call register endpoint provided by stitching server with application manifest, which defines the dependencies, type and URL schema.

### Stiching Server

Stitching Server provides a register hook for MicroAppServers. once a MicroAppServer registers itself to StichingServer, Stitching Server records the manifest of the MicroAppServer.

Later the StitchingServer uses the manifest declaration to resolve the MicroAppServers from the requested uniform resource locator.

After resolution a MicroAppServer and every one of its dependencies, all relative methods in CSS, JS and hypertext mark-up language are prefixed with connected MicroAppServer public uniform resource locator. One further step is prefixing the CSS selectors with a singular symbol of MicroAppServer to stop collision between micro applications on client-side.

Then the most responsibility of StitchingServer comes into the scene: composing and returning a seamless hypertext mark-up language page from all collected components
Try Other Relevant Tools


[![Evolution](./micro-frontends/stiching-layer-lld.png)](link)

## Conclusion

Micro frontends is a relatively new terminology, crowned as recent as 2016, however there have been a lot of large companies, who have tried to solve similar problems like Facebook with its [BigPipe](https://www.facebook.com/notes/facebook-engineering/bigpipe-pipelining-web-pages-for-high-performance/389414033919/).

Zalando open-sourced its solution which is called Project Mosaic.

There is a framework already out there called [single-spa](https://single-spa.js.org).

The topic micro frontends is being discussed quite a lot, web-components based development strategies has been gaining substantial momentum and I believe this topic will be discussed more frequently in time.

Over the coming years,I hope this will become the defacto way of development in large teams.

### Resources
Readers should go through this [presentation](https://www.slideshare.net/nzakas/scalable-javascript-application-architecture) by Nicholas Zakas, who has been inspiration and motivation behind Engine.

[Gain momentum on the road to a new long-lasting and future-proof frontend architecture!](https://micro-frontends.zeef.com/elisabeth.engel?ref=elisabeth.engel&share=ee53d51a914b4951ae5c94ece97642fc)

[Youtube Playlist on Microfrontends](https://www.youtube.com/playlist?list=PLI1AtZo9B3YL_xpi19IuxFcTuCi2_thQT)