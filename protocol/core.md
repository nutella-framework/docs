# Core run-level nutella APIs
The core nutella API allows developers to configure a set of parameters that are used by all the other protocol modules withing a certain component.


```
nutella.init( broker_hostname, app_id, run_id, component_id )
```
The first thing someone using the nutella library in a nutella component needs to do is to call the `init` method to initialize the library, connect to the MQTT broker, set the `app_id`, `run_id` and `component_id`. 

* `app_id` its the unique identifier of the application. Each application MUST have a different `app_id`. Typical values for this are `wallcology`, `roomquake`, `beesim`,... 
* `run_id` its the unique identified of the run. A run is an instance of a certain application and is typically associated to a classroom. Runs are the way nutella handles running the same application multiple time.
* `component_id` its the unique identifier of the component we are working on within the nutella application.

Depending on the language and the platform the component is using, the values of these parameters can be retrieved in different ways. So far we have three possible scenarios.

### Bots
The `broker_hostname`, `app_id` and `run_id` are passed to the bot as command line parameters by the nutella framework, while the `component_id` is simply the name of the bot itself. The nutella library offers some utility methods to help programmers retrieve these values:
* `nutella.parse_args( args )` this method parses and returns `broker_hostname`, `app_id` and `run_id` from the command line
* `nutella.extract_component_id()` this method extracts and returns the `component_id` from the name of the bot

### Web interfaces
The `broker_hostname`, `app_id` and `run_id` are passed to the interface as query parameters either by the nutella main interface or by RoomCast. `component_id` is hard-coded by the developers in their component code. The nutella library ships with a `nutella.parseURLParameters()` function that returns `broker_hostname`, `app_id` and `run_id` together with any other URL query parameters specified by the developer.

### Native (iOS) interfaces
Similarly to web interfaces, the `broker_hostname`, `app_id` and `run_id` are passed to the interface as [custom URL scheme](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html#//apple_ref/doc/uid/TP40007072-CH6-SW1) query parameters either by the nutella main interface or by RoomCast. `component_id` is hard-coded by the developers in their component code.



```
nutella.set_resource_id( resource_id ) # for us Ruby lovers

nutella.setResourceId(resourceId) // for all of you JavaScript fans
```
This method is used to set the `resource_id` _whenever a component has multiple instances_. For instance, an interface could be running on multiple resources (i.e. devices). Each one of such resource or instances needs to be properly identified in order to make it addressable by other components.


