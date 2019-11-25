# nutella communication protocol
This document describes the communication protocol implemented on top of MQTT that is used by nutella.


# Components Communication Layers (CCLs)
In the nutella ecosystem, every interface or bot (i.e. each _component_) can run in one of these 3 layers: _run (R)_, _application (A)_, and _framework (f)_.

### Run level components
Run level components exist within the context (or scope) of a single run. A run is meant to represent an instance of an application running in a certain classroom at a certain moment. Each run level component is initialized with `app_id`, `run_id`, `component_id`. You can optionally set a `resource_id` which is used to identify on which resource a certain interface is running. This is the most common type of component and is addressed by the simple APIs that we designed already. 

### Application level components
Application level components exist within the context (or scope) of a single application. All runs of a certain application could potentially reference the same component at the application level. This also means that application level components need to be able to address multiple run level components individually if they need to. Moreover, application level components need to communicate among each other. Each application level component is initialized with `app_id` and `component_id` and, optionally, `resource_id`.

### Framework level components
Framework level components exist within the context (or scope) of a the whole nutella framework. All applications and all runs of a certain application could potentially reference the same component at the framework level. This also means that framework level components need to be able to address multiple application level and run level components if they need to. Of course framework level components need to communicate with other framework level components. Each framework level component is initialized with `component_id` and, optionally, `resource_id`.


# Hierarchical organization of CCLs
The key to enabling components at different levels to communicate with each other is to organize MQTT channels hierarchically in order to keep framework, application and run contexts fully separated. We define three categories of channels: _framework level (FR)_, _application level (AR)_ and _run level (RL)_. Framework level channels are hierarchically at a higher level than application level channels which are, in turn, at an higher level than run level channels. Therefore `framework > application > run`.
Channels look like this:
- FL channels: `/nutella/<framework_channel>` (`framework_channel` must be different than the string `apps`)
- AL channels: `/nutella/apps/<app_id>/<app_channel>` (`<app_id>` must be different than the string `runs` and `<app_channel>` must be different than the string `runs`).
- RL channel: `/nutella/apps/<app_id>/runs/<run_id>/<run_channel>` (no restriction on channel name)

**To re-iterate: `apps` and `runs` are fixed and restricted strings!!!**

Framework channels are used by framework level components to communicate with each other. They are only accessible to framework level APIs. Application channels are used by application level components to communicate with each other. They are accessible via framework level APIs and application level APIs. Run channels are used by run level bots to communicate with each other. They are accessible to framework level APIs, application level APIs and run level APIs.

Note how:
- Framework level APIs can communicate at the framework, application and run level. 
- Application level APIs can communicate at the application and run level. 
- Run level APIs can only communicate at the run level.

The following diagram summaries the "communication matrix" between CCLs
```
┌─────────┐            ┌─────────┐
│Framework│─────────┬─▶│Framework│
└─────────┘         │  └─────────┘
    ┌─────┐         ├─▶┌─────┐
    │ App │━ ━ ━ ┳ ━│━▶│ App │
    └─────┘         │  └─────┘
    ┌─────┐      ┃  └─▶┌─────┐
    │ Run │       ━ ━ ▶│ Run │
    └─────┘═══════════▶└─────┘
```
- `F <-> F` communications happen on FL channels. Both components use FL APIs that publish, subscribe, request and handle requests on FL channels.

- `F <-> A` communications happen on AL channels. A components simply use AL APIs while F components use FL APIs down-graded to exchange messages on AL channels. They do this by setting `app_id`, which allows F components to address individual A components.

- `F <-> R` communications happen on RL channels. R components simply use RL API while F components use FL APIs down-graded to exchange messages on RL channels. They do this by setting both `app_id` and `run_id` which allow the F components to address individual R components.

- `A <-> A` communications happen on AL channels. Both components use AL APIs that publish, subscribe, request and handle requests on AL channels.

- `A <-> R` communications happen on RL channels. R components simply use RL APIs while A components use AL APIs down-graded to exchange messages on RL channels. They do this by setting `run_id` which allows the A components to address individual R components.

- `R <-> R` communications happen on RL channels. Both components use RL APIs that publish, subscribe, request and handle requests on RL channels.

## Broadcast
When F and A components operate in down-graded mode and exchange messages on on AL and RL cannels, they can choose to "broadcast" to multiple A and R components at the same time. They can do so by using the single-level wildcard character `+` from MQTT topics. There are 5 broadcasts modes available in nutella:
- F component broadcasting to all A components `/nutella/apps/+/<app_level_channel>`
- F components broadcasting to all R components in all apps `/nutella/apps/+/runs/+/<run_level_channel>`
- F component broadcasting to all R components within a specific app `/nutella/apps/<app_id>/runs/+/<run_level_channel>`
- F component broadcasting to all R components with an certain id for all apps`/nutella/apps/+/runs/<run_id>/<run_level_channel>` (this is a bit awkward to think about but is useful to broadcast to all RL components running within apps being run by a specific teacher)
- A components broadcasting to all R components in the app `/nutella/apps/my_app/runs/+/<run_level_channel>`


# Messages formats
All messages exchanged by nutella are in JSON. The protocol imposes minimal mandatory fields as described below.

## Pull strategy (i.e. request/response)
MQTT is a publish subscribe protocol so nutella has to implement its own semantic for request/response. The way it does it is by publishing a message to a certain channel and including an `id` in the payload. Once the message is sent, the nutella library stores the request id, subscribes to the same channel where the request was sent, and waits to get a message with a matching id.

A request will look something like this:
```json
{
    "id": "req_res_id",
    "from": {
        "type": "run",
        "run_id": "my_run_id",
        "app_id": "my_app_id",
        "component_id": "my_component_id",
        "resource_id": "my_resource_id"
    },
    "type": "request",
    "payload": {
        "user": "defined",
        "key-value": "pairs"
    }
}
```
and a response to the previous request, something like this:
```json
{
    "id": "req_res_id",
    "from": {
        "type": "run",
        "run_id": "my_run_id",
        "app_id": "my_app_id",
        "component_id": "my_component_id",
        "resource_id": "my_resource_id"
    },
    "type": "response",
    "payload": {
        "user": "defined",
        "key-value": "pairs"
    }
}
```

As mentioned above, `id` must be a unique value that is used to identify the request/response pair. This is the way a client will know which response matches which request. `type` can only be either `request` or `response` and it is self explanatory. Each request and/or response can (optionally) carry a `payload` that can be arbitrary JSON.

### `from` field
When a message is sent from a component of a certain level it will have a `from` field  that identifies that component uniquely. The content of such `from` field looks something like this:
```json
{
    "type": "run",
    "run_id": "my_run_id",
    "app_id": "my_app_id",
    "component_id": "my_component_id",
    "resource_id": "my_resource_id"
}
```
The `type` identifies the type of component that sent the message: this can be `run`, `app` or `framework`. `app_id` and `run_id` (optional for app and framework level components) identify the application and the run respectively and the `component_id` identifies the component. Finally, there might be a `resource_id` field (optional) that identifies the physical device a certain component is actually running on. This is useful whenever you are working with location.

## Push (i.e. publish/subscribe)
A publish will look something like this:
```json
{
    "from": {
        "type": "run",
        "run_id": "my_run_id",
        "app_id": "my_app_id",
        "component_id": "my_component_id",
        "resource_id": "my_resource_id"
    },
    "type": "publish",
    "payload": {
        "user": "defined",
        "key-value": "pairs"
    }
}
```
`from` and `payload` follow the same rules of request and response above, while `type` is always `publish`.

# RPC Protocol
Nutella is built a client-server application. The nutella CLI does nothing more than sending RPC requests to the server using the nutella protocol (on top of MQTT). The choice of using nutella protocol instead of bare MQTT is dictated by the fact that we want to leverage all the niceties that the nutella protocol gives us.

All RPCs are sent as _requests/responses_ over the _framework level_  `commands` channel.

Requests are essentially command names with options (did you say command line flags?) and look like this
```
{
  "command": "start",
  "opts": {
    "option_name": "option_value"
  }
}
```
Each request is matched by a response in the following format.
```
{
  "success": true,
  "message": "What to display to the user",
  "message_type": "error",
  "exception": "blob of text that contains the stacktrace"
}
```
Three fields, `success`, `message`, and `message_level` are always included, `exception` is optional and is only includeded if `success` is `false`. `message_level` indicates wether the message is `info`, `success`, `debug`, `warn`, or `error`.

# TODO
Talk about how higher level functionality is implemented on top of this basic stuff
- Metadata about nutella
- Run lists, app lists, etc.
- Storage (K/V etc.)
- Logging, etc, etc.