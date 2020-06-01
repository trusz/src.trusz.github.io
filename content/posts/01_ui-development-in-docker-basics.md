+++
title = "UI Development in Docker — Basics"
date = 2020-05-03
tags = ["docker", "ui", "dx"]
categories = ["UI Development"] 
imgs = []
cover = ""  # image show on top
readingTime = true  # show reading time after article date
toc = true
comments = false
justify = false  # text-align: justify;
single = false  # display as a single page, hide navigation on bottom, like as about page.
license = ""  # CC License
draft = false
+++

> This article is part of a series:
>
> 1. UI Development in Docker — Basics
> 2. [UI Development in Docker — Testing](/posts/02_ui-development-in-docker-testing)
> 3. [UI Development in Docker — CI/CD](/posts/03_ui-development-in-docker-cicd)

Back in the day I have always struggled with setting up development environments for projects.  
It usually took hours and in some cases even days to get everything right.
The documentation was always outdated and missing key information.
If you had to work on multiple projects that used
different versions of SDKs and runtimes than the experience was even worse.

Then came the light-weight containerization popularized by [Docker](https://www.docker.com/).
The main usage of containers is to run the software in the same environment on any hardware.
It runs the same way on my machine as in the cloud.

Using this feature of the containers I setup my projects almost the same way.

## The Basics

The basic setup requires only:

* a [docker-compose](https://docs.docker.com/compose/) file
* a [Node.js](https://hub.docker.com/_/node) container
* a mapped port
* the project folder mounted into the container

This approach ensures that everybody runs the same version of Node.js.
Furthermore, to run it one only needs Docker.

The docker-compose file usually looks something like this:

```yaml
# An extract of the `docker-compose.yaml`
version: "3.6"
services:
    ui:
        image: "node:13.5-alpine"
        ports:
            - "3000:3000"
        stdin_open: true
        tty: true
        working_dir: "/app"
        volumes:
            - .:/app
        command: ["sh", "-c", "yarn install && yarn dev"]
```

Alpine images keep the overhead low and the image small.

`yarn install && ...` makes sure that we alway have the correct packages.
This step takes only a few seconds as the `node_modules` are persisted outside of docker
and does not have to be installed again and again.

If we run start the container with `docker-compose run --service-ports`
we get colors, animated progressbars and we can interact with clis, thanks to the
`stdin_open: true` and `tty: true`.

Mounting the folder enables file watcher for live refresh and hot reloading.

```txt
An overview of the basic setup
                ╭──────────────╮
                │ ◎ ○ ○ ░░░░░░░│
                ├──────────────┤      O
                │ Any Browser  │     /|\  
                │              │◀──  / \  
                │              │
                │              │    USER  
                └──────────────┘
                        │
                        ●
                        ◡
                        │
                 localhost:3000
                        │
                       ┌─┐
              ┌────────┤ ├────────┐
              │        └─┘        │
              │         │         │
              │         ●         │
              │         ◡         │
              │         │         │
              │  localhost:3000   │
              │         │         │
              │   ┌───────────┐   │
              │   │Development│   │
              │   │  Server   │   │
              │   └───────────┘   │
              │         │         │
              │         ▼         │
┌────────┐    │     ┌ ─ ─ ─       │
│Project │  ┌─┴─┐     /app │      │
│ Folder │──│   │───│             │
└────────┘  └─┬─┘    ─ ─ ─ ┘      │
              │                   │
              │Container: UI      │
              └───────────────────┘
```

> **Need an example?**  
> Check out our starter kit for [↗ React + Storybook + TypeScript](https://github.com/sprinteins/starter-kits/tree/master/react-storybook-typescript)
