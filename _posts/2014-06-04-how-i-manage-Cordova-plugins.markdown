---
layout: post
title:  "How I manage Cordova/Phonegap plugin dependencies"
---
Since Cordova doesn't recommend a way to keep track of your installed plugins,
nor does it have a default `.gitignore` file, I thought I'd share how I do it
my company's production apps.

I actually have a file named `Pluginfile` that lists all my installed plugins
and their versions.  Here's one for example:

    https://github.com/phonegap-build/PushPlugin.git
    de.appplant.cordova.plugin.local-notification@0.7.4
    org.apache.cordova.vibration@0.3.8
    org.apache.cordova.network-information@0.2.8
    org.apache.cordova.device@0.2.9
    org.apache.cordova.dialogs@0.2.7
    org.apache.cordova.console@0.2.8
    org.apache.cordova.inappbrowser@0.3.0

As you can see, I'm listing all my project's Cordova plugins in here, along
with their version number. This allows the project manager and QA to manage the
dependencies in a sane way.

Then, in the project's `Makefile`, I have a target like this:

    plugins: Pluginfile node_modules 
            mkdir -p platforms
            @cat $< | xargs -L 1 $(CORDOVA) plugin add
    
The 3rd line is key here. It goes over each line in our `Pluginfile` and performs `cordova plugin add` for the plugin listed.

Using this method, **there is absolutely no reason to keep your plugins directory in git.**
Your project's future maintainer will thank you for doing it.

