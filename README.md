# Overview

Dockerfile definitions to build a [Wekan](https://wekan.github.io/) development environment for updates to the codebase.

This isn't intended as a replacement to the methods on the main site, just a helper to pull things into a structure that works for me across my dev machines. 

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

## Local Development

Start commands above use volume on *wekan-source* to hold data. Moving *root/repo* content here allows IDE usage on local machine, with meteor rebuild in container on change.

## Starting App

From within app directory, run *meteor*

*NOTE:* If using root in container, use *--allow-superuser* flag

