####
Win
####

PIDs, Ports, Services
#####################

1) Which process is listening on a port?
****************************************
The Visual way:
===============

Run :code:`resmon.exe`, go to the Network Tab, and check the Listening Ports. Within there you can find the port and the PID of the process that's listening.

The Command line way
====================

Run in a powershell

.. code-block:: bash

    netstat -ano | findstr "portNo"

The last number that will pop up is the PID

2) Which service started the listening process?
***********************************************
To find this we need to find the parent process of the PID that's listening in the port. This can be done with the command

.. code-block:: bash

    wmic process get processid,parentprocessid | findstr "PID"

The first number that will pop up is the PID of the service. 

Note: This command can only be executed from the **command prompt** and **not** from the *powershell*.

3) Find Service based on PID
****************************
* Open the Task Manager, and go to the Services tab and find the service by PID. 
* Right click and press 'Open Services'
* Find the service in the Services, right click and press Properties.

From there it should be easy to find details about the service.



nssm
####

To show service installation GUI:

.. code-block:: bash

        nssm install [<servicename>]

To install a service without confirmation:

.. code-block:: bash

        nssm install <servicename> <app> [<args> ...]

To show service editing GUI:

.. code-block:: bash

        nssm edit <servicename>

To retrieve or edit service parameters directly:

.. code-block:: bash

        nssm get <servicename> <parameter> [<subparameter>]

        nssm set <servicename> <parameter> [<subparameter>] <value>

        nssm reset <servicename> <parameter> [<subparameter>]

To show service removal GUI:

.. code-block:: bash

        nssm remove [<servicename>]

To remove a service without confirmation:

.. code-block:: bash

        nssm remove <servicename> confirm

To manage a service:

.. code-block:: bash

        nssm start <servicename>

        nssm stop <servicename>

        nssm restart <servicename>

        nssm status <servicename>

        nssm rotate <servicename>

