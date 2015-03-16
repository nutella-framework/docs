# Component types
nutella has three types of components: run (R), application (A) and framework (F). Let's see how they communicate with each other but first let's review the characteristics of each one of them.

## Run level components
Run level components exist within the context (or scope) of a single run. A run is meant to represent an instance of an application running in a certain classroom at a certain moment. Each run level component is initialized with `app_id`, `run_id`, `component_id`. You can optionally set a `resource_id` which is used to identify on which resource a certain interface is running. This is the most common type of component and is addressed by the simple APIs that we designed already. 

## Application level components
Application level components exist within the context (or scope) of a single application. All runs of a certain application could potentially reference the same component at the application level. This also means that application level components need to be able to address multiple run level components individually if they need to. Moreover, application level components need to communicate among each other. Each application level component is initialized with `app_id` and `component_id` and, optionally, `resource_id`.

## Framework level components
Framework level components exist within the context (or scope) of a the whole nutella framework. All applications and all runs of a certain application could potentially reference the same component at the framework level. This also means that framework level components need to be able to address multiple application level and run level components if they need to. Of course framework level components need to communicate with other framework level components. Each framework level component is initialized with `component_id` and, optionally, `resource_id`.


# Channels hierarchy
The key to enabling components at different level to communicate with each other is to organize MQTT channels hierarchically in order to keep framework, application and run context fully separated. We define three categories of channels: framework level, application level and run level. Framework level channels are hierarchically at a higher level than application level channels which are, in turn, at an higher level than run level channels. Therefore `framework > application > run`.
- Run level channel: `/nutella/<framework_channel>` (`framework_channel` must be different than the string `apps`)
- Application level channel: `/nutella/apps/<app_id>/<app_channel>` (`app_id` must be different than the string 'runs' and `app channel` must be different than `runs`)
- Framework level channel: `/nutella/apps/<app_id>/runs/<run_id>/<run_channel>` (no restriction of name)

`apps` and `runs` are fixed strings.

Framework channels are used by framework level components to communicate with each other. They are only accessible to framework level APIs. Application channels are used by application level components to communicate with each other. They are accessible via framework level APIs and application level APIs. Run channels are used by run level bots to communicate with each other. They are accessible to framework level APIs, application level APIs and run level APIs.

Note how:
- Framework level APIs can communicate at the framework, application and run level. 
- Application level APIs can communicate at the application and run level. 
- Run level APIs can only communicate at the run level.



# Communication matrix
This matrix summarizes how communication between different components at different levels happens.

### `R <-> R` communications
Happen on R level channels. 

Uses the run level APIs that publish, subscribe, request and handle requests on run level channels. 
### `A <-> A` communications
Happen on A level channels. 

Uses application level APIs that publish, subscribe, request and handle requests on application level channels. 

### `F <-> F` communications
Happen on F level channels. 

Uses framework level APIs that publish, subscribe, request and handle requests on framework level channels. 

### `A <-> F` communications
Happen on A level channels. 

Application components simply use application level APIs. 

Framework level components use framework level APIs down-graded to application level channels. They do this by setting `app_id` which allows to address individual application components. 

### `R <-> A` communications
Happen on R level channels.

Run components simply use run level APIs. 

Application level components use application level APIs down-graded to run level channels. They do this by setting `run_id` which allows to address individual run components.

### `R <-> F` communications
Happen on R level channels.

Run components simply use run level APIs.

Framework components need to use framework level APIs down-graded to run level channels. They do this by setting both `app_id` and `run_id` that allow the framework level component to address individual run components. 

