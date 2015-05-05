# Creating your first app (instant gratification)

To create a nutella app, open your terminal and type:
```
nutella new my_macroworld_app
```
This will create a new folder called `my_macroworld_app` with all of the files and folders that a nutella app needs:

```
bots          # folder for all the bots (a.k.a. the backend)
interfaces    # folder for all the interfaces (a.k.a. the frontend)
nutella.json  # nutella project file containing your project name, version and description
```
To run the newly created app:

```
cd my_macroworld_app
nutella start
```
You'll get a message like this
```
Application my_macroworld_app started!
Application is running on broker: localhost
Go to http://localhost:57880/my_macroworld_app/default to access your interfaces
```

Open your web browser and go to `http://localhost:57880/my_macroworld_app/default` to see the main interface for the app. Your should see something like this.

<img src="images/main_interface.png">
So many buttons! :) Each one of them is a GUI that will help you during the development of your macroworld application. Curious about what each of those buttons does? Keep reading! Wanna tinker for a bit first? Go ahead!

If you are wondering: "How do I stop this application thing now?" the answer is as easy as 
```
nutella stop
``` 

Congratulations! You just created and started your first nutella application. 


[:arrow_backward: PREV](index.md) | [NEXT :arrow_forward:](tutorial_2.md)
