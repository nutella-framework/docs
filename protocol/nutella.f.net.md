## Channels
- Framework level channel: `/nutella/apps/<app_id>/runs/<run_id>/<run_channel>` (no restriction of name)


### Net (framework level)
- `publish_to_f(channel, message)`. Publishes `message` to the framework level channel `/nutella/channel`. 
- `subscribe_to_f(channel, cb(message, from))`. Subscribes to the framework level channel `/nutella/channel`. The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)).
- `request_to_f(channel, request, cb(response))`. Makes a request to the framework level channel `/nutella/channel`. 
- `handle_requests_on_f(channel, callback(request, from))`. Handles requests on the framework level channel `/nutella/channel`. 

### Net (down-graded to app level)
- `publish_to_app(app_id, channel, message)`. Publishes `message` to the app level channel `/nutella/apps/app_id/channel`. 
- `subscribe_to_app(app_id, channel, cb(message, from))`. Subscribes to the app level channel `/nutella/apps/app_id/channel`. The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)).
- `request_to_app(app_id, channel, request, cb(response))`. Makes a request to the app level channel `/nutella/apps/app_id/channel`. 
- `handle_requests_on_app(app_id, channel, callback(request, from))`. Handles requests on the app level channel `/nutella/apps/app_id/channel`. 

### Net (down-graded to app level) broadcasting methods
- `publish_to_all_apps(channel, message)`. Publishes `message` to the same app level channel (`/nutella/apps/+/channel`) for all `app_id`s.
- `subscribe_to_all_apps(channel, cb(message, app_id, from))`. Subscribes to the same app level channel (`/nutella/apps/+/channel`). The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)) for all `app_id`s.
- `request_to_all_apps(channel, request, cb(response))`. Makes a request to the same app level channel (`/nutella/apps/+/channel`) for all `app_id`s.
- `handle_requests_on_all_apps(channel, callback(request, app_id, from))`. Handles requests on the same app level channel (`/nutella/apps/+/channel`) for all `app_id`s.

### Net (down-graded to run level)
- `publish_to_run(app_id, run_id, channel, message)`. Publishes `message` to the run level channel `/nutella/apps/app_id/runs/run_id/channel`.
- `subscribe_to_run(app_id, run_id, channel, cb(message, from))`. Subscribes to the run level channel `/nutella/apps/app_id/runs/run_id/channel`. The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)).
- `request_to_run(app_id, run_id, channel, request, cb(response))`. Makes a request to the run level channel `/nutella/apps/app_id/runs/run_id/channel`.
- `handle_requests_on_run(app_id, run_id, channel, callback(request, from))`. Handles requests on the run level channel `/nutella/apps/app_id/runs/run_id/channel`.

### Net (down-graded to run level) broadcasting methods
- `publish_to_all_runs(channel, message)`. Publishes `message` to the same run level channel (`/nutella/apps/+/runs/+/channel`) for all `run_id`s.
- `subscribe_to_all_runs(channel, cb(message, app_id, run_id, from))`. Subscribes to the same run level channel (`/nutella/apps/+/runs/+/channel`) for all `run_id`s. The `from` parameter in the callback is a hash (i.e. object in JavaScript, Hash in Ruby, HashMap in Java,...) containing the type of component that sent the `message` and the fields of the `from` that are set (see [above](Components-communications#from-field)).
- `request_to_all_runs(channel, request, cb(response))`. Makes a request to the same run level channel (`/nutella/apps/+/runs/+/channel`) for all `run_id`s.
- `handle_requests_on_all_runs(channel, callback(request, app_id, run_id, from))`. Handles requests to the same run level channel (`/nutella/apps/+/runs/+/channel`) for all `run_id`s.