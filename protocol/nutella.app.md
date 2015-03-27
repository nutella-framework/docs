# Core application-level nutella APIs
Similartly to run-level core APIs, these app-level core APIs handle the initialization of application-level components. If you are familiar with run-level APIs you'll find application-level APIs pretty intuitive to understand.

## `init`
```
nutella.app.init( broker_hostname, app_id, component_id )
```
Similarly to the run-level `nutella.init`, this method allows to initialize an application level component. Some utility methods are provided to assist with the retrieval/parising of `broker_hostname`, `app_id` and `component_id`. 

```
nutella.app.parse_args(args)
nutella.app.parseURLParameters()`
```
Returns the `broker_hostname` and `app_id` as parsed from the command line or the URL query parameters. See the [run-level equivalent](core.md).

```
nutella.app.extract_component_id()
```
See the [run-level equivalent](core.md).

## Set resource ID`
```
nutella.app.set_resource_id( resource_id )
nutella.app.setResourceId(resourceId)
```
See the [run-level equivalent](core.md).
