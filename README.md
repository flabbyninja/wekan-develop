# Overview

Dockerfile definitions to build a [Wekan](https://wekan.github.io/) local development environment.

This isn't intended as a replacement to the instructions on the main Wekan site, it's just there to help me to pull things into a structure that works with the way I've set up my dev machines. 

# Structure

* **base** sets up Ubuntu with build and node 9.x dependencies
* **wekandev** builds on top of base to add meteor, node-gyp and checkout and build of latest devel branch version of wekan

wekandev contains code built under `root/repos`, following base pattern from wekan the rebuild scripts for virtualbox.

## Starting Containers

This is set up to use a separate MongoDB instance rather than the built-in meteor Mongo instance.

*Caution:* This is for development only. The data volume is inside the container. Follow the prod setup guide from [Wekan](https://wekan.github.io/) wiki to use for prod.

### MongoDB 

`docker run --name wekan-mongo -d mongo`

### Base app

`docker run --privileged=true --link wekan-mongo:mongo -p 3000:3000 -it -v wekan-source:/wekan-source flabbyninja/wekandev:latest bash`

*TODO:* Docker compose config to start this seamlessly

## Local Development

The 'start commands' above use a volume on `wekan-source` to hold data. Moving the `root/repos` content locally allows editing on an IDE on the local machine, with meteor rebuilding within the container when it detects changes.

**NOTE:** Link the `.meteor` subdirectory back to the container filesystem on `/root/repos/wekan/.meteor` or the rebuilds can cause permission issues on the mounted Docker volume that requires restarts

## Starting App

From within the app directory, run `meteor`

*NOTE:* If using root in container (not recommended), use `--allow-superuser` flag

