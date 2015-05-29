To avoid confusion, in this page we'll describe the terminology used in the context of the nutella framework.

**Table of contents**
- [Applicaiton](Nutella-glossary#applicaiton)
- [Bot](Nutella-glossary#bot)
- [Component](Nutella-glossary#component)
- [Interface](Nutella-glossary#interface)
- [Run](Nutella-glossary#run)
- [Project](Nutella-glossary#project)
- [Template](Nutella-glossary#template)

***

### Application
Same as [Project](Nutella-glossary#project)

### Bot
A bot is a piece of software that interacts _exclusively_ with other components, not with users. This means that all inputs and outputs bots receive come from either other bots or interfaces. There are three kinds of bots: _framework bots_, _project bots_ and _run (or plain) bots_. Framework bots are shared by all projects and belong to the nutella framework itself. They are started only once and shared by all projects and runs. Project bots are started only once per project and are shared by all the runs of a particular project. Finally, plain bots are started once per run and they belong exclusively to that run. Most of the times you'll have to deal only with plain bots. You might occasionally have to start a project bot, shared among a bunch of runs (e.g. a simulation shared among classrooms) but, unless you are actively contributing to the development of nutella, you shouldn't have to deal with framework bots.

### Component
In nutella, a component can be either a bot or an interface. Component are the basic building block of a nutella applicaiton and encapsulate a piece of functionality of the application we are trying to develop. Components are self contained and interact with other componets _exclusively_ through the publish/subscibe mechanisms provided by nutell. A good example of a component could be a bot that implements the logic for a simulation which needs to be studied by a group of kids. Another example could be a bot that collects and stores all the notes kids take using a note-taking app on their ipads (a good example of an interface) while studing such simulation.

### Interface
An interface (or user interface) is a componet that interacts with other components _and_ users. This means that interfaces receive inputs from either bots, interfaces or users and provide outputs to the same three.

### Project
A nutella project is simply a collection of components stitched together. 

### Run
A run is an instance of a particular nutella project. Runs are useful whenever we want to run the same project (or )

### Template
A template is simply a collection of boilerplate code that simplifies a certain task. If you want to know more about templates, read the [templates wiki page](https://github.com/nutella-framework/nutella_framework/wiki/Templates).
