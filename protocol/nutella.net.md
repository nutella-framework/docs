# nutella.net
This module handles all the communication of a certain component with the rest of the application. nutella offers both a pull strategy to retrieve information (i.e. request/response) and a push strategy (i.e. publish/subscribe). 

_Note: All the APIs here use `snake_case` (as it's customary in Ruby) but can be easily adapted to `camelCase` (as it is customary in JavaScript). Run level and application level APIs are shipped in `nutella_lib` while framework level APIs are only available in Ruby and are shipped with the framework itself. Also, for brevity, we don't include both synchronous and aynchronous requests, but only the last one._

## APIs
1. `nutella.net.subscribe(channel, callback(message, component_id, resource_id))`
1. `nutella.net.subscribe(filter, callback(message, channel, component_id, resource_id))`
1. `nutella.net.unsubscribe(channel/filter)`
1. `nutella.net.publish(channel, message)`
1. `nutella.net.sync_request(channel, request)`
1. `nutella.net.async_request(channel, request, callback(response))`
1. `nutella.net.handle_requests(channel, callback(request, component_id, resource_id))`

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

### From field
When a message is sent from a certain API level it will have a `from` field that identifies the level of the component that sent it and the unique identified of that component. Let's recap:
- Message sent from a run level component: `run::app_id/run_id/component_id/[resource_id]`
- Message sent from an application level component: `app::app_id/component_id/[resource_id]`
- Message sent from a framework level component: `framework::component_id/[resource_id]`

`run`, `app` and `framework` are fixed strings.

### Pull strategy (i.e. request/response) (NEEDS UPDATING!!!)
A request will look something like this:
```json
{
  "id": "req_res_id",
  "from": "component_id/resource_id",
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
  "from": "component_id/resource_id", 
  "type": "response", 
  "payload": {
    "user": "defined",
    "key-value": "pairs"
  }
}
```

Where `id` must be a unique-ish string or a number that is used to identify the request/response pair. This is the way a client will know which response matches which request. `from` is composed of two portions: the `component_id` followed, if it exists, by the `resource_id`. This way of identifying the sender of a message has been borrowed from [XMPP and its resourcepart](http://tools.ietf.org/html/rfc6122#section-2.4). The `resource_id` portion of the from is optional. `type` can only be either `request` or `response` and it is self explanatory. Finally, each request can (optionally) carry a `payload` that can be an arbitrary [JSON value](http://www.json.org/).

### Push (i.e. publish/subscribe)
A publish will look something like this:
```json
{
  "from": "component_id/resource_id", 
  "type": "publish",
  "payload": {
    "user": "defined",
    "key-value": "pairs"
  }
}
```
`from` and `payload` follow the same rules of request and response above, while `type` is always `publish`.

