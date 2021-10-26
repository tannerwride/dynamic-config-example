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

## Getting Started with Dynamic Config

A few setup steps are required to begin using dynamic config. The first is enableing dynamic config on your desired project in CircleCI. 

### Enable Dynamic Config

- In CircleCI, open the project settings that you would like to enable dynamic config on. 

<img src="images/projectsettings.png">

- On the left handed panel, select **Advanced**. 
- Scroll to the bottom of the page and select **Enable dynamic config using setup workflows**.

<img src="images/enabledynamic.png">

### Add setup Key

Once the project has dynamic config enabled, your static config.yml will continue to work as normal. This feature will not be used until you add the key `setup` with a value of `true`.
