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
To find this we need to find the parent process of the PID that's listening in the port. This can be done in the following ways:

The Command line way
====================

.. code-block:: bash

    wmic process get parentprocessid,processid | findstr "PID"

The result will have two columns, the first is the parentprocess id and the second the process id.

The Powershell way
==================

.. code-block:: bash

   Get-WmiObject win32_process | ?{$_.processId -like '*PID*'} | select Name, processId, parentprocessId, CreationDate

To see all the available fields, execute

.. code-block:: bash

   Get-WmiObject win32_process | ?{$_.processId -like '*PID*'} | Format-List -Property *


3) Find a service based on PID
****************************
The Visual way:
===============
* Open the Task Manager, and go to the Services tab and find the service by PID. 
* Right click and press 'Open Services'
* Find the service in the Services, right click and press Properties.

From there it should be easy to find details about the service.

The Command line way
====================

.. code-block:: bash

   wmic service get | findstr "PID"

The Powershell way
==================

.. code-block:: bash

   Get-WmiObject win32_service | ?{$_.processId -like '*PID*'} | Format-List -Property *

4) nssm based services
**********************

To See all the services running with nssm:

The Command line way
====================

.. code-block:: bash

   wmic service get | findstr "nssm"

The Powershell way
==================

.. code-block:: bash

    Get-WmiObject win32_service | ?{$_.PathName -like '*nssm*'} | select Name, DisplayName, State, PathName

The same can be done for node processes/services

5) List and kill processes
**************************

.. code-block:: bash

   tasklist
   taskkill

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

