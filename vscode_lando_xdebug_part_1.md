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

# How to Set Up Xdebug with Lando and VSCode

## Initial Configuration

### .lando.yml

Here's my initial `.lando.yml` configuration:

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

This file configures my Lando environment. I've set the PHP version to 8.1, specified the webroot and database, and enabled Xdebug in the `appserver` service. I've also set the `XDEBUG_MODE` environment variable to "develop,debug", which enables both development aids and step debugging in Xdebug.

### .vscode/launch.json

Here's my initial `.vscode/launch.json` configuration:

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

This file configures VSCode to listen for Xdebug connections on port 9003 and pause execution at the first line of every PHP script. The `pathMappings` setting maps the path in my Docker container (`/app/`) to the path on my local machine (`${workspaceFolder}/`).

## Steps Taken

1. **Researched existing guides and documentation**: I conducted a web search for existing guides on setting up Xdebug with Lando and VSCode. I analyzed the search results and recommended a guide to follow.

2. **Formulated a setup guide**: Based on the research, I formulated a setup guide that includes steps for setting up Xdebug to auto-start, having the IDE listen for incoming debug connections, and setting the necessary environment variables in Lando.

3. **Checked PHP Xdebug settings**: I ran `php -i | grep 'xdebug\.'` and checked the following settings:

   - `xdebug.mode`: Found it to be "develop,debug", which matches the recommendation. This enables both development aids and step debugging.
   - `xdebug.client_host`: Found it to be "192.168.0.237", while the guide recommends "$lando_host_ip". This might need to be adjusted depending on my Lando setup. Usually, it should be "localhost" or "127.0.0.1".
   - `xdebug.client_port`: Found it to be "9003", which matches the recommendation. This is the port Xdebug will connect to.
   - `xdebug.start_with_request`: Found it to be "trigger", while the guide recommends "yes". This means Xdebug will only start if a trigger (like a GET/POST variable or a cookie) is present. If I want Xdebug to always start, I should change this to "yes".

   Based on these findings, I decided to keep `xdebug.start_with_request` set to "trigger" and manually set the `XDEBUG_SESSION` environment variable when needed.

4. **Encountered an error when starting the debugger in VSCode**: When I attempted to start the debugger in VSCode, I encountered an error: "listen EADDRINUSE: address already in use :::9003". This suggests that the port 9003 was already being used by another process. I restarted VSCode and my Lando project to resolve this issue.

5. **Located and shut down a separate Lando instance**: I realized that I had another Lando instance running, which could have been causing the "address already in use" error. I navigated to the directory of the other Lando project and ran `lando stop` to shut it down.

6. **Set `XDEBUG_SESSION` in the wrong environment and session**: In the process of troubleshooting, I set the `XDEBUG_SESSION` environment variable in the wrong Lando session. This is a crucial step because the `XDEBUG_SESSION` environment variable needs to be set in the same environment where I'm running my script. If it's not set, Xdebug won't start, even if `xdebug.start_with_request` is set to "trigger". I realized this mistake, set the `XDEBUG_SESSION` environment variable in the correct Lando session, and ran my script again.

7. **Ran a PHP script with Xdebug trigger**: After setting the `XDEBUG_SESSION` environment variable to 1 with the command `export XDEBUG_SESSION=1`, I ran a PHP script with the command `php create_application_jwt_token.php`. This time, the breakpoint set in VSCode was hit, indicating that Xdebug was working correctly.

Remember, it's crucial to ensure that the `XDEBUG_SESSION` environment variable is set in my environment when using this workflow. If it's not set, Xdebug won't start, even if `xdebug.start_with_request` is set to "trigger". I always double-check my environment variables before running my scripts.

## Current Status

I've successfully set up Xdebug with Lando and VSCode. I'm now able to set breakpoints in VSCode and hit them when running PHP scripts.

## Next Steps

1. **Test the setup with different scripts and scenarios**: Now that the basic setup is working, I can test it with different scripts and scenarios to make sure it works as expected.

2. **Adjust the setup as needed**: If I encounter any issues or if there are specific features I want to use, I might need to adjust the setup. For example, I might want to change the `xdebug.start_with_request` setting or add additional configuration options to my `.lando.yml` or `php.ini` files.

I'll continue with these next steps in the next session. The goal is to have a working Xdebug setup that allows me to debug both website and CLI scripts in my Lando + VSCode environment.
