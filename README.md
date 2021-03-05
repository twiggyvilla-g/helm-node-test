# helm-node-test

A really basic helm chart to show how to run locally on my mac...

Assuming you already have docker and kubectl installed...

Before spinning up the helm chart locally you need to:

1. Build the node image locally (so it's available on your machine)
2. Install ```kind``` (a local kubernetes)

## Build the image locally

Clone locally and run through the steps in https://github.com/twiggyvilla-g/docker_node_test/blob/main/README.md

```docker images``` - ensure node:0.1.1 image is available

## Install kind and helm

```brew install kind```

```kind create cluster --name local```

```docker ps``` - you should see a container running for kind (https://iximiuz.com/en/posts/kubernetes-kind-load-docker-image/) - for instance:

```
CONTAINER ID   IMAGE                  COMMAND                  CREATED        STATUS        PORTS                       NAMES
e141a505e14a   kindest/node:v1.20.2   "/usr/local/bin/entr…"   19 hours ago   Up 19 hours   127.0.0.1:62909->6443/tcp   local-control-plane
```

We can now setup helm:

```brew install helm```

## Spin up node-test helm chart

Clone this repo locally and cd into it

This chart is using the custom node image from https://github.com/twiggyvilla-g/docker_node_test - we need to load this image into kind so it can be found when the helm chart wants to use it:

```kind load docker-image node:0.1.1 --name local```

To check the chart without actually starting it:

```helm install node-test --dry-run --debug .```

To start the chart:

```helm install node-test .```

You will be given the instructions on how to open it locally (a couple of export commands and a kubectl command for forwarding traffic to the container port)

To stop and remove the running chart:

```helm delete node-test```
