####
Win
####

PIDs, Ports, Services
#####################
To see the services Run :code:`services.msc`

To see what's listening on each port Run :code:`resmon.exe`, go to the Network Tab, and check the Listening Ports.

To see the PID of a service open the task manager and go to the Services tab.

To see the executable for a given PID, open the task manager and go to the Details tab.

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

