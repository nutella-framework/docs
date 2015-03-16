# nutella protocol and APIs

The nutella protocol is designed to be modular with a core and several modules that implement specific functionalities. This page describes the nutella protocol together with some guidelines for designing language-specific libraries that implement the protocol. It's important to point out that each language specific nutella library, should follow the style and the conventions that are customary in that specific language while striving to implement all the APIs outlined here and maintaining consistency across languages.

**Note 1: At the moment of writing we are looking to fully support Ruby ([nutella_lib.rb](https://github.com/nutella-framework/nutella_lib.rb)), JavaScript ([nutella_lib.js](https://github.com/nutella-framework/nutella_lib.js)) and Swift ([nutella_lib.swift](https://github.com/nutella-framework/nutella_lib.swift))** 

**Note 2: Many portions of this specification are not yet finalized so take the whole thing with a grain of salt**

## Run-level APIs
These are the APIs are implemented by `nutella_lib.*` and are used for almost everything when designing a new nutella application. They are capable of handling the communication between run-level components (i.e. regular components) and interact with framework level functions, such as logging and location.
* [nutella](core.md) Core module
* [nutella.net](nutella.net.md) Communication module
* [nutella.log](nutella.log.md) Logging module
* [nutella.persist](nutella.persist.md) Bots persistence module.
* [nutella.location](nutella.location.md) Location module (a.k.a. RoomPlaces)

## Application-level APIs
Application-level APIs are used by application-level components, that is the components that are shared among all the runs in a certain application. These APIs also ship with `nutella_lib.*` and are available to nutella application designers that need to implement application level components.
* [nutella.app](nutella.app.md) Application-level core module
* [nutella.app.net](nutella.app.net.md) Application-level communication module
* ...

## Framework-level APIs
Framework level APIs are used by framework-level components. They don't ship in `nutella_lib.*` but, instead, are "buried" inside the nutella framework. The reason for this is that only designers of framework level components should care and be able to use them.
* [nutella.f](nutella.f.md) Framework-level core module
* [nutella.f.net](nutella.f.net.md) Framework-level communication module
* ...

