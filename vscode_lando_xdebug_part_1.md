# Assignment name: Setting up Xdebug with Lando and VSCode Part 1

## Executive Summary 

We're working on setting up Xdebug in a Lando + VSCode environment, ensuring it works for both website debugging and CLI scripts. We've researched existing guides, formulated a setup guide, and started working through the guide. We've encountered an issue with hitting breakpoints in VSCode, which we're currently troubleshooting.

## Plan: 

1. **Research existing guides and documentation**
   - Status: Completed
2. **Formulate a setup guide**
   - Status: Completed
3. **Work through the setup guide and keep a log**
   - Status: In progress
4. **Adjust the plan as new requirements or changes are discovered**
   - Status: Not started

## Actions Taken 

1. Researched existing guides and documentation on setting up Xdebug with Lando and VSCode.
2. Formulated a setup guide based on the research.
3. Started working through the setup guide.
4. Encountered an issue with hitting breakpoints in VSCode.

## Loose ends

- Resolve the issue with hitting breakpoints in VSCode.
- Determine if any additional configuration is needed for CLI debugging.
- Determine if any Chrome extensions are needed for website debugging.

## Pending actions  / Next steps.

- Continue troubleshooting the issue with hitting breakpoints in VSCode.
- Adjust the plan as new requirements or changes are discovered.

## Recommended Next Steps 

1. **Troubleshoot the issue with hitting breakpoints in VSCode**
   - Task: Check the path mappings in `launch.json`, the Xdebug configuration in `.lando.yml` and `php.ini`, and the network settings.
2. **Adjust the plan as new requirements or changes are discovered**
   - Task: If new requirements or changes are discovered during the setup process, adjust the plan and the guide accordingly.

## Log of Today's Session

1. Discussed the desired workflow for setting up Xdebug with Lando and VSCode.
2. Updated the plan to include a research phase and a more iterative approach.
3. Conducted a web search for existing guides on setting up Xdebug with Lando and VSCode.
4. Analyzed the search results and recommended a guide to follow.
5. Reviewed the existing `.lando.yml` and `.vscode/launch.json` files and the current PHP Xdebug settings.
6. Discussed potential issues with hitting breakpoints in VSCode and recommended troubleshooting steps.
7. Agreed to continue troubleshooting the issue with hitting breakpoints in the next session.

---

# How to Set Up Xdebug with Lando and VSCode Part 1

## Current Configuration

### .lando.yml

Here's your current `.lando.yml` configuration:

```yml
name: basiqvoyager
recipe: lamp
config:
  php: '8.1'
  webroot: web 
  database: mariadb
services:
  appserver:
    xdebug: true
    overrides:
      environment:
        XDEBUG_MODE: "develop,debug"
```

This file configures your Lando environment. You've set the PHP version to 8.1, specified the webroot and database, and enabled Xdebug in the `appserver` service. You've also set the `XDEBUG_MODE` environment variable to "develop,debug", which enables both development aids and step debugging in Xdebug.

### .vscode/launch.json

Here's your current `.vscode/launch.json` configuration:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "stopOnEntry": true,
            "pathMappings": {
                "/app/": "${workspaceFolder}/",
            }
        }
    ]
}
```

This file configures VSCode to listen for Xdebug connections on port 9003 and pause execution at the first line of every PHP script. The `pathMappings` setting maps the path in your Docker container (`/app/`) to the path on your local machine (`${workspaceFolder}/`).

## Steps Taken

1. **Researched existing guides and documentation**: We conducted a web search for existing guides on setting up Xdebug with Lando and VSCode. We analyzed the search results and recommended a guide to follow.

2. **Formulated a setup guide**: Based on the research, we formulated a setup guide that includes steps for setting up Xdebug to auto-start, having the IDE listen for incoming debug connections, and setting the necessary environment variables in Lando.

3. **Started working through the setup guide**: We reviewed your existing `.lando.yml` and `.vscode/launch.json` files and the current PHP Xdebug settings. We discussed potential issues with hitting breakpoints in VSCode and recommended troubleshooting steps.

## Current Status

We've encountered an issue with hitting breakpoints in VSCode. When you set a breakpoint and run a PHP script, the breakpoint is not hit. We're currently troubleshooting this issue.

## Next Steps

1. **Check the path mappings in `launch.json`**: Make sure the path mappings in your `launch.json` match the paths in your Docker container.

2. **Check the Xdebug configuration in `.lando.yml` and `php.ini`**: Make sure you've followed all the steps in the guide to set up Xdebug in your `.lando.yml` and `php.ini` files. Also, make sure your `XDEBUG_SESSION` environment variable is set when you're running your script.

3. **Check the network settings**: If you're running a firewall or have certain network settings, they might be preventing VSCode from connecting to Xdebug. You might need to adjust these settings to allow the connection.

We'll continue troubleshooting the issue with hitting breakpoints in the next session. The goal is to have a working Xdebug setup that allows you to debug both website and CLI scripts in your Lando + VSCode environment.
