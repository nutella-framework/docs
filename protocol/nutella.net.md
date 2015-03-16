# nutella.net
This module handles all the communication of a certain component with the rest of the application. nutella offers both a pull strategy to retrieve information (i.e. request/response) and a push strategy (i.e. publish/subscribe). 

_Note: All the APIs here use `snake_case` (as it's customary in Ruby) but can be easily adapted to `camelCase` (as it is customary in JavaScript). Run level and application level APIs are shipped in `nutella_lib` while framework level APIs are only available in Ruby and are shipped with the framework itself. Also, for brevity, we don't include both synchronous and asynchronous requests, but only the last one._

## APIs
1. `nutella.net.subscribe(channel, callback(message, from))`
1. `nutella.net.subscribe(filter, callback(message, channel, from))`
1. `nutella.net.unsubscribe(channel/filter)`
1. `nutella.net.publish(channel, message)`
1. `nutella.net.sync_request(channel, request)`
1. `nutella.net.async_request(channel, request, callback(response))`
1. `nutella.net.handle_requests(channel, callback(request, from))`

All the methods handle messages in an **asynchronous** way, except for `synch_request` that does not. This doesn't mean that the methods themselves must be asynchronous. For instance, `nutella.net.subscribe` will register a callback that will be asynchronously fired whenever a message is received. However the method itself could be synchronous or asynchronous, meaning that `subscribe` could not return until the subscription is successful or it could return immediately and fire a callback whenever the subscription completes successfully. nutella doesn't impose any restrictions on this in an effort to be as idiomatic as possible.

## Channels
One of the goals of the nutella library is to isolate each run from the others. This means that whenever calling one of the methods above, nutella will "pad" the specified `channel` with the `app_id` and `run_id` passed to `nutella.init`. For instance, if in my bot code I call:
```ruby
subscribe( "my_channel", lambda do |message| 
  do_something_with( message )
end)
```
nutella will actually subscribe to channel `/nutella/apps/<app_id>/runs/<run_id>/my_channel` in order to keep all the communication of this run isolated from all other runs. 

## Messages formats
All messages are in JSON. The protocol imposes minimal mandatory fields. 


### Pull strategy (i.e. request/response)
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

Where `id` must be a unique-ish value that is used to identify the request/response pair. This is the way a client will know which response matches which request. `type` can only be either `request` or `response` and it is self explanatory. Each request and/or response can (optionally) carry a `payload` that can be an arbitrary [JSON value](http://www.json.org/).

#### `from` field
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
The `type` identifies the type of component that sent the message: this can be `run`, `app` or `framework`. `app_id` and `run_id` identify the application and the run respectively and the `component_id` identifies the component. Finally, there might be a `resource_id` field (optional) that identifies All fields are mandatory, except for `resource_id`.



### Push (i.e. publish/subscribe)
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

