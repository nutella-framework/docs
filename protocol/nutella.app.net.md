## Channels
Application level channel: `/nutella/apps/<app_id>/<app_channel>` (`app_id` must be different than the string 'runs' and `app channel` must be different than `runs`)


### Net (application level)
- `publish_to_app(channel, message)`. Publishes `message` to the app level channel `/nutella/apps/app_id/channel`. 
- `subscribe_to_app(channel, cb(message, from))`. Subscribes to the app level channel `/nutella/apps/app_id/channel`. The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)).
- `request_to_app(channel, request, cb(response))`. Makes a request to the app level channel `/nutella/apps/app_id/channel`. 
- `handle_requests_on_app(channel, callback(request, from))`. Handles requests on the app level channel `/nutella/apps/app_id/channel`. 

### Net (down-graded to run level)
- `publish_to_run(run_id, channel, message)`. Publishes `message` to the run level channel `/nutella/apps/app_id/runs/run_id/channel`.
- `subscribe_to_run(run_id, channel, cb(message, from))`. Subscribes to the run level channel `/nutella/apps/app_id/runs/run_id/channel`. The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)).
- `request_to_run(run_id, channel, request, cb(response))`. Makes a request to the run level channel `/nutella/apps/app_id/runs/run_id/channel`.
- `handle_requests_on_run(run_id, channel, callback(request, from))`. Handles requests on the run level channel `/nutella/apps/app_id/runs/run_id/channel`.

### Net (down-graded to run level) broadcasting methods
- `publish_to_all_runs(channel, message)`. Publishes `message` to the same run level channel (`/nutella/apps/app_id/runs/+/channel`) for all `run_id`s.
- `subscribe_to_all_runs(channel, cb(message, from))`. Subscribes to the same run level channel (`/nutella/apps/app_id/runs/+/channel`) for all `run_id`s. The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)).
- `request_to_all_runs(channel, request, cb(response))`. Makes a request to the same run level channel (`/nutella/apps/app_id/runs/+/channel`) for all `run_id`s.
- `handle_requests_on_all_runs(channel, callback(request, from))`. Handles requests to the same run level channel (`/nutella/apps/app_id/runs/+/channel`) for all `run_id`s.
