# Assignment name: Configuring Xdebug with VSCode and Lando for PHP Development

## Executive Summary 

We've been working on setting up Xdebug for PHP development using VSCode and Lando. The goal is to have Xdebug running in Lando all the time, with the ability to debug both CLI scripts and the website. We've made significant progress, but there are still some loose ends to tie up.

## Plan: 

1. **Research existing guides on setting up Xdebug with Lando and VSCode for PHP development**
   - Status: Completed
2. **Analyze the guides and select the most suitable one**
   - Status: Completed
3. **Formulate my own setup guide**
   - Status: Completed
3. **Follow the formulated guide to set up Xdebug**
   - Status: Comleted
4. **Test the setup by running a PHP script with Xdebug**
   - Status: Completed
4. **Test the CLI setup by running a PHP script with Xdebug**
   - Status: Completed
5. **Troubleshoot any issues encountered during testing**
   - Status: In Progress

## Actions Taken 

1. **Researched existing guides on setting up Xdebug with Lando and VSCode for PHP development**
   - Result: Found several guides and selected the most suitable one based on our requirements.
   - Feedback: The guide provided a good starting point, but we encountered some issues during implementation.
   - Next Step: Continue with the setup process and troubleshoot any issues.

2. **Followed the selected guide to set up Xdebug**
   - Result: Successfully configured Xdebug in Lando and VSCode.
   - Feedback: The guide was helpful, but we encountered an issue when starting the debugger in VSCode.
   - Next Step: Troubleshoot the issue and test the setup again.

3. **Tested the setup by running a PHP script with Xdebug**
   - Result: Encountered an error when starting the debugger in VSCode: "listen EADDRINUSE: address already in use :::9003".
   - Feedback: The error suggests that the port 9003 is already in use. We need to ensure that no other process is using this port.
   - Next Step: Troubleshoot the issue and test the setup again.

4. **Restarted VSCode**
   - Result: VSCode status bar went blue when I clicked "Listen for XDebug" indicated success.
   - Feedback: The blue status bar, which also now reads "Listen for XDebug" tells us VSCode is ready to connect to to our debug session.
   - Next Step: Troubleshoot the issue and test the setup again.

4. **Run PHP script on CLI**
   - Result: Script completed successfully, but VSCode did not catch any breakpoints.
   - Feedback: The blue status bar, which also now reads "Listen for XDebug" tells us VSCode is ready to connect to to our debug session.
   - Next Step: Troubleshoot the issue and test the setup again.

6. **Troubleshot the issue and tested the setup again**
   - Result: Successfully ran a PHP script with Xdebug after setting the `XDEBUG_SESSION` environment variable to 1 `lando ssh` `export XDEBUG_SESSION=1` 
   - Feedback: The setup seems to be working correctly now. We need to do further testing to confirm.
   - Next Step: Continue testing the setup and troubleshoot any issues.

## Pending actions  / Next steps.

- Update the setup guide based on our experience and findings.

---

# How to Configure Xdebug with Lando and VSCode for PHP Development

In this guide, I'll walk you through the steps I took to set up Xdebug for PHP development using VSCode and Lando. The goal was to have Xdebug running in Lando all the time, with the ability to debug both CLI scripts and the website.

## Prerequisites

- Lando
- VSCode
- Xdebug Helper extension for Chrome

## Steps

1. **Configure Lando to use Xdebug**

   I added the following configuration to my `.lando.yml` file to enable Xdebug:

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

   This tells Lando to enable Xdebug in the `appserver` service and sets the `XDEBUG_MODE` environment variable to "develop,debug".

2. **Configure VSCode to listen for Xdebug connections**

   I added the following configuration to my `.vscode/launch.json` file:

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

   This tells VSCode to listen for incoming Xdebug connections on port 9003 and to stop at the first line of the script.

3. **Check Xdebug settings**

   I ran `php -i` in my Lando environment and checked the following Xdebug settings:

   ```bash
   php -i | grep -E 'xdebug.mode|xdebug.client_host|xdebug.client_port|xdebug.start_with_request'
   ```

   This gave me the following output:

   ```
   xdebug.mode => develop,debug => develop,debug
   xdebug.client_host => 192.168.0.237 => localhost
   xdebug.client_port => 9003 => 9003
   xdebug.start_with_request => trigger => trigger
   ```

   I noticed that `xdebug.start_with_request` was set to "trigger", which means Xdebug will only start debugging when a specific trigger is present. In my case, this trigger is the `XDEBUG_SESSION` environment variable.
