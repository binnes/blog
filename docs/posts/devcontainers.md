---
date:
  created: 2025-10-28
categories:
  - devcontainers
  - application development
  - containers
tags:
  - personal
authors:
  - brian
---

# Introduction to Devcontainers

!!!Todo
    what is Devcontainers?
    why should it be used?

## How to use Devcontainers?

Devcontainers is a technology that allows a custom environment to be created based on container technology.  

<!-- more -->

The diagram below shows the options for creating the required environment for a development project:

```mermaid
---
title: Devcontainers
config:
  layout: elk
---
flowchart TD
    %% ---------- Sources ----------
    subgraph Sources
        img["Docker Image"] -->|pull| createDevcontainer[Create Devcontainer]
        df["Dockerfile"]   -->|docker build| createDevcontainer
        dc["docker‑compose.yml"] -->|docker compose up| createDevcontainer
    end

    %% ---------- Creation ----------
    createDevcontainer --> decideFeatures{Add Features?}  
    
    %% ---------- Feature selection ----------
    decideFeatures -- Yes --> feat1["Feature 1 (e.g., Node, Python)"]
    decideFeatures -- Yes --> feat2["Feature 2 (VS Code extension)"]
    decideFeatures -- Yes --> featN["Feature N …"]
    decideFeatures -- No --> decideScripts{Add Commands / Scripts?}

    %% Aggregate feature install scripts
    subgraph Feature_Stage
        direction TB
        feat1 --> installScripts[Run Feature Install Scripts]
        feat2 --> installScripts
        featN --> installScripts
    end
    
    %% ---------- Commands / Scripts ----------
    installScripts --> decideScripts{"Add Commands / Scripts / Customisations?"}

    decideScripts -- Yes --> preCreate["Pre‑create script (init.sh)"]
    decideScripts -- Yes --> postCreate["Post‑create script (setup.sh)"]
    decideScripts -- Yes --> onStart["On‑start command (e.g., npm start)"]
    decideScripts -- Yes --> customizations["Customizations to vscode"]
    decideScripts -- No --> ready[Container Ready]
    
    %% ---------- Final container ----------
    subgraph Final_Devcontainer
        direction TB
        preCreate --> ready
        postCreate --> ready
        onStart   --> ready
        customizations --> ready
    end
```

### Source

!!!Todo
    What the source of the Devcontainer is - 3 options
    Docker-compose only 1 containers is mounted in the editor, others provide services

### Features
!!!Todo
    what are features
    Issues with different OS distributions

### Scripts and customisations

!!!Todo
    How can I customise a devcontainer before it is used?
    Scripts run at different stages, explain
    How to add plugins into the VS Code environment

## Using Devcontainers

!!!Todo
    How to use a devcontainer in a project

## Speeding up containers

!!!Todo
    The devcontainers CLI
    Speed up by creating a custom container image and save to repo

