# Docker

## Dockerfile

`FROM <image name>:<version (optional)>` which image to base the newly created image on.

`LABEL "maintainer"="maintainer@example.com"` searchable metadata.

`USER root` this is the default but can be used to change to another user.

`ENV <NAME> <value>` sets environment variables that can be used later in the file.

`RUN apt-get -y update` runs a command in the image. Not recommended to run the update commands every time but to base the image on another image that contains these updates.

`ADD <source> <destination>` adds source to the destination in the filesystem of the image.

`WORKDIR <destination>` changes workdir to destination.

`CMD [<command>, <options (optional)>` command to run when image starts.

## Images and Image Handling

### Build an Image

$ `docker build -t <build name>:<version> <location of Dockerfile>` builds an image, example `docker build -t rumnoodle/node:latest .`. Cache can be disabled with the `--no-cache` flag.

$ `docker history <image ID or image name>` shows all layers of an image.

### Run an Image

$ `docker run -d -p <container port>:<docker host port> <image name>:<version (optional)>` runs an image. `-d` flag runs it as daemon in the background.

$ `docker ps` lists all running images.

$ `docker images -a` lists all images.

### Troubleshooting a Build

Add something here.

### Image Vulnerabilities Scanning

All commands run on the last built image unless a name is given.

$ `docker scout quickview <image name (optional)>` shows vulnerabilities in the most recently built image unless an image is provided. [Documentation](https://docs.docker.com/engine/reference/commandline/scout_quickview/ "Docker scout quickview documentation")

$ `docker scout cves <image name (optional)>` lists vulnerabilities.

$ `docker scout recommendations <image name (optional)>` shows update recommendations for base image. Adding the `--org <organization>` option includes the organization's policy results.
