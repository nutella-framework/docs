nutella supports OSX and Linux.

# Installing
nutella is written in ruby but it uses a bunch of other technologies that need to be installed for nutella to work. You need:

1. _ruby_ (version >= 2.1.0). Do yourself a favor and use [RVM](https://rvm.io/rvm/install) to install Ruby.
1. _git_ (version >= 1.8.0). Do yourself a favor and use [Homebrew](http://brew.sh/) to install git, if you are on OSX.
1. _tmux_ (version >= 1.8.0). Do yourself a favor and use [Homebrew](http://brew.sh/) to install tmux, if you are on OSX.
1. _node.js_ (version >= 0.10.0). Yes, really, you need to install node becaue we use it to run the broker that handles all communications between all the pieces of the framework. Do yourself a favor and use [nvm](https://github.com/creationix/nvm).
1. _mongoDB_ (optional). You'll need mongoDB if you want to use it with `nutella.persist`.

Once you have all of these, to install nutella do:
```
gem install nutella_framework
```
Once the installation is complete you should be able to type `nutella` in your shell and get a welcome message. 

## nutella checkup
If you are reading this you probably already saw the warning: "Looks like this is a fresh installation of nutella. Please run 'nutella checkup' to check all dependencies are installed".

nutella is written in ruby but it's designed to run bots and interfaces written in virtually any programming language. All communications among these components are handled by an _MQTT broker_ which needs to be installed (together with its dependencies) before nutella can actually work correctly. Therefore **right after your install nutella** you should run:
```
nutella checkup
```
This will install the [Mosca](http://www.mosca.io/) MQTT broker and make sure all the dependencies required by nutella are installed as well. This could take a minute so please be patient.

Congratulations! nutella is ready to use!


# Where next?
Two options:

- If you **already have an application** you want to tinker with (like [RoomQuake](https://github.com/ltg-uic/roomquake)) simply clone the application to a local folder (if it's in a remote git repo), `cd /to/my/local/folder` in the terminal, fire up your favorite editor and and start tinkering away. Not sure how? Check out the [man page for the nutella command line tool](https://github.com/nutella-framework/nutella_framework/wiki).

- If you **want to create your own application from scratch**, hold on tight, a tutorial is coming soon.
