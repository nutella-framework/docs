# Creating your first app

To create a nutella app, open your terminal and type:
```
nutella new crepe
```
This will create a new folder called `crepe` with all of the files and folders that a nutella app needs:

```
bots          # folder for all the bots (a.k.a. the backend)
interfaces    # folder for all the interfaces (a.k.a. the frontend)
nutella.json  # nutella project file containing your project name, version and description
.meteor               
```
To run the newly created app:

```
cd crepe
nutella start
```
You'll get a message like this
```
Application crepe started!
Application is running on broker: localhost
Do `tmux attach-session -t crepe/default` to monitor your bots.
Go to http://localhost:57880/crepe/default to access your interfaces
```

Open your web browser and go to `http://localhost:57880/crepe/default` to see the main interface for the app


[NEXT :arrow_forward:](tutorial_2.md)
