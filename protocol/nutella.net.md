# nutella.net
This module handles all the communication of a certain component with the rest of the application. nutella offers both a pull strategy to retrieve information (i.e. request/response) and a push strategy (i.e. publish/subscribe). 

## APIs
1. `nutella.net.subscribe(channel, callback(message, component_id, resource_id))`
1. `nutella.net.subscribe(filter, callback(message, channel, component_id, resource_id))`
1. `nutella.net.unsubscribe(channel/filter)`
1. `nutella.net.publish(channel, message)`
1. `nutella.net.sync_request(channel, request)`
1. `nutella.net.async_request(channel, request, callback(response))`
1. `nutella.net.handle_requests(channel, callback(request, component_id, resource_id))`

All the methods handle messages in an **asynchronous** way, except for `synch_request` that does not. This doesn't mean that the methods themselves must be asynchronous. For instance, `nutella.net.subscribe` will register a callback that will be asynchronously fired whenever a message is received. However the method itself could be synchronous or asynchronous, meaning that `subscribe` could not return until the subscription is successful or it could return immediately and fire a callback whenever the subscription completes successfully. nutella doesn't impose any restrictions on this.  

## Channels
One of the goals of the nutella library is to isolate each run from the others. This means that whenever calling one of the methods above, nutella will "pad" the specified `channel` with the `run_id`. For instance, if in my bot code I call:
```ruby
subscribe( "my_channel", lambda do |message| 
  do_something_with( message )
end)
```
nutella will actually subscribe to channel "run_id/my_channel" in order to keep all the communication of this run isolated from all other runs. This has also the advantage that, if I want to listen to all the channels in a run, all I have to do is subscribe to channel `my_run_id/#`.

## Messages formats
All messages are in JSON. The protocol imposes minimal mandatory fields. 

### Pull strategy (i.e. request/response)
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





# nutella.net.bin (STUB)
This is the binary files counterpart of `nutella.net`. It implements methods that are similar to the ones in `nutella.net` but allow binary files (i.e. images, videos, audio and any arbitrary file) to be transferred between components.

1. `nutella.net.bin.subscribe(channel, callback(file_url, metadata)`
1. `nutella.net.bin.subscribe(filter, callback(file_url, channel, metadata))`
1. `nutella.net.bin.unsubscribe(channel/filter)`
1. `nutella.net.bin.publish(channel, binary_file, metadata)`

The first thing to notice is the absence of requests. The reason for this is that we couldn't find a suitable use case for them so we decided to leave them out.

The second thing to notice is that publish and subscribe are not "symmetrical" anymore. Publish takes a `binary_file` as an input while the callback in subscribe takes a `file_url`. This might change in the future since it was heavily inspired by the web but, native applications, might need a way of downloading the file in addition to uploading it. The other challenge the still needs to be solved is how to indicate what type of resource each URL corresponds to but that might be the place where metadata becomes helpful. 