############
Debian Setup
############


Virtual Box additions
*********************
Follow the next steps to install the virtual box additions.

* Login as root
* :code:`apt-get update`
* :code:`apt-get upgrade`
* Install required packages with :code:`apt-get install build-essential module-assistant`
* Configure your system for building kernel modules by running :code:`m-a prepare`
* On the VM window, click on Devices > Install Guest Additions. 
* In the terminal run :code:`mount /media/cdrom`.
* Run :code:`sh /media/cdrom/VBoxLinuxAdditions.run`, and follow the instructions on screen.

Shared folder
============

* In Virtual box add a folder in Shared folders
* In the Guest, as an administrator type

.. code-block:: bash

    mount -t vboxsf <name_of_shared_folder> <path/to/the/shared/folder/in/the/guest>

* :code:`name_of_shared_folder` is the name of the folder virtual box gives to the shared folder in the host, not the path to the actual folder on the host.

Tmux
****

* :code:`apt-get install tmux`
* copy the :code:`.tmux` file from the host and make the necessary modifications

Vim
***
* :code:`apt-get install vim`


Git
***

* :code:`apt-get install git`
* Add the private ssh key in :code:`~/.ssh`
* write the following in the :code:`~/.ssh/config` file.

.. code-block:: bash

    Host gitlab.com
      Preferredauthentications publickey
      IdentityFile ~/.ssh/<name_of_the_key>

Docker
******

Run all the following as superuser.

.. code-block:: bash

    apt-get remove docker docker-engine docker.io containerd runc

.. code-block:: bash

    apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common

.. code-block:: bash

    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -

.. code-block:: bash

    add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/debian \
       $(lsb_release -cs) \
       stable"

.. code-block:: bash

    apt-get update

.. code-block:: bash

    apt-get install docker-ce docker-ce-cli containerd.io

Info available in:
`<https://docs.docker.com/install/linux/docker-ce/debian/>`_

To make docker available to :code:`user`, run

.. code-block:: bash

  groupadd docker

.. code-block:: bash

  usermod -aG docker user

and restart the VM.


Docker compose
**************

.. code-block:: bash

    curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

.. code-block:: bash

    sudo chmod +x /usr/local/bin/docker-compose

Install node and npm
********************


.. code-block:: bash

    apt-get update

.. code-block:: bash

    apt-get install curl

.. code-block:: bash

    curl -sL https://deb.nodesource.com/setup_12.x | bash -

.. code-block:: bash

    apt-get install -y nodejs

.. code-block:: bash

    apt-get install -y build-essential


Change the time zone
********************

Change the time zone using :code:`timedatectl`.

.. code-block:: bash

    timedatectl Europe/London


:code:`timedatectl list-timezones` shows a list of available timezones.


