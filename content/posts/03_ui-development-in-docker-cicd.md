+++
_title = "{{ replace .Name `-` ` ` | title }}"
title = "UI Development in Docker — CI/CD"
date = 2020-05-05
tags = ["docker", "ui", "dx", "ci/cd", "testing"]
categories = ["UI Development in Docker"] 
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
> 1. [UI Development in Docker — Basics](/posts/01_ui-development-in-docker-basics)
> 2. [UI Development in Docker — Testing](/posts/02_ui-development-in-docker-testing)
> 3. UI Development in Docker — CI/CD

## Testing in CI/CD Pipeline

The main difference between testing locally and in a pipeline is that we start
up the production build of the UI and we start a separate container with the tests and
headless chromium. This is quite easy as Chromium and [Chrome Puppeteer](https://github.com/puppeteer/puppeteer) are also available in a Docker container: [Docker Hub](https://hub.docker.com/search?q=puppeteer&type=image).

The source files are still mounted in so we don't have to build
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

> **Need an example?**  
> Check out our starter kit for [↗ React + Storybook + TypeScript](https://github.com/sprinteins/starter-kits/tree/master/react-storybook-typescript)
