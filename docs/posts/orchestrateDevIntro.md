---
date:
  created: 2026-01-09
categories:
  - IBM Orchestrate
  - IBM Orchestrate Developer Edition
  - application development
  - AI
  - Agents
tags:
  - personal
authors:
  - brian
---

# IBM Orchestrate Developer Edition - Introduction 

The IBM Orchestrate Developer Edition (Orchestrate Dev) is a tool to help developers build and test applications for IBM Orchestrate.  You do need entitlement to use it, but it gives you a dedicated environment to work in on you local machine with AI inferencing using cloud hosted services.

## TL;DR

- IBM Orchestrate Developer Edition provides a local development environment for building and testing Orchestrate applications
- Requires entitlement through IBM Orchestrate or watsonx.ai to access
- Install via Python package ibm-watsonx-orchestrate from PyPI
- Uses Lima VM to host containers, requiring significant resources (100GB+ disk space, substantial CPU and memory)
- Set LIMA_HOME environment variable if using external storage for Lima data
- Server starts with `orchestrate server start` command, making API available on port 4321
- UI starts separately with `orchestrate chat start` command
- Authorization credentials for API access found in $HOME/.cache/orchestrate/credentials.yaml
- Consider local image registry for team environments or isolated networks
- Use YAML definitions for artifacts to enable automation and easy environment resets

<!-- more -->

This is the first blog exploring the capabilities of Orchestrate Dev and how it can be used to build applications, but also highlighting when it should not be used or differences with production systems.

## Installation

The [documentation](https://developer.watson-orchestrate.ibm.com/developer_edition/wxOde_setup) sets out how to install the Orchestrate Dev tooling on MacOS, Windows and Linux and how to get the required entitlement using either an instance of Orchestrate or watsonx.ai.  It is as simple of installing the python package [**ibm-watsonx-orchestrate**](https://pypi.org/project/ibm-watsonx-orchestrate/#history).  Be sure to check the latest release and stay up to date as new releases are made available on a frequent basis.

### Resources

Orchestrate dev takes a considerable amount of resources to run, both CPU, memory and disk space.  

Orchestrate dev uses [Lima](https://lima-vm.io) to host the container environment.  By default Lima will use directory *.lima* in your home directory to store data.  If you need to use an external drive to host the lima data, which can require over 100GB of space, then set the environment variable **LIMA_HOME** to point to the location you want lima to use, e.g. `export LIMA_HOME=/Volumes/SSD_4TB/lima`

Depending on the features you want to use you may need to increase the resources allocated to the lima VM.  You can do this using the `orchestrate server edit` command.  Like all orchestrate cli commands using the `--help` flag will show you the options available.

## Starting Orchestrate Dev

There are 2 parts to starting Orchestrate Dev.

### Starting the server

The first part is to start the server - this makes the API available but not the User Interface.  

You can start the server using the `orchestrate server start` command.  There are a number of options available to add additional functionality to the server.  You can use the `--help` flag or refer to the [documentation](https://developer.watson-orchestrate.ibm.com/developer_edition/manage_local_server#initializing-watsonx-orchestrate-developer-edition-server).  Be aware that adding additional features will require additional resources.  On the first run the Lima environment will be initialised and the container images will be pulled from the repositories, this can take a while to complete before the server starts.  The initial start can sometimes fail, but so long as you have sufficient resources available simply repeat the start command again with the same options and it will resume the startup process.

By default the API is available on port 4321 and the OpenAPI UI is available at endpoint http://localhost:4321/docs.  You will notice that most of the API endpoints are protected requiring authorisation to be completed.  Currently the documentation does not specify how to find the authorisation credentials needed to unlock the protected API endpoints.

The **orchestrate** cli tool can be used to manage both the dev edition and production instances of Orchestrate.  You can add configurations for remote instances of orchestrate and the details of all environments can be found in the `$HOME/.cache/orchestrate/credentials.yaml` file.  This file is where you find the authorisation token for the orchestrate dev API.  You should have an entry for **local** in the credentials file.  The value of the **wxo_mcsp_token** is the authorisation token for the orchestrate dev OpenAPI UI authorisation.

### Starting the UI

If you want to access the User Interface you need to start the UI server using command `orchestrate chat start`.  By default this will automatically open a browser window to the UI.  You can stop this using a command line flag.  Depending on the options chosen when starting the server the UI will show the available functionality.  However, there are some features that you will see in a production deployment that you will not see in the dev edition.

## Tips when starting to work with Orchestrate

- The downloads needed when initially starting Orchestrate Dev are large.  If you find yourself or your team repeatedly pulling the images, or need to work in an isolated network, then setting up a local image registry can be a good solution - this is covered in the [docs](https://developer.watson-orchestrate.ibm.com/developer_edition/custom_registry)
- if you want to keep up to date with the latest changes and often reset your local environment, then using yaml definitions of orchestrate artifacts rather than using the UI to create them allows you to automate the creation of agents, connections, tools, etc...