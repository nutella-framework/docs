This page contains a bunch of ideas about nutella and its evolution

- We should have a `nutella.store` module that provides all the facilities to store data in a backend to interface designers and developers. Something like [backbone.js](http://backbonejs.org/) or [PARSE](https://parse.com/). However not sure we need to re-invent the wheel...


## In a galaxy far far away...
The following stuff is quite down the line and addresses issues of scalability and distribution. 

* Languages to support in the future (in order): 
  1. Java (for bots, Processing and Android)
  1. C# (for Unity)
  1. Go (for bots)
  1. Scala (for bots)
  1. Python (for bots)

* Develop a search engine for templates. More broadly, we need to find a way of solving the problem of templates overload. How do we separate them, categorize them based on their level of completeness, language, etc etc

* People should be able to create projects using "recipes" or lists of templates they would like. Not sure this is really needed. Need to think more about it

* Provide support for scalability. Nutella is designed to launch a number of different _runs_ for the same project with `nutella start <runid>. This is all cool and great except when we serve the runs via a website we'll need to have high availability and we'll need multiple instances of nutella with a load balancer in front of them. In order to keep them synched we'll need to use a shared DB of _runs_.

* Provide support for distribution of bots. Right now we are assuming all the bots are running in a single machine. What happens to bots running on a different device? We need to be able to provide some management facilities for those (and we will) but we are talking more something along the lines of running nutella in each one of these devices and running the command on one device will sync to a centralized (or distributed) DB. This will introduce quite a bit of complexity. This is very low priority at the moment.

* We need to provide a way to have "remote bots" which you can control in a way similar to ssh. Provide a set of nutella commands to do so.


## Done 
* We should have a visualization that shows the "piping" between the bots. Who's listening and who's publishing and who's up.

* We should have a mailer/chat/notification plugin that sends us messages when stuff goes down.

* Find a way to have bots shared among runs

* Nutella libraries need to shelter bots programmers from connection and transport issues. The only exposed primitives available are`publish` and `subscribe`. Nutella clients automagically connect (and reconnect) as soon as the bot is started. (need to add support for this to the MQTT client libraries)

* Improve storage support: add support for binary files upload/download/sync

