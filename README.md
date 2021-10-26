# Dynamic Config Example

In this example, we will create a basic dynamic configuration in CircleCI that merges 3 different config files into one config file. 

## Prereqs

- Some basic knowledge of YAML and CircleCI configuration
- Some basic knowledge of git and an existing GitHub.com account

## What is Dynamic Config? 

The ability to generate configurations dynamically depending on specific pipeline parameters. Dynamic config allows you to:

- Execute conditional workflows/commands
- Pass pipeline parameter values and/or generate additional configuration
- Trigger separate config.yml configurations which exist outside the default parent .circleci/ directory

To use our dynamic configuration feature, you can add the key `setup` with a value of `true` to the top-level of your parent configuration file (in the .circleci/ directory). This will designate that config.yaml as a setup workflow configuration, enabling you and your team to get up and running with dynamic configuration.

### Why Dynamic Config?

Users may find that nstead of manually creating each and every individual CircleCI configuration per project, they would prefer to generate these configurations dynamically, depending on specific pipeline parameters or file-paths.

This becomes particularly useful in cases where your team is using a monorepo, or a single repository, as opposed to using multiple repositories to store your code. In the case of using a monorepo, it is of course optimal to only trigger specific builds in specific areas of your project. Otherwise, all of your microservices/sub-projects will go through the entirety of your build, test, and deployment processes when any single update is introduced.

## Getting Started with Dynamic Config

A few setup steps are required to begin using dynamic config. The first is enableing dynamic config on your desired project in CircleCI. 

### Enable Dynamic Config

- In CircleCI, open the project settings that you would like to enable dynamic config on. 

<img src="images/projectsettings.png">

- On the left handed panel, select **Advanced**. 
- Scroll to the bottom of the page and select **Enable dynamic config using setup workflows**.

<img src="images/enabledynamic.png">

### Add setup Key

Once the project has dynamic config enabled, your static config.yml will continue to work as normal. This feature will not be used until you add the key `setup` with a value of `true`. We will work on creating our own dynamic config soon, but for reference below is an example of what this looks like in a `config.yml` file. 

```yml
version: 2.1
setup: true
```
### The Continuation Orb

When using dynamic configuration, at the end of the setup workflow, a continue job from the `continuation orb` must be called (NOTE: this does not apply if you desire to conditionally execute workflows or steps based on updates to specified files, as described in the [Configuration Cookbook](https://circleci.com/docs/2.0/configuration-cookbook/?section=examples-and-guides#execute-specific-workflows-or-steps-based-on-which-files-are-modified) example).

The `continuation` orb assists CircleCI users in managing the pipeline continuation process easily. The continuation orb wraps an API call to `continuePipeline` in an easy-to-use fashion. See the [continuation](https://circleci.com/developer/orbs/orb/circleci/continuation) orb documentation for more information.

```yml
version: 2.1
setup: true

orbs:
  continuation: circleci/continuation@0.1.2
```
## Create a Dynamic Config

In this example, we will create a dynamic config file that will generate a pipeline.yml file that is a combination of 3 configuration files that exist in our repository. This is a basic example of dynamic config that showcases how to generate and then use a dynamically generated config file. 

### Create a New Repository

- Navigate to your account on GitHub.com
  - Go to the Repositories tab and then select New

<img src="images/newrepo.png">

- Add a repository name and initialize with a README
- Click **Create Repository**

<img src="images/repodesc.png">

### Add a .yml file

CircleCI uses a YAML file to identify how you want your testing environment setup and what tests you want to run. On CircleCI 2.0, this file must be called `config.yml` and must be in a hidden folder called .circleci (on Mac, Linux, and Windows systems, files and folders whose names start with a period are treated as system files that are hidden from users by default).

- In your new repository, click the "Create new file" button and type `.circleci/config.yml`.
- You should now have in front of you a blank `config.yml` file in a `.circleci` folder. 

To begin, we will create a simple config file. Copy the code below and paste it into your config file in your repo. 

```yml
version: 2.1
jobs:
  build:
    docker: 
      - image: cimg/go:1.17.2
    steps:
      - checkout
      - run: echo "A static config file!"
```
This is a static config file. Will add to this file as we go along to make it dynamic. For now, commit the changes. 

### Follow and Enable Dynamic Config

Now that we have created a repo with a config file, we can follow it on CircleCI and enable dynamic config. 

- Navigate to the CircleCI application.
- Click follow next to your new repo under "Projects"
