## Engine-Gateway

All routing to Microservices registered in Innovaccer realm can be accessed through `Engine-gateway` package. This package provides [API Gateway](/fundamental/concepts?id=api-gateway) functionality for Engine base frontend applications.

The package system has a gateway function that passes along the package object to the main gateway file typically server/gateways/apidefs.js By default the Package Object is passed to the routes along with the other arguments

```js
MyPackage.gateway(app, datastore, 'dataflows');  
// defined in skeletonEngine/config/env/<dev,prod,test>.js
```

So a package in Engine ecosystem can register API definition for a Microservice that uses Innovaccer Dataflow Service.


```js
'use strict';

module.exports = function (Package, app, datastore, service) {

  const Pipeline = datastore.service(service).model(
    'Pipeline', ['id', 'name', 'schedule', 'settings', 'createdBy',
      'createdOn', 'flow', 'mode', 'dataflow', 'status', 'timeElapsed',
      'lastRun', 'pastRuns', 'session', 'contextStatus', 'spark_config'
    ]
  );  // datastore.service('someservice').model provides a Model Builder , 
      // which is used here to register a Data Model for  Pipeline

  let pipelineModelBuilder = ({
    pipeline_id,
    pipeline_name,
    dataflow_name,
    dataflow_id,
    created_by,
    created_on,
    schedule,
    flow,
    status,
    last_run,
    time_elapsed,
    context_created,
    settings,
    past_runs,
    mode,
    spark_config,
    session
  }) => {
    return Pipeline({
      id: pipeline_id,
      name: pipeline_name,
      dataflow: {
        name: dataflow_name,
        id: dataflow_id
      },
      createdBy: created_by,
      createdOn: created_on,
      schedule: schedule ? schedule : {},
      settings: settings,
      flow: flow,
      status: status,
      lastRun: last_run,
      timeElapsed: time_elapsed,
      contextStatus: context_created,
      session: session,
      pastRuns: past_runs,
      mode: mode ? mode : 'development',
      spark_config: spark_config
    });
  };


  let translators = [{
    method: 'GET',
    pattern: [
      '/pipelines/dataflow_([0-9]+$)'
    ], // here we have to define REGEX patterns for API endpoints we are consuming , 
       // along with translate function which uses Pipeline model defined above to 
       // map the data to a frontend specific Data model.
    translate: function (data) {
      var pipelines = data.map(pipelineModelBuilder);
      return pipelines;
    }
  }, {
    method: 'GET',
    pattern: [
      '/pipelines/pipeline_([0-9]+\/$)'
    ],
    translate: function (data) {
      return pipelineModelBuilder(data.data);
    }
  }];

  Package.addTranslator(service, translators);
};

```

Once you have defined Gateway definition for the backend service, you have to inject  gateway as dependency in your module and invoke activate method on gateway package with our current package as argument

```js
 gateway.activate(Dataflow);
```

This will register all the configuration provided by the `CurrentPackage.translators`, and use the regex and method to define an `express` route for the pattern. 

See the example below for details.

```js
/*
 * Defining the Package
 */
const Engine = require('Engine-core');
const Module = Engine.Module;
const config = Engine.getConfig();
const path = require('path');
const Dataflow = new Module('pipeline');

/*
 * All Engine packages require registration
 * Dependency injection is used to define required modules
 */

Dataflow.register((app, datastore, gateway, admin) => {
 
  const Router = require('express').Router();
  Dataflow.routes(app, datastore);
  Dataflow.gateway(app, datastore, 'dataFlow');
  Dataflow.angularDependencies([
    'dataflowBootstrap',
    'shapeValidator'
  ]);
  Dataflow.menus.add({
    title: 'Settings',
    link: '/dataflows/',
    roles: ['menu.can_dataflow_management', 'admin.can_platform_config', 'menu.can_view_smartpipelines', 'menu.can_view_system'],
    weight: 17,
    name: 'settings',
    menu: 'care'
  });
  Dataflow.menus.add({
    title: 'Dataflows',
    link: '/dataflows/',
    roles: ['menu.can_dataflow_management'],
    weight: 17,
    name: 'dataflows',
    menu: 'settings'
  });
  Dataflow.events.on(['starter', 'test'], function (ev) {
    console.log('invokkedf from starter');
    console.log(ev);
    Dataflow.events.emit('ack', true);
  });
  gateway.activate(Dataflow);
  return Dataflow;
});
```

Now navigate to `/api/Engine/gateways/`, you can see list of all API endpoints mapped to registered API