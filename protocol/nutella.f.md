# Core framework-level nutella APIs
Similartly to run-level core APIs, these framework-level core APIs handle the initialization of framework-level components.

## `init`
```
nutella.f.init( broker_hostname, component_id )
```
Similarly to the run-level `nutella.init`, this method allows developers to initialize a framework level component. 

Differently from run-level and applicaiton-level components, framework-level components have full access to the nutella configuration file and the list of all applications and instances running on a particular instance of nutella. They access these parameters with the internal APIs used by the framework. See [Nutella.config](https://github.com/nutella-framework/nutella_framework/blob/master/lib/config/config.rb) and [Nutella.runlist](https://github.com/nutella-framework/nutella_framework/blob/master/lib/config/runlist.rb) for a documentation of the respective APIs.

## Set resource ID

```
nutella.f.set_resource_id( resource_id )
nutella.f.setResourceId(resourceId)
```
Equivalent of the [run-level API](core.md).
