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

## Images and Containers

### Build an Image

$ `docker build .` to build an image from a Dockerfile in the current directory.

$ `docker build -t <build name>:<version> <location of Dockerfile>` builds an image, example `docker build -t rumnoodle/node:latest .`. Cache can be disabled with the `--no-cache` flag.

Some useful options for build include `--platform linux/amd64` to build an image that wasn't originally built for arm platform.

$ `docker images` lists all created images.

$ `docker tag <old tag> <new tag>` to change a tag. The first part of the tag needs to match the account on docker hub to be uploadable e.g. `example/some-image:latest` example must match the account name used when loggin in. This adds a tag to the same image, after this command both tags will be available.

$ `docker history <image ID or image name>` shows all layers of an image.

$ `docker export <container ID> -o <outfile.tar>` exports an image to a tar file named by the `-o` option.

$ `docker image inspect <image name>` to inspect an image. It won't pull image so you'll need to do that first if it doesn't exist.

$ `docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh` to enter an image and explore the filesystem shown when running the inspect command (see for instance values in GraphDriver.)

### Clean Up Images and Containers

$ `docker images` lists all created images.

$ `docker rm <container name>` removes a container.

$ `docker rmi <image name>` removes an image and all associated layers.

$ `docker system prune` removes all images and a bit more, `-a` option removes only unused images.

$ `docker rm $(docker ps -a -q)` remove all containers.

$ `docker rmi $(docker images -q)` remove all images.

### Run a Container

`docker run` is a combination of the two commands `docker create` and `docker start`.

$ `docker run -d -p <container port>:<docker host port> <image name>:<version (optional)>` runs an image. `-d` flag runs it as daemon in the background.

$ `docker run -ti <image name>:<version> /bin/sh` to start an image and run the shell. `-i` flag says to run in interactive mode.

```
-e NAME="value : set an environment variable in the image
-i : start interactive session, keeps STDIN open
-l <label name>="<label value>" : sets a label with label of label name and value of label value
-t : allocate a pseoudo TTY
--cpu-shares <0 to 1024> : how many cpu shares to allocate to the image when it runs where 1024 is full and 0 is none. A container with 512 will execute a cycle for half as long as a container with 1024.
--cpuset=0 : run container in CPU 0 of host, "cannot start container" error if number >= number of cores.
--cpus="<0.01 to number of cores>" : set how much CPU should be available to the image.
--hostname="<name of host.com>" : sets hostname to name of host.com
--memory <memory in b, k, m, or g> : how mych RAM and swap to use (unless --memory-swap set in which case they're set separately and swap is memory-swap - memory, a value of -1 in swap disables it. --kernel-memory limits the amount of kernel memory)
--name="<container name>" : sets the image name to container name
--oom-kill-disable, --oom-score-adj : disables killing processes when running out of memory and other things related to that.
--platform linux/amd64 : run an image not compatible with mac new arm platform on a mac
--read-only=true : makes the root of the filesystem in the container read only
--restart <no, always, on-failure, or unless-stopped> : restarts container if circumstances fit.
--rm : remobes the image when it exits
--tmpfs /tmp:mount,options,size=256M : mounts a temporary file system that is lost on exit with mount options as a comma separated list after the name and a size of 256 megabytes
--ulimit <key>=<value> : sets limits on filesystem resources in the container.
```

$ `docker stop <container ID or image name>` stops an image from running. `-t <seconds>` kills the container after a specified amount of seconds unless it was able to stop on its own accord. `docker kill` forces it to stop immediately.

$ `docker pause` or `docker unpause` pauses and resumes execution of a container.

$ `docker ps` lists all running images. 

```
-a : show all containers, not only running ones.
-q : same as --quiet.
--filter <filter> : filters containers e.g. 'exited!=0' includes all containers that exited with non 0 exit status.
--format "table {{.ID}}\t{{.Image}}\t{{.Status}}" : format the output.
--quiet : shows only container ID.
```

Labels are searchable e.g. `docker ps -a -f label=<name>=<value>` searches for a label with the name name and value value.

$ `docker images` lists all images. `-a` option lists all images?

$ `docker container update` updates a running docker image.

### Troubleshooting a Build

Add something here.

### Image Vulnerabilities Scanning

All commands run on the last built image unless a name is given.

$ `docker scout quickview <image name (optional)>` shows vulnerabilities in the most recently built image unless an image is provided. [Documentation](https://docs.docker.com/engine/reference/commandline/scout_quickview/ "Docker scout quickview documentation")

$ `docker scout cves <image name (optional)>` lists vulnerabilities.

$ `docker scout recommendations <image name (optional)>` shows update recommendations for base image. Adding the `--org <organization>` option includes the organization's policy results.


### Docker Hub

$ `docker login <url to registry unless docker hub (optonal)>` to log in and you will be asked for your username and password.

$ `docker logout` to log out and stop caching credentials in ~/.docker/config.json.

$ `docker push <image name>:<version>` to push an image, e.g. `docker pull example/some-image:latest`.

$ `docker pull <image name>:<version>` to pull an image.

### Other Stuff

$ `docker info` shows information about the current docker runtime.
