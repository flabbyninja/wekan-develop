# Overview

Dockerfile definitions to build a [Wekan](https://wekan.github.io/) development environment.

# Structure

* **base** sets up Ubuntu with build and node 9.x dependencies
* **wekandev** builds on top of base to add meteor, node-gyp and checkout and build of latest dev version of wekan

wekandev contains built code under root/repos, following base pattern from wekan rebuild scripts for virtualbox.

## Starting Containers

This is set up to use a separate MongoDB instance rather than built in meteor instance.

*Caution:* This is for development only. Data volume is inside container.

### MongoDB 

docker run --name wekan-mongo -d mongo

### Base app

docker run --privileged=true --link wekan-mongo:mongo -p 3000:3000 -it -v wekan-source:/wekan-source flabbyninja/wekandev:latest bash

*TODO:* This is first version. Docker compose config will be provided to start this seamlessly