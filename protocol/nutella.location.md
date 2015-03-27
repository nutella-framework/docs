# nutella.location

This module handle all the the physical resources that are present inside the room. The main goal of this module is provide an easy and reliable way for tracking the location of all those resources inside the room. Resources can be created, updated and deleted using the APIs provided. Every resource contains the actual location and a vector of key pairs that you can use in order to store data that can be accessed in a distributed way.

## Location APIs
Resource list

` nutella.location.resources `

Resource type:

` nutella.location.resource['rid'] `

Continuous tracking system:

` nutella.location.resource['rid'].continuous.x `

 ` nutella.location.resource['rid'].continuous.y `
 
Discrete tracking system:

` nutella.location.resource['rid'].discrete.x `

` nutella.location.resource['rid'].discrete.y `

Proximity tracking system with continous base station:

` nutella.location.resource['rid'].proximity.continuous.x `

` nutella.location.resource['rid'].proximity.continuous.y `

Proximity tracking system with discrete base station (read only):

` nutella.location.resource['rid'].proximity.rid `

` nutella.location.resource['rid'].proximity.continuous.x `

` nutella.location.resource['rid'].proximity.continuous.y `

Key - value pairs access (read/write):

` nutella.location.resource['rid'].parameters ` list of all the keys of parameters

` nutella.location.resource['rid'].parameter['key'] `

Receive update when the resource change

` nutella.location.resource['rid'].notifyUpdate ` set true for receiving update on the status of the resource

Receive update when a dynamic resource enter/exit the range

` nutella.location.resource['rid'].notifyEnter ` set true for receiving update when a resource enter in the range

` nutella.location.resource['rid'].notifyExit ` set true for receiving update when a resource exit the range
