########
OpenVPN
########

Below are instructions for setting up an OpenVPN connection on an Ubuntu machine

We assume that we have a :code:`client.ovpn` file, a :code:`username` and a :code:`password` for the connection.

The openvpn.service
###################
Install openvpn using

.. code-block:: bash

   sudo apt install openvpn

Make the :code:`openvpn.service` restart automatically 

.. code-block:: bash

   sudo vi /etc/default/openvpn

and uncomment the line :code:`AUTOSTART="all"`

To **stop** the service run

.. code-block:: bash

   sudo /etc/init.d/openvpn stop
   pgrep openvpn 
   pgrep openvpn |xargs sudo kill -9
   sudo systemctl daemon-reload
   sudo systemctl reset-failed

To **start** the service run

.. code-block:: bash

   sudo /etc/init.d/openvpn start 

*Note*: If the directory :code:`/etc/openvpn` has any :code:`*.conf` files, the corresponding connections will (attempt to) start automatically.


Setting up a connection
#######################

Create a file named :code:`pass` and enter the :code:`username` and the :code:`password`, each in its own line. Give it the following ownership and permissions

.. code-block:: bash

   sudo chown root:root pass
   sudo chmod 400 pass

Then move it in the :code:`/etc/openvpn` directory


Open the :code:`client.ovpn` file and change the line 

.. code-block:: bash

   auth-user-pass 

to 

.. code-block:: bash

   auth-user-pass pass

where the second :code:`pass` is the name of the file with the credentials.

Rename the file to :code:`client.conf`,  give it the following ownership and permissions

.. code-block:: bash

   sudo chown root:root client.conf
   sudo chmod 644 client.conf

Then move it in the :code:`/etc/openvpn` directory.

By the time you move it in the :code:`/etc/openvpn` directory, if the :code:`openvpn.service` is running, it will attempt to start the connection.

Enabling and Running the connection
###################################

If everything went to plan, :code:`openvpn.service` must have created a service for this connection, called :code:`openvpn@client`, or in general :code:`openvpn@<name_of_the_conf_file>`

To start the service and enable it (i.e. have it restart on reboot) run the following.


.. code-block:: bash

    sudo systemctl enable openvpn@client.service
    sudo systemctl daemon-reload
    sudo systemctl start openvpn@client.service

At the end of this we should have a VPN connection that persists on reboot.


Stopping the connection
#######################

To stop a VPN connection do the following

.. code-block:: bash

    sudo systemctl disable openvpn@client.service
    sudo systemctl stop openvpn@client.service

Running 

.. code-block:: bash

    sudo systemctl status openvpn@client.service

should return the PID of the process. Killing it with :code:`kill -9 <PID>` should clean up everything. Removing the :code:`client.conf` and :code:`pass` files from :code:`/etc/openvpn`, will prevent the :code:`openvpn.service` to restart it next time the computer boots.



General systemctl commands
##########################


.. code-block:: bash

   # Start a service 
   sudo systemctl start <service_name>

   # Stop a service 
   sudo systemctl stop <service_name>

   # 'Enable' a service  (will start automatically on reboot)
   sudo systemctl enable <service_name>

   # 'Disable' a service  (will not start automatically on reboot)
   sudo systemctl disable <service_name>

   # Show a service's status
   sudo systemctl status <service_name>

   # List all services
   sydo systemctl list-units --type=service



References
##########

https://www.ivpn.net/knowledgebase/linux/linux-autostart-openvpn-in-systemd-ubuntu/


