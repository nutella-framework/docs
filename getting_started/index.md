Welcome to nutella! This tutorial will teach you how to install nutella and how to create your first macroworld application. 

nutella supports OSX and Ubuntu (might work with other Linux distros but we didn't check yet).

# Pre-requisites
nutella is written in ruby but it uses a bunch of other technologies that need to be installed for nutella to work. You need:

1. _ruby_ (version >= 2.1.0). Do yourself a favor and use [RVM](https://rvm.io/rvm/install) to install Ruby both on OSX and Ubuntu.
1. _git_ (version >= 1.8.0). Do yourself a favor and use [Homebrew](http://brew.sh/) to install git, if you are on OSX. You can use apt-get on Ubuntu.
1. _tmux_ (version >= 1.8.0). Do yourself a favor and use [Homebrew](http://brew.sh/) to install tmux, if you are on OSX. You can use apt-get on Ubuntu.
1. _MongoDB server_ (version >= 2.6.9). You'll need MongoDB in order to power `nutella.persist` and store log data. If you are on OSX use [Homebrew](http://brew.sh/). On Ubuntu you might want to avoid `apt-get` and use [this method](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/) instead.
1. _node.js_ (version >= 0.10.10). Yes, really, you need to install node becaue we use it to run the broker that handles all communication between all the framework components. Do yourself a favor and use [Homebrew](http://brew.sh/) to install node, if you are on OSX and On Ubuntu you can [choose your favorite method](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-an-ubuntu-14-04-server). We like [nvm](https://github.com/creationix/nvm).


# Installing nutella
Once you have all the pre-requisites, to install nutella do:
```
gem install nutella_framework
```
Once the installation is complete you should be able to type `nutella` in your shell and get a welcome message. 

## nutella checkup
If you are reading this you probably already saw the warning: _Looks like this is a fresh installation of nutella. Please run 'nutella checkup' to check all dependencies are installed._ 

Please do `nutella checkup` in your shell **right after your install nutella**". Why do you have to do that? Because nutella is written in Ruby but it is designed to run bots and interfaces written in virtually any programming language. All communications among these components are handled by an _MQTT broker_ which needs to be installed (together with its dependencies) before nutella can work. Therefore **right after your install nutella** you should run `nutella checkup`. This will install the [Mosca](http://www.mosca.io/) MQTT broker and make sure all other dependencies required by nutella are installed as well. This could take a minute so please be patient.

Congratulations! nutella is ready to use!


# Where next?
Two options:

- If you **already have an application** you want to tinker with (like [RoomQuake](https://github.com/ltg-uic/roomquake)) simply clone the application to a local folder (if it's in a remote git repo), `cd /to/my/local/folder` in the terminal, fire up your favorite editor and and start tinkering away. Not sure how? Check out the [man page for the nutella command line tool](https://github.com/nutella-framework/docs/blob/master/cli/index.md).

- If you **want to create your own application from scratch**, [continue this tutorial](tutorial_1.md) to find out how!

[NEXT :arrow_forward:](tutorial_1.md)
