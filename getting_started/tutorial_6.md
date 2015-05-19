# Create bot (back-end)
<img src="images/dev_process_4.png" width="200" align="right">

Now that we have successfully created our first interface it is time to work on our first bot. As we said before, bots are used for back end components and, in our Hunger Games macroworld, we will create a bot to assemble and store the total amount of calories collected by each kid while foraging. These statics will then be used by the kids to reflect on their foraging behavior and hopefully help them better understand the rules of the game.

Similarly to what we did for our interface we are going to use JavaScript and RoomComponents. In particular, for out `stats-bot` we are going to use `basic-node-bot`. Similarly to what we did for out `patch-interface` we can install the RoomComponent into our application by doing
```
$ nutella install basic-node-bot stats-bot
Installed template: basic-node-bot as stats-bot
```

Since we are installing a bot component and not an interface component the files for our `stats-bot` will be created in `bots/stats-bot`. 

## Installing dependencies for your bot
Since this bot has been written in node.js it uses `npm` to manage dependencies. In order to intall the dependencies for this all all the other bots you can do

```
$ nutella dependencies
```
and this will exectute the `dependencies` script inside each component in your application. In the case of `stats bot` this script will simply execute `npm install`, which is the standard npm command to install dependencies.


## Starting and monitoring your bot

The one difference with interfaces is that, in order to start a newly created bot, you need to restart nutella. Therefore just do
```
$ nutella stop
Application hunger-games stopped!
$ nutella start
Application hunger-games started!
Application is running on broker: 127.0.0.1
Do `tmux attach-session -t hunger-games/default` to monitor your bots.
Go to http://localhost:57880/hunger-games/default to access your interfaces
```

If you compare the `nutella start` command output with the output of the same command from earlier you'll notice an extra line appeared.
```
Do `tmux attach-session -t hunger-games/default` to monitor your bots.
```

When you type `nutella start`, the framework automatically starts all your bots and creates a [tmux](http://tmux.sourceforge.net/) session for you to monitor them. tmux is simply a terminal multiplexer similar to [screen](http://www.gnu.org/software/screen/). Each bot in your macroworld application is executed inside its own tmux window. You can swap between windows by pressing `control-c` and then the number of the window you want to go to. To get out of this "virtual shell" without killing it simply type `ctrl-c` and then `d`.

**Pro tip: you'll use tmux extensively so if you are not familiar with it we strongly suggest take a minute to check out [Daniel Miessler's tmux primer](https://danielmiessler.com/study/tmux/)**

Now that we learned how to monitor our bot let's start to tinker with it!

## Implementing `stats-bot`
In order to collect the total amount of calories accumulated by each student we will need to set the beginning and ending of the collection time (using a message sent to the bot) and then use the arrival/departure events to calculate the amount of time spent at each patch. Let's see how to do that. 

The first bit is really the usual nutella initialization stuff. Then we declare some variables. One of them is special: `scores`. This object is actually linked to a JSON file. The object has two methods `load()` and `save()` that load and persist the content of the object to the JSON file respectively. This is the way nutella simplifies storage operations.
```js
var NUTELLA = require('nutella_lib');

// Get configuration parameters and init nutella
var cliArgs = NUTELLA.parseArgs();
var componentId = NUTELLA.parseComponentId();
var nutella = NUTELLA.init(cliArgs.broker, cliArgs.app_id, cliArgs.run_id, componentId);

// State
var foraging = false;   // Boolean variables that indicates if we are foraging or not
var timers = {};        // Stores the timers used to calculate the score

// Persistence (to file)
// Stores the scores for all beacons in the format {"beacon10":8,"beacon5":23, ... ,"beacon1":22}
var scores = nutella.persist.getJsonObjectStore('scores');
```


Then it's time to subscribe to the events that are going to signal the bot that foraging has started and stopped. When the foraging bout stops we call `save()` and we store the scores of students. 
```javascript
// Handles the starting of the foraging bot
nutella.net.subscribe('start_foraging_bout', function(message, from) {
    foraging = true;
});

// Handles the end of the foraging bot
nutella.net.subscribe('stop_foraging_bout', function(message, from) {
    foraging = false;
    scores.save();
});
```

After star/stop bout we need to subscribe to arrival and departures. Similarly to what we did for our `patch-interface` we simply set `notifyEnter` and `notifyExit` to `true` for all static resources. In order to do that we download the list of resources, filter the static ones (`filter`), extract their resource id (`map`) and then iterate the array of resource ids (`forEach`) and subscribe. 
```js
// Subscribes to all enter and exit events for all beacons and 
// register callbacks for these events
nutella.location.ready(function() {
    var patches = nutella.location.resources.filter(function(el){
        return el.type==='STATIC';
    }).map(function(el){
        return el.rid;
    }).forEach(function(rid) {
        nutella.location.resource[rid].notifyEnter = true;
        nutella.location.resource[rid].notifyExit = true;
    });
    nutella.location.resourceEntered(beaconEntered);
    nutella.location.resourceExited(beaconExited);
});
```

When subscribing to arrival/departures events we have to specify callbacks that are fired whenever the events occur. Here we simply register a timer that gives students a point for each second they sit in a patch. Not quite the sophisticated algorithm used in Hunger Games but for our purposes it will do! :smiley: Notice how events fired outside the foraging period are discarder.
```js
// Fired whenever a beacon enters a patch
function beaconEntered(beacon, patch) {
    if (foraging) {
        timers[beacon.rid] = setInterval(function() {
            if (scores[beacon.rid]===undefined)
                scores[beacon.rid] = 1;
            else
                scores[beacon.rid] = scores[beacon.rid] + 1;
        },1000);
    }
}

// Fired whenever a beacon exists a patch
function beaconExited(beacon, patch) {
    if (foraging) {
        clearInterval(timers[beacon.rid]);
    }
}
```

The last bit is simply providing a way for other components to access the scores calculated by out `stats-bot`. Here all we have to do is to use the `handle_request` function provided by `nutella.net` and, in the callback, simply return our scores object.
```js
//  Handles the requests for the stats of all students
nutella.net.handle_requests('complete_stats', function(message, from) {
    return scores;
});
```

Putting the whole thing together, this is what we get. 

```js
var NUTELLA = require('nutella_lib');

// Get configuration parameters and init nutella
var cliArgs = NUTELLA.parseArgs();
var componentId = NUTELLA.parseComponentId();
var nutella = NUTELLA.init(cliArgs.broker, cliArgs.app_id, cliArgs.run_id, componentId);

// State
var foraging = false;   // Boolean variables that indicates if we are foraging or not
var timers = {};        // Stores the timers used to calculate the score

// Persistence (to file)
// Stores the scores for all beacons in the format {"beacon10":8,"beacon5":23, ... ,"beacon1":22}
var scores = nutella.persist.getJsonObjectStore('scores');

// Handles the starting of the foraging bot
nutella.net.subscribe('start_foraging_bout', function(message, from) {
    foraging = true;
});

// Handles the end of the foraging bot
nutella.net.subscribe('stop_foraging_bout', function(message, from) {
    foraging = false;
    scores.save();
    // Clearing intervals to stop counting up
    Object.keys(timers).forEach(function(t) {
        clearInterval(timers[t]);
    });
});

// Subscribes to all enter and exit events for all beacons and 
// register callbacks for these events
nutella.location.ready(function() {
    var patches = nutella.location.resources.filter(function(el){
        return el.type==='STATIC';
    }).map(function(el){
        return el.rid;
    }).forEach(function(rid) {
        nutella.location.resource[rid].notifyEnter = true;
        nutella.location.resource[rid].notifyExit = true;
    });
    nutella.location.resourceEntered(beaconEntered);
    nutella.location.resourceExited(beaconExited);
});

// Fired whenever a beacon enters a patch
function beaconEntered(beacon, patch) {
    if (foraging) {
        timers[beacon.rid] = setInterval(function() {
            if (scores[beacon.rid]===undefined)
                scores[beacon.rid] = 1;
            else
                scores[beacon.rid] = scores[beacon.rid] + 1;
        },1000);
    }
}

// Fired whenever a beacon exists a patch
function beaconExited(beacon, patch) {
    if (foraging) {
        clearInterval(timers[beacon.rid]);
    }
}

//  Handles the requests for the stats of all students
nutella.net.handle_requests('complete_stats', function(message, from) {
    return scores;
});
```

## Next
That wasn't that bad, was it? Now let's finish our application and let's see how we can get an interface to display the stats calculated by this bot.


[:arrow_backward: PREV](tutorial_5.md) | [NEXT :arrow_forward:](tutorial_7.md)
