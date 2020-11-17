######
Docker
######

General
*******

.. code-block:: bash

  docker
  docker --version
  docker version
  docker info

Build
*****

Build an image from a Dockerfile

.. code-block:: bash

  docker build [OPTIONS] PATH | URL | -

*Options*

* :code:`-t` Tag the image.

*Example*

Create an image tagged `debian` using the current directory's Dockerfile

.. code-block:: bash

  docker build -t debian .

Run
***

Run a command in a *new* container.

.. code-block:: bash

  docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]

*Options*

* :code:`-p 4000:80` Map host port 4000 to container port 80
* :code:`-d` Run in detached mode
* :code:`-v /full/path/src:/full/path/dest` mount host's src to container's dest
* :code:`--name` Give a name to the container
* :code:`-d` Run in detached mode (return control to the terminal)
* :code:`-i` interactive (STDIN open even if not attached)
* :code:`-t` Allocate a pseudo-TTY


*Examples*

Start a new container named `my_container` from the image named `debian` exposing the port 80. The container should run in detached mode. 

.. code-block:: bash

  docker run -d --name my_container -p 80:80 debian 


Start interactively a new container from the image named `debian` exposing ports 80 and 443 and mounting the host's `home` directory to the container's `app`.

.. code-block:: bash

  docker container run -it -p 80:80 -p 443:443 -v /home:/app debian /bin/bash



Exec
****

Run a command in a running container. E.g. start a bash shell in a running container:

.. code-block:: bash

  docker exec -it <running_container_name> /bin/bash

* :code:`-i` : interactive
* :code:`-t` : opens a tty connection


Container management
********************

Containers can be managed either with the :code:`docker` or the :code:`docker container` commands. Some examples are given below.  :code:`<docker_id>` can either be the container's name or its hash tag. 

**List all running containers**

.. code-block:: bash

    docker container ls
        # -a show all containers, not just running
        # -q only display numeric ids

**Create container**

Create a new container (syntax similar to docker run).

.. code-block:: bash

    docker create <docker_name>

**Start a stopped container**

.. code-block:: bash

    docker start <docker_id>

**Restart a container**

.. code-block:: bash

    docker restart <docker_id>

**Stop container**

Gracefully stop container

.. code-block:: bash

    docker stop <docker_id>

**Kill a container**

Forcefully stop container

.. code-block:: bash

    docker kill <docker_id>

**Remove specified container**

.. code-block:: bash

    docker rm <docker_id>

**Remove all containers**

.. code-block:: bash

    docker rm $(docker container ls -a -q)


**NUCLEAR: Remove everything**

WARNING! This will remove:

* all stopped containers
* all networks not used by at least one container
* all images without at least one container associated to them
* all build cache

.. code-block:: bash

    docker system prune -a

Image management 
*****************

Images can be managed with the :code:`docker image` command. 

**List images**

.. code-block:: bash

    docker image ls
        # -a show all containers, not just running
        # -q only display numeric ids

**Remove images**

.. code-block:: bash

    docker image rm <image_id>

**Remove all images from this machine**

.. code-block:: bash

    docker image rm $(docker image ls -aq)

**Dangling images**

Sometimes after building an image, some images are also created that have :code:`REPOSITORY` and :code:`TAG` :code:`<none>`. These are called *dangling* images. To find these images use the following

.. code-block:: bash

    docker images -f "dangling=true" 

This will list all the dangling images which can then be deleted with :code:`docker image rm`. Note that the above command uses :code:`images` instead of :code:`image`.






Ps
**

List containers 

.. code-block:: bash

  docker ps

Help on ps

.. code-block:: bash

  docker ps --help

Networking
**********

List all docker networks

.. code-block:: bash

    docker network ls

Inspect the network :code:`<network_name>`. The name can be found from the :code:`ls` command.

.. code-block:: bash

    docker inspect <network_name> 


Image layers and Build Cache
****************************

Docker creates a new image layer each time it executes a `RUN`, `COPY` or `ADD` instruction. If you build the image again, the build engine will check each instruction to see if it has an image layer cached for the operation. If it finds a match in the cache, it uses the existing image layer rather than executing the instruction again and rebuilding the layer. 

For file copying instructions like `COPY` and `ADD`, Docker compares the checksums of the files to see if the operation needs to be performed again. 

https://www.digitalocean.com/community/tutorials/building-optimized-containers-for-kubernetes#managing-container-layers

Various
*******

Log in
======

Log in this CLI session using your Docker credentials

.. code-block:: bash

  docker login

Tags
====

Tag :code:`<image>` for upload to registry

.. code-block:: bash

  docker tag <image> username/repository:tag

Uploading tagged images to registry
===================================

.. code-block:: bash

  docker push username/repository:tag

Prune 
======

Remove stopped containers and all of the images 

.. code-block:: bash

  docker system prune -a

**User-defined bridge networks**

A user-defined bridge network like this enables communication between containers on the same Docker daemon host. This streamlines traffic and communication within your application, since it opens all ports between containers on the same bridge network, while exposing no ports to the outside world. Thus, you can be selective about opening only the ports you need to expose your frontend services. 

Logs
====

Show the logs for a specific container (service)

.. code-block:: bash

  docker-compose logs <service_name>

Copy files 
===========

Copy files between a container and the local filesystem

.. code-block:: bash

   docker cp <container>:/path/to/file /path/to/local/directory




Docker files
************
Start image

.. code-block:: bash

  FROM

Copy the source destination (from the hard drive) to the docker.

.. code-block:: bash

  COPY src dest

Expose port 80, the container will listen to that

.. code-block:: bash

  EXPOSE 80



Docker compose
**************

Run the docker compose file

.. code-block:: bash

  docker-compose up 

* :code:`-d` 

  * Run in a detached mode


More info
*********

`<https://docs.docker.com/install/linux/docker-ce/debian/>`_


Rebuild images:
-------------------

One way of updating a container using latest code is to rebuild the image, and restart the containers. This can be done using

.. code-block:: bash

  docker-compose build --no-cache <service-name>
  docker-compose down
  docker-compose up


Docker registry
***************

Create and upload an image to a (local) registry
================================================

Get the base image

.. code-block:: bash

    docker pull ubuntu:16.04

Tag it including the repository's address

.. code-block:: bash

    docker tag ubuntu:16.04 localhost:5000/my-ubuntu

Alternatively, if you want to build the image from an existing Dockerfile, run

.. code-block:: bash

    docker build -t localhost:5000/my-ubuntu .


Push it to the registry

.. code-block:: bash

    docker push localhost:5000/my-ubuntu

Delete local copies

.. code-block:: bash

    docker image rm ubuntu:16.04
    docker image rm localhost:5000/my-ubuntu

Get it back from the registry

.. code-block:: bash

    docker pull localhost:5000/my-ubuntu

Insecure registries
===================

If the registry is not secured with https, trying to push an image will return the following error

.. code-block::

    This push refers to repository [<server.name>:<port>/<name>]
    Get https://<server.name>:<port>/v2: http: server gave HTTP response to HTTPS client

If this happens, open :code:`/etc/docker/daemon.json` and add the line 

.. code-block::

    {"insecure-registries":["<server.name>:<port>"]}

Then restart the docker service by 

.. code-block:: bash

    service docker restart

Allowing deletes in a registry
==============================

A registry by default does not allow you to delete an image. To enable this functionality, open in the container that runs the registry the file :code:`/etc/docker/registry/config.yml` and add the the :code:`delete:enabled:true` as in the example below.

.. code-block::
   :emphasize-lines: 9,10

    log:
      fields:
        service: registry
    storage:
        cache:
            layerinfo: inmemory
        filesystem:
            rootdirectory: /var/lib/registry
        delete:
            enabled: true
    http:
        addr: :5000

Afterwards, restart the container as

.. code-block::

    docker container restart <container_name>

Deleting an image from the repository
=====================================

Get the image's digest 

.. code-block::

    curl -i -H "Accept: application/vnd.docker.distribution.manifest.v2+json" \
    <server.name>:port/v2/<repo_name>/manifests/<tag>  2>&1 \
    | grep Docker-Content-Digest | awk '{print($2)}'

This will return the digest in the form :code:`sha256:54b69....`.

Note that the Header (:code:`-H`) part is necessary. If not, the digest returned is not the one that will allow us to delete the image. For more see (https://docs.docker.com/registry/spec/api/#deleting-an-image).

Once the digest is obtained run the following to delete the image

.. code-block::

    curl -X DELETE <server.name>:<port>/v2/<repo_name>/manifests/`sha256:54b69....

Get in the registry's container and run the garbage collector

.. code-block::

    /bin/registry garbage-collect /etc/docker/registry/config.yml


Deleting the repository itself
==============================

It is possible to delete a repository after all its images have been deleted. Assuming that a repository has no images, get in the registry's container and run the following

.. code-block::

    /bin/registry garbage-collect /etc/docker/registry/config.yml
    rm -r /var/lib/registry/docker/registry/v2/repositories/<name>

The reference for the registry's api is 
https://docs.docker.com/registry/spec/api/

Docker installation on Centos 8
*******************************


Below are instructions for adding Docker and Docker Compose on Centos 8.

**Docker installation**

Enable Docker CE Repository

.. code-block:: bash

  dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo

Install docker using the DNF command

.. code-block:: bash

  dnf install docker-ce --nobest -y
  systemctl start docker
  systemctl enable docker

Verify and test the Docker CE Engine

.. code-block:: bash

  docker --version
  docker run hello-world

To run docker without root permissions for user 'user_name' 

.. code-block:: bash

  groupadd docker
  usermod -aG docker user_name

**docker-compose installation**

.. code-block:: bash

  dnf install curl -y
  curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  sudo chmod +x /usr/local/bin/docker-compose
  docker-compose --version


It's worth checking the latest docker compose release in https://github.com/docker/compose/releases.


Instructions taken from 
https://www.linuxtechi.com/install-docker-ce-centos-8-rhel-8/






