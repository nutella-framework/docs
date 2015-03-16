# Core framework-level nutella APIs
Similartly to run-level core APIs, these framework-level core APIs handle the initialization of framework-level components.

```
nutella.f.init( broker_hostname, component_id )
```
Similarly to the run-level `nutella.init`, this method allows developers to initialize a framework level component. Some utility methods are provided to assist with the retrieval/parising of `broker_hostname` and `component_id`. 

```
nutella.f.parse_component_args(args)
nutella.f.parseURLParameters()`
```
Returns the `broker_hostname` as parsed from the command line or the URL query parameters. See the [run-level equivalent](core.md).

```
nutella.f.extract_component_id()
```
See the [run-level equivalent](core.md).

```
nutella.f.set_resource_id( resource_id )
nutella.f.setResourceId(resourceId)
```
See the [run-level equivalent](core.md).