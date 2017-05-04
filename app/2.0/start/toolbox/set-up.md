---
title: Step 1. Get set up
subtitle: "Build an app with App Toolbox"
---

<!-- toc -->

The [Polymer App Toolbox][toolbox] is a collection of components, tools and
templates for building Progressive Web Apps with Polymer.

Follow the instructions below to install, build, and deploy a project using an
App Toolbox template in less than 15 minutes.

## Install the Polymer CLI

Polymer CLI is an all-in-one command line tool for Polymer projects. In this tutorial you use Polymer CLI to initialize, serve, and build your project. You can also use it for linting and testing, but this tutorial won't cover those topics.

1.  Check that you have already installed all of Polymer CLI's dependencies
    by running the following commands.

    *   Git

            git --version

    *   Node.js (LTS version 6.x)

            node -v

    *   npm v3 or higher

            npm -v

    *   Bower

            bower -v

    You should see the typical output indicating what version you're running
    of each of these dependencies.
    If you're missing any of these dependencies, follow the instructions in
    the following section from the Polymer CLI guide to learn how to install
    each one:
    [Install section from Polymer CLI guide](/2.0/docs/tools/polymer-cli#install).

1.  Install the Polymer CLI.

        npm install -g polymer-cli

## Initialize your project from a template

1. Create a new project folder to start from.

        mkdir my-app
        cd my-app

1. Initialize your project with an app template.

        polymer init polymer-2-starter-kit

## Serve your project

The App Toolbox templates do not require any build steps to get started
developing.  You can serve the application using the Polymer CLI, and
file changes you make will be immediately visible by refreshing
your browser.

    polymer serve

In the output from the `polymer serve` command, you'll see the URL of your locally-hosted application:

![Output from the polymer serve command](/images/2.0/toolbox/polymer-serve-output.png)

Open this URL in your browser:

![App Toolbox: Starter Kit Template](/images/2.0/toolbox/starter-kit-template.png)

## Project structure

The diagram below is a brief summary of the files and directories within
the project.

```text
bower.json             # bower configuration
bower_components/      # app dependencies
images/
index.html             # main entry point into your app
manifest.json          # app manifest configuration
package.json           # npm metadata file
polymer.json           # Polymer CLI configuration
service-worker.js      # auto-generated service worker
src/                   # app-specific elements
  my-app.html            # top-level element
  my-icons.html          # app icons
  my-view1.html          # sample views or "pages"
  my-view2.hmtl
  my-view3.html
  my-view404.html        # sample 404 page
  shared-styles.html     # sample shared styles
sw-precache-config.js  # service worker pre-cache configuration
test/                  # unit tests
```

## Next steps

Your app is now up and running locally. Next, learn how to add
a page to your app.

<a class="blue-button"
    href="create-a-page">Next step: Create a page</a>

[toolbox]: /2.0/toolbox/
[md]: http://www.google.com/design/spec/material-design/introduction.html
