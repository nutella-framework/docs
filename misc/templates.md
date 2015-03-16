## What is a template
A template is simply a folder containing a `nutella.json` file which contains all the information necessary to use the template. This information includes:
- template name (mandatory, unique if in nutella repo)
- template version (mandatory)
- template type (mandatory): `bot` or `interface` for now
- template git repo (optional)
- template description (optional)
- requirements. This is the most interesting bit. Since templates can be written in any language each one of them requires specific tools to be run, download dependencies and what not. For this reason templates need to specify the required the tools and the version required. 

### Bot templates
In addition to this a bot template is characterized by three shell scripts that are used by nutella download dependencies, compile and run the template:
- _dependencies_: this script is optional and takes care of downloading the dependencies of the project (i.e. `gem install`, `mvn install`, `npm install`)
- _compile_: this script is optional and compiles the bots that require this step (i.e. `mvn compile/package`)
- _run_: this script is mandatory and stars the bot. See #6 for more details on how to write startup scripts.
Nutella will only run the scripts that it finds.

### Interface templates
Interface bots are more delicate and require some more thinking

## Installation
The standard way to install a templates is `nutella install <templateId>` from within the directory of the project. The command looks into the repository of nutella templates. When you do so, nutella will search the **templates database** (see below) and will simply do a `git clone` of the repo into the current project. As easy as that... 

You can always install a template by simply specifying the file path of the template like so `nutella install /my/absolute/path/to/file` or by specifying the git repo where the template is.

## Templates Database
Templates repository is simply the `templates-database` branch of nutella repo containing a bunch of `.json` files. Each file is named after the template with the same name and contains:
- template name
- template version
- template type (`bot` or `interface`)
- template git repo
- template description

To submit a new template for inclusion in nutella you simply do a pull request to the `templates-database` branch and you are good to go. (And, of course, offer the template in a git repo)
