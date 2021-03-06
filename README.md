[![Build Status](https://travis-ci.org/jupyter-incubator/dashboards_nodejs_app.svg?branch=master)](https://travis-ci.org/jupyter-incubator/dashboards_nodejs_app) [![Google Group](https://img.shields.io/badge/-Google%20Group-lightgrey.svg)](https://groups.google.com/forum/#!forum/jupyter)

# Jupyter Dashboards Server

This repo contains a NodeJS application that can display Jupyter notebooks as dynamic dashboards outside of the Jupyter Notebook server. The behavior of the application is similar to that of [Thebe](https://github.com/oreillymedia/thebe), but with some key technical differences:

* The notebook code is visible only in the NodeJS application backend.
* The NodeJS backend is the only actor that can send notebook code to a kernel for execution.
* The browser client can only send [Jupyter comm messages](http://jupyter-client.readthedocs.org/en/latest/messaging.html#opening-a-comm) to kernels (*not* arbitrary code).
* The application uses the jupyter-js-services and jupyter-js-widgets libraries for communication with kernels.

**Note** that this project is very much a work-in-progress as part of the [dashboards deployment roadmap](https://github.com/jupyter-incubator/dashboards/wiki/Deployment-Roadmap).

## Try It

Running the demos in this repository requires Docker. A simple way to run [Docker](https://www.docker.com/) is to use [docker-machine](https://docs.docker.com/machine/get-started/). After setting up Docker, do the following:

```
make build
make demo-container
```

Open your web browser and point it to the dashboards server running on your Docker host at `http://<docker host ip>:3000/`.

To see another notebook as a dashboard:

1. Create a dashboard layout in Jupyter Notebook using the `jupyter_dashboards` extension.
2. Copy the `*.ipynb` file to the `data/` directory in the project root.
3. Run `make demo-container` again -- this will rebuild the Docker image and restart the Node application container.

**Note again** that this project is a work in progress and so many notebooks with dashboard layouts and interactive widgets will not work here yet.

## Deploy It

A minimal deployment of the dashboards server has the following components:

![Minimal dashboard app deployment diagram](etc/simple_deploy.png)

For more details, including use cases and alternative deployments, see the [dashboards deployment roadmap](https://github.com/jupyter-incubator/dashboards/wiki/Deployment-Roadmap).

## Develop It

You can use the Try It setup above for development, but any change you make to the source will require a restart of the dashboard server container. A better approach is to install the following on your host machine:

* Node 5.5.0
* npm 3.5.3
* gulp 3.9.0
* Docker 1.9.1
* Docker Machine 0.5.6

With these installed, you can use the `make dev-*` targets. Under the covers, these targets use `gulp` to automatically rebuild and restart the dashboard server any time you make a code change. Run `make help` to see the full gamut of targets and options. See the next few sections for the most common patterns. Of course, you can mix and match.

### Dashboard Server w/ Auto Restart

```bash
make dev
# mac shortcut for visiting URL in a browser
open http://127.0.0.1:3000
```

### Dashboard Server w/ Auto Restart and Debug Console Logging

```bash
make dev-logging
# mac shortcut for visiting URL in a browser
open http://127.0.0.1:3000
```

### Dashboard Server w/ Auto Restart and Remote Debugging

```bash
npm install -g node-inspector
make dev-debug
# a browser tab should open with the debugger visible
# refresh if it errors: the server might not be running yet
```

### Dashboard Server w/ Auto Restart and Form Auth

```bash
make dev USERNAME=admin PASSWORD=password
# mac shortcut for visiting URL in a browser
open http://127.0.0.1:3000
```

### Dashboard Server w/ Auto Restart and Self-Signed HTTPS Certificate

```bash
make certs
make dev HTTPS_KEY_FILE=certs/server.pem HTTPS_CERT_FILE=certs/server.pem
# mac shortcut for visiting URL in a browser
open https://127.0.0.1:3001
```

### Dashboard Server Tests

```bash
# unit tests
make test
# backend integration tests
make integration-test
```
