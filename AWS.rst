#######
AWS
#######

This page contains information about using the Amazon Web Services platform.

Clone an AWS instance
######################

* Go to the EC2 Dashboard and select the Running instances.
* Select the instance you want to clone and click :code:`Actions > Image > Create Image`.
* Select a name for the new image and click :code:`Create Image`.
* Go to :code:`Images > AMIs` and select the newly created image.
* Click Launch and launch the new EC2.
* After a successful launch, delete the Image if no longer required. 


Elastic IP Association / Disassociation
######################################

* Go to :code:`Elastic IPs`
* Select the IP address and in the :code:`Actions` menu click :code:`Disassociate`
* Select the IP address again and associate with the new instance.



Docker installation on Amazon Linux
###################################


Below are instructions for adding Docker and Docker Compose on the Amazon EC2 instance, specifically on the Amazon Linux 2 AMI. Before proceeding, make sure you're logged in as :code:`ec2-user`.

**Docker installation**

.. code-block:: bash

  sudo yum update -y
  sudo yum install -y docker
  sudo service docker start
  sudo usermod -aG docker ec2-user

After completing the above steps, log out and in again for the :code:`docker` group to take effect on the :code:`ec2-user` user.

**Docker-compose installation**

.. code-block:: bash

    curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

.. code-block:: bash

    sudo chmod +x /usr/local/bin/docker-compose

It's worth checking the latest docker compose release in https://github.com/docker/compose/releases.

