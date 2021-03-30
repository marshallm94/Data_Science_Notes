The below notes were taken while going through the Introduction to Docker Learning Path on
Cloud Academy

[TOC]

# What is Docker?

* Docker is a container platform that allows you to package everything that your code needs to run (the source code
  itself, specific versions of third party libraries, binary files, OS, etc.) into **one** self contained identity that
  can be run on any machine with the Docker engine installed.
	* Can be thought of as having a single executable, regardless of the target OS.
* This greatly simplifies the deployment process - instead of checking that all dependencies are the same on a target
  server (in Production, for example), all you have to do is make sure Docker is installed.
* Containers are isolated from other containers that are running on the same machine.
* Docker can be installed on the 3 top OS' from [this link](https://docs.docker.com/get-docker/).
* Note that Docker is not the only container technology, and containers existed prior to docker.

## Docker Architecture

* Uses a client-server architecture:
	* Server = Docker daemon
		* Exposes REST API
	* Client = Docker binary (i.e. the `$ docker build`, `$ docker pull`, `$ docker run` are all calling the
	  `docker` binary)
* [Dockerhub](https://hub.docker.com/) is the 'main' repository/registry for docker images.

## Technologies

There are various technologies that make docker able to run processes in isolated environments. Some key technologies
are:

**Namespaces**
* pid namespace: Process isolation (PID = process ID).
* net namespace: Managing network interfaces (NET = Networking).
* ipc namespace: Managing access to IPC resources (IPC = InterProcess Communication).
* mnt namespace: Managing filesystem mount points (MNT = Mount).
* uts namespace: Isolating kernel and version identifiers (UTS = Unix Timesharing System).

**Control Groups (aka C Groups)**
* Resource limiting: groups can be set to not exceed a configured memory limit.
* Prioritization: some groups may get a larger share of CPU utilization of disk I/O throughput.
* Accounting: measures a group's resource usage.
* Control: freezing groups of processes.

**Union FS (File System)**
* Merging: overlay filesystem branches to merge changes.
* Read/Write: brances can be read-only or read-write.

# Images & Containers

## Images vs. Containers

* Containers are instances of images; another way of saying that is, "images are templates from which containers are
  built."
	* An important point here is that once a container is created (using `$ docker build`), any changes made to
	  the images **will not** affect the already built container.
	* Another way of saying this is, "Each time you create a container, that container is based on how the image
	  looks **at the moment the container is created.**"
* Images are based on "layers" - all layers together contain everything that is necessary to run your application.

## Images From the Dockerfile 

* **Creating images from a Dockerfile should be your default method for creating images.**
* Images are created from the Dockerfile.
* Dockerfiles are texts files with commands that are used to create images.
* A Dockerfile is called just that; no extenstion, just `Dockerfile`.
* The `CMD` instruction in the Dockerfile is the default command to run when a container first starts. **There should be
  one `CMD` instruction per Dockerfile.**

## Images From Containers 

* Although creating an image from a Dockerfile, and then creating a container from said image is the default way of
  creating a container, it isn't the only way.
* Images can be created from containers using the `$ docker commit` command:
	1. Start a container that will serve as the "base" for your new image. 
	2. Once you are "inside" the running container, make any changes you would like.
	3. Exit the running container.
	4. Run `$ docker commit <CONTAINER_ID> <NEW_IMAGE_NAME>`, which will create a new image <NEW_IMAGE_NAME> based
	   off the container <CONTAINER_ID>.
* Note that if you want to change the CMD of the container, you can do so while running the `$ docker commit` command by
  running `$ docker commit --change='CMD <whatever_you_want_the_CMD_to_be>'`

# Port Mapping 

* Port mapping, as it sounds, maps exposed ports on the container to available ports of the host machine on which the
  container is running.
* Port mapping can be done dynamically with the publish all flag,  `$ docker run -P`. When this is run, the exposed ports
  of the container are mapped to available ports of the host randomly.
* Port mapping can be done manually with the publish flag `$ docker run --p <host_port>:<container_port>`. When this is
  run, <container_port> is explicitly mapped to <host_port> of the host.

# Docker Command List

* `$ docker run` - run a docker containter.
	* `$ docker run -d` - run a docker containter *in detached mode* (allows the use of the terminal).
	* `$ docker run -d -P` - run a docker containter *in detached mode* (allows the use of the terminal) and maps the
	  container ports to ports of the host machine.
* `$ docker build -t <desired_repo_name> <path_to_directory_where_dockerfile_is_located>` - build a container from an
  image as outlined in the Docker file.
* `$ docker pull` - pull a container from a registry.
* `$ docker images` - list the images stored locally (stored in `/var/lib/docker/`).
* `$ docker start <container_name>` - started a container named <container_name>.
* `$ docker attach <container_name>` - make an already running container (named <container_name>) interactive.
* `$ docker ps` - list all containers.
* `$ docker ps -a` - list all *running* containers.
* `$ docker prune` - **permanently delete** all local, non-running containers.
* `$ docker rm <container_name>` - **permanently delete** the container named <container_name>.
* `$ docker commit `- Used to commit a conatiner's file changes or settings into a new images.
