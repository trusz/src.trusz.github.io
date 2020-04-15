+++
_title = "{{ replace .Name `-` ` ` | title }}"
title = "UI Development in Docker"
date = "{{ .Date }}"
lastmod = "{{ .Date }}"
tags = []
categories = []
imgs = []
cover = ""  # image show on top
readingTime = true  # show reading time after article date
toc = true
comments = false
justify = false  # text-align: justify;
single = false  # display as a single page, hide navigation on bottom, like as about page.
license = ""  # CC License
draft = true
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

## Testing Locally and Visually

For the best and safest results I use [Chromium](https://www.chromium.org/) that runs
in the local environment outside of the container.
This is necessary to see the UI itself when working on the styling.

Chromium has to be launched with `--enable-automation` so that the browser driver ([Chrome Puppeteer](https://github.com/puppeteer/puppeteer)) can control it through websocket.

I usually have a shell script to do it, similar to this:

```sh
chromium=/Applications/Chromium.app/Contents/MacOS/Chromium
$chromium --no-sandbox --enable-automation --remote-debugging-port=9922
```

Chromium has a limitation who can control it:

* in headless mode (no visible UI), connects are allowed from any source address as the websocket listens on the wildcard IP: `0.0.0.0:9922`
* in non-headless mode (visible UI), only the connections from localhost are allowed as the chromium
  switches back to `127.0.0.0:9922` (or `localhost:9922`)

Fortunately, the tests can connect from inside the container through `host.docker.internal:9922`
making it seem like that they also runs on `localhost`.

The container also watches for file changes to re-run the tests.

The container has to open its port for the browser.

```yaml
# An extract of the `docker-compose.yaml`
version: "3.6"
services:
  ui-dev:
      image: "node:13.10"
      stdin_open: true
      tty: true
      working_dir: "/app"
      volumes:
          - ./:/app
      ports:
          - 3000:3000
      command: ["sh", "-c", "yarn install && yarn tdd"]
```

```txt
An overview of the "Testing Locally and Visually" setup
                               ╭──────────────╮
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
              │         ●              │             │
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

## Testing in CI/CD Pipeline

The main difference between testing locally and in a pipeline is that we start
up the production build of the UI and we start a separate container with the tests and
headless chromium. This is quite easy as Chromium and [Chrome Puppeteer](https://github.com/puppeteer/puppeteer) are also available in a Docker container: [Docker Hub](https://hub.docker.com/search?q=puppeteer&type=image). The source files are still mounted in so we don't have to build
the `Test` container over and over again.

```yaml
# An extract of the `docker-compose.yaml`
version: "3.6"
services:
  ui:
   ...

  test:
    build:
      context: .
      dockerfile: "test.Dockerfile"
    depends_on:
      - ui
    stdin_open: true
    tty: true
    working_dir: "/app"
    volumes:
        - ./:/app
    command: ["sh", "-c", "yarn install && yarn test"]
```

The new `test` service

```txt
An overview of the "Testing in CI/CD Pipeline" setup
              ┌───────────────────┐
              │   ┌──────────┐    │
              │   │ Headless │    │
              │   │ Chromium │────┼───────────●
              │   │          │    │           ◡
              │   └──────────┘    │           │
              │         ▲         │    localhost:3000
              │         │         │           │
              │         │         │          ┌─┐
              │         │         │  ┌───────┤ ├────────┐
              │      ┌─────┐      │  │       └─┘        │
              │      │Tests│      │  │        │         │
              │      └─────┘      │  │        ●         │
              │         │         │  │        ◡         │
              │         │         │  │        │         │
              │         │         │  │ localhost:3000   │
              │         ▼         │  │        │         │
┌────────┐    │     ┌ ─ ─ ─       │  │  ┌───────────┐   │
│Project │  ┌─┴─┐     /app │      │  │  │Production │   │
│ Folder │──│   │───│             │  │  │  Server   │   │
└────────┘  └─┬─┘    ─ ─ ─ ┘      │  │  └───────────┘   │
              │                   │  │                  │
              │Container: Test    │  │Container: UI     │
              └───────────────────┘  └──────────────────┘
```

## Docker-Compose

The final `docker-compose.yaml` looks like this:

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

    ui-dev:
      image: "node:13.10"
      stdin_open: true
      tty: true
      working_dir: "/app"
      volumes:
        - ./:/app
      ports:
        - 3000:3000
      command: ["sh", "-c", "yarn install && yarn tdd"]

    test:
      build:
        context: .
        dockerfile: "test.Dockerfile"
      depends_on:
        - ui
      stdin_open: true
      tty: true
      working_dir: "/app"
      volumes:
        - ./:/app
      command: ["sh", "-c", "yarn install && yarn test"]


```
