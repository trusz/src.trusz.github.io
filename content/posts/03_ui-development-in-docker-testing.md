+++
_title = "{{ replace .Name `-` ` ` | title }}"
title = "UI Development in Docker — Testing"
date = "{{ .Date }}"
lastmod = "{{ .Date }}"
tags = ["docker", "ui", "dx", "testing", "tdd"]
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
> 2. [UI Development in Docker — CI/CD](/posts/02_ui-development-in-docker-cicd)
> 3. UI Development in Docker — Testing

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
