+++
title = "UI Development in Docker"
date = "2020-04-08"
author = "Tamás Rusz"
cover = ""
tags = ["ui","docker","dx"]
keywords = ["ui", "front end", "docker", "testing","software","development"]
description = "A quick and simple-to-use setup for web front end development"
showFullContent = false
+++

Back in the day I have always struggled with setting up development environments for projects.  
It usually took hours and in some cases days to get everything right.
The documentation was always outdated and missing key information.
If you had to work on multiple projects that used
different versions of SDKs and runtimes than the experience was even worse.

Than came the light-weight containerization popularized by [Docker](https://www.docker.com/).
The main usage of containers is to run the software in the same environment on any hardware.
It runs the same way on my machine as in the cloud.

Using this feature of the containers I setup my projects almost the same way.

## Basic

The basic setup does not require much:

* a [docker-compose](https://docs.docker.com/compose/) file
* a [Node.js](https://hub.docker.com/_/node) container
* a mapped port
* the project folder mounted into the container

This approach ensures that everybody runs the same version of Node.js.
Furthermore, to run it one only need docker.

The docker-compose file usually looks something like this:

```yaml
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
we get colors, animated progressbars and we can interact with clis thanks to the
`stdin_open: true` and `tty: true`.

Mounting the folder enables file watcher for live refresh and hot reloading.

_An overview  the basic setup_

```txt
                ┌──────────────┐
                │ ◎ ○ ○ ░░░░░░░│
                ├──────────────┤      O
                │ Any browser  │     /|\  
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

## Testing in CI/CD Pipeline

With [Chrome Puppeteer](https://github.com/puppeteer/puppeteer) we have an easy time with testing
as chromium is also available in the container: [Docker Hub](https://hub.docker.com/search?q=puppeteer&type=image)

```yaml
version: "3.6"
services:
  ui-test:
    build:
      context: .
      dockerfile: "test.Dockerfile"
    depends_on:
      - ui-lib
    stdin_open: true
    tty: true
    working_dir: "/app"
    volumes:
        - ./:/app
    ports:
        - 9922:9922
    command: ["sh", "-c", "yarn install && yarn test"]
```

_An overview of the "Testing in CI/CD Pipeline" setup_

```txt
              ┌───────────────────────────────────────────────────────┐
              │                   ┌──────────┐                        │
              │                   │ Headless │                        │
              │         ●───────┬─│ Chromium │◀────┐                  │
              │         ◡       │ │          │     │                  │
              │         │       │ └──────────┘     │                  │
              │  localhost:3000 │                  │                  │
              │         │       │                  │                  │
              │   ┌───────────┐ │               ┌─────┐               │
              │   │Development│ └───────────────│Tests│               │
              │   │  Server   │                 └─────┘               │
              │   └───────────┘                    │                  │
              │         │                          │                  │
              │         ▼                          │                  │
┌────────┐    │     ┌ ─ ─ ─                        │                  │
│Project │  ┌─┴─┐     /app │                       │                  │
│ Folder │──│   │───│       ◀──────────────────────┘                  │
└────────┘  └─┬─┘    ─ ─ ─ ┘                                          │
              │                                                       │
              │Container: UI                                          │
              └───────────────────────────────────────────────────────┘
```

## Testing Locally and Visually

_An overview of the "Testing Locally and Visually" setup_

```txt
                               ┌──────────────┐
                               │ ◎ ○ ○ ░░░░░░░│
                               ├──────────────┤
                               │   Chromium   │
                        ┌──────│              │
                        │      │              │
                        │      │              │
                        ●      └──────────────┘
                        ◡              │
                        │        localhost:9922
                 localhost:3000        │
                        │              ◠
                       ┌─┐             ●
              ┌────────┤ ├─────────────┼─────────────┐
              │        └─┘             │             │
              │         │  host.docker.internal:9922 │
              │         ●──────────────┤             │
              │         ◡              │             │
              │         │              │             │
              │  localhost:3000        │             │
              │         │              │             │
              │   ┌───────────┐     ┌─────┐          │
              │   │Development│     │Tests│          │
              │   │  Server   │     └─────┘          │
              │   └───────────┘        │             │
              │         │              │             │
              │         ▼              │             │
┌────────┐    │     ┌ ─ ─ ─            │             │
│Project │  ┌─┴─┐     /app │           │             │
│ Folder │──│   │───│       ◀──────────┘             │
└────────┘  └─┬─┘    ─ ─ ─ ┘                         │
              │                                      │
              │Container: UI                         │
              └──────────────────────────────────────┘
```
