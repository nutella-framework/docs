# Command line utility reference
The nutella command line interface is used to create, run and interact with nutella projects. The general syntax to do this is: `nutella <command> <parameters>`. In this page we are going to provide a detailed description of all available commands available in nutella.

## Commands
- [new](#new)
- [install](#install)
- [dependencies](#dependencies)
- [compile](#compile)
- [start](#start)
- [runs](#runs)
- [stop](#stop)
- [help](#help)
- [checkup](#checkup)
- [broker](#broker)
- [template](#template)

***

#### new
This command is used to create new projects.
```
nutella new <project_name>
``` 
Creates a new folder `project_name` in the current directory and initializes it with all the files necessary for you to get started.


#### install
This command is used to install templates into the current project. 
[Templates](Templates) are boilerplate code which simplifies the creation of new bots and interfaces. (see the [nutella glossary page](Nutella-glossary) for more details) What this command really does is simply to "copy" the template into the proper folder, inside the `bots` or `interfaces` folders, in the current project.

```
nutella install <template_name> [<destination_folder>]
```
Installs a template from the [nutella templates database](Templates#templates-database). Simply specify the name of the template. If the destination folder is specified the template will be copied inside that folder. Examples: `nutella install basic-ruby-bot` or, with destination folder, `nutella install basic-ruby-bot my-awesome-bot`

```
nutella install <template_folder> [<destination_folder>]
``` 
Installs a template from a local file. Specify the absolute path of the template. If the destination folder is specified the template will be copied inside that folder. Example: `nutella install /Users/tebemis/code/basic-ruby-bot`
```
nutella install <template_git_repo_url> [<destination_folder>]
``` 
Installs a template from a remote git repository. Specify the URL of the git repo. If the destination folder is specified the template will be copied inside that folder. Example: `nutella install https://github.com/nutella-framework/basic-ruby-bot.git`.

Note: the `<destination_folder>` option is useful whenever you want to have multiple copies of a certain template in your project. This becomes handy if, for instance, we are trying to create two different bots from the same template. Example: `nutella install basic-ruby-bot my-awesome-bot` and `nutella install basic-ruby-bot my-awesome-bot-2`.

#### dependencies
This command is used to download and install all the dependencies for all the actors (bots and interfaces) in the project.
```
nutella dependencies
```
This command simply executes the `dependencies` scripts for all the actors (bots and interfaces) in the current project.

#### compile
This command is used to compile all the actors (bots and interfaces) in the project
```
nutella compile
```
This command simply executes the `compile` scripts for all the actors (bots and interfaces) in the current project.

#### start
This command is used to start an instance of the current project.
```
nutella start [<run_id>] [<options>]
```
Creates and starts an instance of the current project. If `run_id` is not specified it will be set to the project name. Once an instance of the current project has been created and started you can monitor it using [tmux](http://tmux.sourceforge.net/) simply doing `tmux attach -t <run_id>` (as indicated by the command output). This will launch the tmux session associated with this particular running instance of the project. You can also access all the interfaces by pointing your browsers to `http://localhost:57880/<run_id>` (as indicated by the command output).

##### Options: including and excluding bots
Sometimes you don't want to start all the components of a single application but only some. Vice versa, some other times you want to start all the components but exclude only a couple.
```
nutella start --with=botX,botY,botZ
nutella start -w=botX,botY,botZ
```
This option allows you to select the bots that you want to start. ONLY the bots in the option list will be started.
```
nutella start --without=botA,botB,botC
nutella start -wo=botA,botB,botC
```
This option allows you to select the bots that you do NOT want to start. nutella will start all the bots EXCEPT the ones specified in the option list.

Note that you can only use `--with / -w` and `--without / -wo` exclusively.

##### Options: sharing bots across runs
Sometimes you need to have a bot running across all runs of a particular project. These bots are called _project bots_. To start a bot as a project bot you need to add it to the `project_bots` array in the `nutella.json` file inside the project. Your `nutella.json` file should look something like this:
```json
{
  "name": "my_project",
  "version": "0.1.0",
  "nutella_version": "0.3.0",
  "type": "project",
  "description": "A quick description of your project",
  "app_bots": ["botA", "botB"]
}
```
This will tell nutella to start `botA` and `botB` only once for all instances (runs) of a certain project. You can monitor these special bots by doing `tmux attach-session -t <project_name>-project-bots` as indicated by the command output. Project bots are not affected by the `--with / -w` and `--without / -wo` options.

#### runs
This command lists all the running instances of a certain project.
```
nutella runs 
```
Shows the list of all the running instances (i.e. runs) for the current project.


```
nutella runs --all
nutella runs -a
```
Shows the list of all instances of _all projects_ associated with nutella.


#### stop
This command is used to stop an instance of the current project.
```
nutella stop [<run_id>]
```
Creates and starts an instance of the current project. If `run_id` is not specified it will be set to the project name. The command will terminate all the bots in the project, the internal broker (if not in use by any other project) and destroy the tmux session associated with this particular project instance.


## Additional commands
In addition to commands used to interact with projects, nutella provides a set of additional commands that allow to interact with nutella itself.

#### help
Displays a list of all commands and a brief description of their purpose.

#### checkup
Used to verify that all the dependencies required by nutella have been installed. 
- `nutella checkup` will show you all the missing/installed dependencies with their version. In the future this command will also take care of installing missing dependencies but for now this is it.

#### broker
This command allows to interact with the [MQTT broker](http://en.wikipedia.org/wiki/MQTT) that nutella uses for all its communications. 
- `nutella broker` will print out the name of the broker being used at the moment. `localhost` means that nutella is using the internal broker that ships with the framework. `[running]` means that the broker is (surprise!) running and `[stopped] means that the broker is (surprise again!) stopped.
- `nutella broker set <broker hostname>` Sets the hostname of the broker to be used by nutella. Setting the hostname to `localhost` will trigger the use of the internal broker shipping with nutella.

#### template
This command is used to generate and validate templates.
```
nutella template create <your_template_name>
```
Will prompt you with a series of choices that will guide you through the generation of a nutella template.
```
nutella template validate <your_template_dir>
```
It will validate a template contained in a certain folder.
