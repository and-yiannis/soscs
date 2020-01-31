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

Containers can be managed either with the :code:`docker` or the :code:`docker container` commands. Some examples are given below. 

* :code:`docker container ls`

  * List all running containers

* :code:`docker container ls -aq` 

  * List all containers, showing only the hashes

* :code:`docker create <docker_name>`

  * Create a new container (syntax similar to docker run).

* :code:`docker start <docker_id>`

  * Restart a non-running container 

* :code:`docker stop <docker_id>`

  * Gracefully stop container

* :code:`docker kill <docker_id>`

  * Forcefully stop container

* :code:`docker rm <docker_id>`

  * Remove specified container

* :code:`docker rm $(docker container ls -a -q)`

  * Remove all containers

:code:`<docker_id>` can either be the container's name or its hash tag. 



Image management 
*****************

Images can be managed with the :code:`docker image` command. 

Some examples.

* :code:`docker image ls`

  * List images

* :code:`docker image ls -a`  

  * List all images

* :code:`docker image rm <image_id>`

  * Remove image

* :code:`docker image rm $(docker image ls -aq)`

  * Remove all images from this machine





Ps
**

List containers 

.. code-block:: bash

  docker ps

Help on ps

.. code-block:: bash

  docker ps --help

Image layers and Build Cache
****************************

Docker creates a new image layer each time it executes a `RUN`, `COPY` or `ADD` instruction. If you build the image again, the build engine will check each instruction to see if it has an image layer cached for the operation. If it finds a match in the cache, it uses the existing image layer rather than executing the instruction again and rebuilding the layer. 

For file copying instructions like `COPY` and `ADD`, Docker compares the checksums of the files to see if the operation needs to be performed again. 

https://www.digitalocean.com/community/tutorials/building-optimized-containers-for-kubernetes#managing-container-layers

Various
*******

Log in this CLI session using your Docker credentials

.. code-block:: bash

  docker login

Tag :code:`<image>` for upload to registry

.. code-block:: bash

  docker tag <image> username/repository:tag

Upload tagged image to registry

.. code-block:: bash

  docker push username/repository:tag

Remove stopped containers and all of the images 

.. code-block:: bash

  docker system prune -a

**User-defined bridge networks**

A user-defined bridge network like this enables communication between containers on the same Docker daemon host. This streamlines traffic and communication within your application, since it opens all ports between containers on the same bridge network, while exposing no ports to the outside world. Thus, you can be selective about opening only the ports you need to expose your frontend services. 

**Logs**

Show the logs for a specific container (service)

.. code-block:: bash

  docker-compose logs <service_name>


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














