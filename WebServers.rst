##########
Webservers
##########

Https setup instructions
########################


**Install packages**

.. code-block:: bash

  apk update
  apk add netcat-openbsd bc curl wget git bash
  apk add libressl
  apk add openssl


**Install acme.sh client**

.. code-block:: bash

    cd /tmp/
    git clone https://github.com/Neilpang/acme.sh.git
    cd acme.sh/
    ./acme.sh --install
    ln -s /root/.acme.sh/acme.sh /usr/sbin/acme.sh

**Create .well-known/acme-challenge/ directory**

Let `D` be the full path to the directory from where the app is served, e.g. `D=/app/dist`.

.. code-block:: bash

   mkdir -vp ${D}/.well-known/acme-challenge/
   chown -R nginx:nginx ${D}/.well-known/acme-challenge/
   chmod -R 0555 ${D}/.well-known/acme-challenge/

**Generate a global dhparam file**

Let `I` be the site's web address (e.g. `I=google.com` if you wanted to secure google!)

.. code-block:: bash

   mkdir -p /etc/nginx/ssl/letsencrypt/${I}/
   cd /etc/nginx/ssl/letsencrypt/${I}
   openssl dhparam -dsaparam -out dhparams.pem 4096

**Issue a certificate for the domain**

Before running this, make sure that the webserver is up (e.g. by running `nginx`)

.. code-block:: bash

   acme.sh --issue -w $D -d $I -k 4096


**Configure TLS/SSL on Nginx web server**

Replace <_Domain_Name_> with the domain's name (e.g. google.com).
Replace <_Root_Dir_> with the site's root directory.

Store the following code in the file `/etc/nginx/conf.d/ssl.<_Domain_Name_>.conf`

.. code-block:: bash

   ## START: SSL/HTTPS <_Domain_Name_> ###
   server {
       listen 443 ssl;
       server_name <_Domain_Name_>;
       ssl_certificate /root/.acme.sh/<_Domain_Name_>/<_Domain_Name_>.cer;
       ssl_certificate_key /root/.acme.sh/<_Domain_Name_>/<_Domain_Name_>.key;
       ssl_session_timeout 1d;
       ssl_session_cache shared:SSL:50m;
       ssl_session_tickets off;
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
       ssl_ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS;
       ssl_dhparam /etc/nginx/ssl/letsencrypt/<_Domain_Name_>/dhparams.pem;
       ssl_prefer_server_ciphers on;
   
       ## Improves TTFB by using a smaller SSL buffer than the nginx default
       ssl_buffer_size 8k;
   
       ## Enables OCSP stapling
       #ssl_stapling on;
       #resolver 8.8.8.8;
       #ssl_stapling_verify on;
   
       ## Send header to tell the browser to prefer https to http traffic
       #add_header Strict-Transport-Security max-age=31536000;
   
       ## SSL logs ##
       access_log /var/log/nginx/<_Domain_Name_>_ssl_access.log;
       error_log /var/log/nginx/<_Domain_Name_>_ssl_error.log;
       #-------- END SSL config -------##
   
      root <_Root_Dir_>;
      index index.html index.htm index.php;
      server_name <_Domain_Name_>;
      location / {
       }
   }
   ## END SSL <_Domain_Name_>


**Install the issued certificate to Nginx web server**

.. code-block:: bash

   acme.sh --installcert -d ${I} \
   --keypath /etc/nginx/ssl/letsencrypt/${I}/${I}.key \
   --fullchainpath /etc/nginx/ssl/letsencrypt/${I}/${I}.cer \
   --reloadcmd 'nginx -s reload'


**Manual renewal**

.. code-block:: bash

   acme.sh --renew -d <_Domain_Name_>

**Upgrade the client**

.. code-block:: bash

   acme.sh --upgrade


Apache on Linux
###############

Apache can be used as a:

 * Proxy server
 * Load Balancer
 * Web server

A web server is a software with a primary function to store, process and deliver web pages to clients.

Useful tutorials: https://httpd.apache.org/docs/2.4/howto/

Installation
************

.. code-block:: bash

   sudo apt-get update && sudo apt-get upgrade
   sudo apt-get install apache2
   systemctl status apache2.service

Key folders and files
*********************

The key folders and files in the apache configuration directory are

.. code-block:: bash

   # Files
   apache2.conf # The configuration file
   envvars
   magic
   ports-conf

   # Directories
   conf-available
   conf-enabled
   mods-available
   mods-enabled
   sites-available
   sites-enabled

Enable and disable configurations/modules/sites
***********************************************

The following commands enable and disable configurations, modules and sites, by adding or removing symlinks in the (conf-, mod-, site-)enabled directory respectively.

.. code-block:: bash

    a2enconf <conf-name>
    a2disconf <conf-name>

    a2enmod <mod-name>
    a2dismod <mod-name>

    a2ensite <site-name>
    a2dissite <site-name>


:code:`apachectl configtest` checks if the configuration files are ok

The above utilities are in :code:`/usr/sbin`

Apache and php-fpm
******************

.. code-block:: bash

    apt install php
    apt install software-properties-common
    wget -q https://packages.sury.org/php/apt.gpg -O- | sudo apt-key add -
    echo "deb https://packages.sury.org/php/ stretch main" | sudo tee /etc/apt/sources.list.d/php.list
    apt update
    apt install php5.6
    apt install php7.3
    apt install php5.6-fpm
    apt install php7.3-fpm
    apt install libapache2-mod-fcgid php-fpm 

To see the default php version

.. code-block:: bash

    php -v

To change the default php version

.. code-block:: bash

    update-alternatives --set php /usr/bin/php5.6

Start the fpm services

.. code-block:: bash

    systemctl start php5.6-fpm
    systemctl start php7.3-fpm

Check they're working

.. code-block:: bash

    systemctl status php5.6-fpm
    systemctl status php7.3-fpm


Enable modules

.. code-block:: bash

    sudo a2enmod actions fcgid alias proxy_fcgi
    systemctl restart apache2

Add the highlighed lines in the site's .conf file. 

.. code-block:: bash
    :emphasize-lines: 14-17

    <VirtualHost *:80>
         ServerAdmin admin@site2.your_domain
         ServerName site2.your_domain
         DocumentRoot /var/www/site2.your_domain
         DirectoryIndex info.php
    
         <Directory /var/www/site2.your_domain>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Order allow,deny
            allow from all
         </Directory>
    
        <FilesMatch \.php$>
          # For Apache version 2.4.10 and above, use SetHandler to run PHP as a fastCGI process server
          SetHandler "proxy:unix:/run/php/php7.2-fpm.sock|fcgi://localhost"
        </FilesMatch>
    
         ErrorLog ${APACHE_LOG_DIR}/site2.your_domain_error.log
         CustomLog ${APACHE_LOG_DIR}/site2.your_domain_access.log combined
    </VirtualHost>

Apache on Windows
#################

Installing Apache on Windows
****************************

Installation 
=============

https://www.sitepoint.com/how-to-install-apache-on-windows/

* Make sure no one is listening on port 80 (IIS)
* Visit www.apachelounge.com/download
* As per the instructions, download Visual Studio C++ 2019 aka VS16 from the link found in the page, (https://aka.ms/vs/16/release/VC_redist.x64.exe) and install
* Download Apache from www.apachelounge.com/download
* Extract the Apache2x directory in the location you want to install the webserver. 

Setting up 
==========

Do the following changes in the :code:`conf/httpd.conf` file.

* (ln 37) Set the directory where the server is installed in :code:`Define SRVROOT`. 
* (ln 60) Set the listening port(s) e.g. :code:`Listen *:80`
* (ln 227) Set the servername e.g. :code:`ServerName localhost:80`
* (ln 162) Enable the rewrite module :code:`LoadModule rewrite_module modules/modrewrite.so`
* (ln 251) Set the default directory out of which all docs are server e.g. :code:`DocumentRoot "D:/WebPages"`
* Set options for the default directory

    * (ln 252) Enter the default directory's name e.g. :code:`<Directory "D:/WebPages">`
    * (ln 265) Change :code:`Options Indexes FollowSymLinks` to :code:`Options FollowSymLinks` to stop the server from listing files.
    * (ln 277) Change to :code:`Require all denied`, so that content is denied by default and is served explicitly in directories and virtual hosts.

Security Considerations
=======================

**Default Deny policy for root**

No one should have access to the server's root folder. That's ensured with the (default) directives found in :code:`httpd.conf`

.. code-block:: 

    <Directory />
       AllowOverride none
       Require all denied
    </Directory>

**AllowOverride None**

Set :code:`AllowOverride None` everywhere in :code:`httpd.conf`, so that :code:`.htaccess` files can't modify the server's behaviour. This can be enabled, where and when needed.

**Disable public access to .htfiles**

Prevent :code:`htaccess` and :code:`.htpasswd` files from being viewed by Web clients.

.. code-block:: 

   <Files ".ht*">
       Require all denied
   </Files>


**ServerSignature Off**

This is a default choice that stops the server from displaying information about it (version, etc).


Testing and starting the server
===============================

* Test the installation by running :code:`./httpd.exe -t` in the :code:`Apache24/bin` directory.
* Start the server locally for testing by running :code:`./httpd.exe`.
* Install as windows service by running :code:`./httpd.exe -k install`. 





Installing a single PHP on windows
**********************************

https://www.sitepoint.com/how-to-install-php-on-windows/

* Download the thread safe php (zip)
* Extract it in e.g. `C:\php`
* Add the directory to the windows' path
* Copy php.ini-development to php.ini and uncomment the following:

    * extension=curl
    * extension=gd
    * extension=mbstring
    * extension=pdo_mysql

* Configure php as an apache module by adding at the end of the 
  apache httpd.conf file the following

.. code-block:: bash

  PHPIniDir "C:/php"
   LoadModule php7_module "C:/php/php7apache2_4.dll"
   Addtype application/x-httpd-php .php

* Check the httpd.conf file

   `Apache24/bin/httpd -t` 

* Restart apache

* Test php (`localhost/index.php`) with `index.php` being

.. code-block:: php

    <?php phpinfo(); ?>



Multiple PHPs on the same windows machine
*****************************************
This tutorial is based on https://www.dionysopoulos.me/apache-mysql-php-server-on-windows-with-multiple-simultaneous-php-versions.html, with big portions of the later copied here.

Setting up PHP
==============
* **Make sure that the correct version of the Microsoft Visual C++ Redistributable for Visual Studio is installed for the version of the PHP that is being installed.**. Next to the php version, you can see the required VC, (e.g. vc11, vc15, etc). Search for the Microsoft Visual C++ Redistributable for Visual Studio with this code and install it. 
* Download the PHP versions you want from http://windows.php.net/download. Choose the Non Thread Safe releases.
* Create a folder to store the PHPs, e.g. :code:`C:\PHP`. 
* Make one folder for each php within there, e.g. :code:`C:\PHP\5.4`, :code:`C:\PHP\7.4` etc
* Inside each PHP folder copy :code:`php.ini-development` to :code:`php.ini` and edit to match your preferences.
* Make the following change

.. code-block:: php

    ; extension_dir="ext"

    to

    extension_dir = "C:\PHP\5.4\ext"

for version 5.4, etc.

* Open the Windows Environment Variables and append the :code:`PATH` variable with the names of the php version folders, e.g. :code:`C:\PHP\5.4;C:\PHP\7.4` etc


* If there is an environment variable called :code:`PHPRC`, php will use the :code:`php.ini` file found in that folder. To use a different :code:`php.ini` file when testing from the command line, export the path to the correct :code:`php.ini` file to the :code:`PHPRC` variable. See also the "Different php.ini files per php version". 

To test that the php versions are working, open a command terminal in each PHP folder and run

.. code-block:: 

    ./php.exe --version

You should get the version number with no errors. 

Setting up Apache
=================
* Install Apache following the instructions in the **Installing Apache on Windows** section, up to the point where we test the installation.
* Download the the :code:`mod_fcgid` module from https://www.apachelounge.com/download/, extract its contents and move the :code:`mod_fcgid.so` file in the :code:`C:\Apache24\modules` folder.
* Edit the :code:`C:\Apache24\conf\extra\httpd-fcgid.conf` (or create it) adding the following lines at the end

.. code-block::

    FcgidInitialEnv PATH "c:/php/5.4;C:/WINDOWS/system32;C:/WINDOWS;C:/WINDOWS/System32/Wbem;"
    FcgidInitialEnv SystemRoot "C:/Windows"
    FcgidInitialEnv SystemDrive "C:"
    FcgidInitialEnv TEMP "C:/WINDOWS/Temp"
    FcgidInitialEnv TMP "C:/WINDOWS/Temp"
    FcgidInitialEnv windir "C:/WINDOWS"
    FcgidIOTimeout 64
    FcgidConnectTimeout 16
    FcgidMaxRequestsPerProcess 1000
    FcgidMaxProcesses 50
    FcgidMaxRequestLen 8131072
    # Location of php.ini
    FcgidInitialEnv PHPRC "c:/php/5.4"
    FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000
    <Files ~ "\.php$">
      AddHandler fcgid-script .php
      FcgidWrapper "c:/php/5.4/php-cgi.exe" .php
      Options +ExecCGI
      Require all granted
    </Files>

The :code:`c:/php/5.4` in  :code:`FcgidInitialEnv PATH`,  :code:`FcgidInitialEnv PHPRC` and  :code:`FcgidWrapper` above, represents the default php version, which can be changed accordingly.

* Uncomment the following in :code:`C:\Apache24\conf\httpd.conf` (removing the leading :code:`#`).

.. code-block::

    # LoadModule include_module modules/mod_include.so
    # LoadModule fcgid_module modules/mod_fcgid.so
    # LoadModule vhost_alias_module modules/mod_vhost_alias.so

* Add the following in :code:`C:\Apache24\conf\httpd.conf` somewhere close to the rest of the :code:`IfModule` s (e.g. after the :code:`<IfModule cgi`).

.. code-block::

    <IfModule fcgid_module>
        Include conf/extra/httpd-fcgid.conf
    </IfModule>

* Now test and start the Apache as indicated in the **Installing Apache on Windows** section.
* Add in the :code:`DocumentRoot` directory defined above, a file named :code:`phpinfo.php` with the content

.. code-block:: php

    <?php phpinfo(); ?>

Visiting http://localhost/phpinfo.php on a browser should show the version of the php used. If not review the steps above.

Virtual Hosts
=============

Our goal is to have every site on our local server run under a different PHP version. This can be done depending:

* On the domain we are using
* On the port we are using
* On the directory the code is in

We start with differentiating the php version based on the domain we're using. 

* Open the :code:`hosts` file (:code:`C:\Windows\System32\drivers\etc\hosts`) and add the following

.. code-block:: php

    127.0.0.1 local.web www.local.web
    127.0.0.1 local56.web www.local56.web
    127.0.0.1 local74.web www.local74.web

    127.0.0.1 foobar.local.web 
    127.0.0.1 foobar.local56.web 
    127.0.0.1 foobar.local74.web 

The last 3 lines will allow the folder :code:`DocumentRoot/foobar` to be accessed using both http://local.web/foobar and http://foobar.local.web

* Edit the :code:`conf/extra/httpd-vhosts.conf` file and change its contents with

.. code-block:: 

    # Default PHP virtual host (local.web)
    <VirtualHost *:80>
     ServerAdmin webmaster@local.web
     DocumentRoot "c:/WebPages"
     ServerName www.local.web
     #ErrorLog "logs/dummy-host.example.com-error.log"
     #CustomLog "logs/dummy-host.example.com-access.log" common
    </VirtualHost>

    # Default PHP Dynamic virtual hosts using vhost_alias
    <VirtualHost *:80>
     ServerAlias *.local.web
     UseCanonicalName Off
     VirtualDocumentRoot "c:/WebPages/%1"
    </VirtualHost>

    # PHP 5.6 virtual host (local56.web)
    <VirtualHost *:80>
     ServerAdmin webmaster@local.web
     DocumentRoot "c:/WebPages"
     ServerName www.local56.web
     #ErrorLog "logs/dummy-host.example.com-error.log"
     #CustomLog "logs/dummy-host.example.com-access.log" common
    	
     <Directory "c:/WebPages">
      <Files ~ "\.php$">
       AddHandler fcgid-script .php
       FcgidWrapper "c:/php/5.6/php-cgi.exe" .php
       Options +ExecCGI
       Require all granted
      </Files>
     </Directory>
    </VirtualHost>
    
    # Dynamic virtual hosts using vhost_alias, PHP 5.6
    <VirtualHost *:80>
     ServerAlias *.local56.web
     UseCanonicalName Off
     VirtualDocumentRoot "c:/WebPages/%1"
    	
     <Directory "c:/WebPages">
      <Files ~ "\.php$">
       AddHandler fcgid-script .php
       FcgidWrapper "c:/php/5.6/php-cgi.exe" .php
       Options +ExecCGI
       Require all granted
      </Files>
     </Directory>
    </VirtualHost>

Accessing http://local.web/phpinfo.php will return the default version of the php, while visiting http://local56.web/phpinfo.php will return version 5.6.


**Changing the php version according to the port** can be achieved by replacing the port :code:`80` with the desired port in the virtual host's definition. In this case don't forget to adjust the Listen directive in line 60 of the :code:`httpd.conf` file accordingly, i.e. by adding the new port.

**Changing the php version according to the directory** can be achieved in 2 ways: 

The first is by adding the appropriate :code:`Directory` directives in the virtual host. E.g. the following

.. code-block:: 

    # Default PHP virtual host (local.web)
    <VirtualHost *:80>
     ServerAdmin webmaster@local.web
     DocumentRoot "c:/WebPages"
     ServerName www.local.web
     #ErrorLog "logs/dummy-host.example.com-error.log"
     #CustomLog "logs/dummy-host.example.com-access.log" common

     <Directory "c:/WebPages/55">
      <Files ~ "\.php$">
       AddHandler fcgid-script .php
       FcgidWrapper "c:/php/5.5/php-cgi.exe" .php
       Options +ExecCGI
       Require all granted
      </Files>
     </Directory>

     <Directory "c:/WebPages/74">
      <Files ~ "\.php$">
       AddHandler fcgid-script .php
       FcgidWrapper "c:/php/7.4/php-cgi.exe" .php
       Options +ExecCGI
       Require all granted
      </Files>
     </Directory>

    </VirtualHost>

will give PHP version 5.5 in folder http://localhost/55 and version 7.4 in folder folder http://localhost/74

The second method is by placing the :code:`Files` directive that's in the :code:`Directory` directive in the :code:`.htaccess` file of each folder. 

Different php.ini files per php version
---------------------------------------

The above setting will result in all phps being initialised with the same :code:`php.ini` file, which will be the one described in :code:`FcgidInitialEnv PHPRC "..."` in the :code:`httpd-fcgid.conf` file.

To use a different :code:`php.ini` file per php version (recommended), each php version has to have its own virtual host. For each virtual host, add the :code:`FcgidInitialEnv PHPRC "..."` directive indicating the location of the respective :code:`php.ini` file, e.g. 

.. code-block:: 

    # PHP 5.6 virtual host (local56.web)
    <VirtualHost *:80>
      FcgidInitialEnv PHPRC "c:/php/5.6/"
      <Files ~ "\.php$">
        AddHandler fcgid-script .php
        FcgidWrapper "c:/php/5.6/php-cgi.exe" .php
        Options +ExecCGI
        Require all granted
      </Files>
    </VirtualHost>
    
    # PHP 5.7 virtual host (local57.web)
    <VirtualHost *:81>
      FcgidInitialEnv PHPRC "c:/php/5.7/"
      <Files ~ "\.php$">
        AddHandler fcgid-script .php
        FcgidWrapper "c:/php/5.7/php-cgi.exe" .php
        Options +ExecCGI
        Require all granted
      </Files>
    </VirtualHost>



Traefik
#######

.. code-block:: yaml

   #Reference: https://docs.docker.com/compose/compose-file/compose-file-v3/
   
   version: "3.9"
   
   networks:
     net-1:
       driver: overlay 
       attachable: true
     ext-web: # a network defined outside this .yml file
       external: true 
   
   services:
   
     traefik:
       command:
         ## API Settings - https://docs.traefik.io/operations/api/, endpoints - https://docs.traefik.io/operations/api/#endpoints ##
         - --api
         - --api.insecure=true # <== Enabling insecure api, NOT RECOMMENDED FOR PRODUCTION
         - --api.dashboard=true # <== Enabling the dashboard to view services, middlewares, routers, etc...
         - --api.debug=true # <== Enabling additional endpoints for debugging and profiling
   
         ## Entrypoints Settings - https://docs.traefik.io/routing/entrypoints/#configuration ##
         - --entrypoints.web.address=:80
         - --entrypoints.web.http.redirections.entryPoint.to=websecure
         - --entrypoints.web.http.redirections.entryPoint.scheme=https
         - --entrypoints.websecure.address=:443
   
         ## Certificate Settings (Let's Encrypt) -  https://docs.traefik.io/https/acme/#configuration-examples ##
         - --certificatesresolvers.mytlschallenge.acme.tlschallenge=true # <== Enable TLS-ALPN-01 to generate and renew ACME certs
         - --certificatesresolvers.le.acme.email=${EMAIL?Variable not set} # <== Setting email for certs
         - --certificatesresolvers.mytlschallenge.acme.storage=/letsencrypt/acme.json # <== Defining acme file to store cert information
   
         ## Log Settings https://docs.traefik.io/observability/logs/ - options: ERROR, DEBUG, PANIC, FATAL, WARN, INFO) ##(
         - --log.level=DEBUG # <== Setting the level of the logs from traefik
         # Enable the access log, with HTTP requests
         - --accesslog
   
         ## Provider Settings - https://docs.traefik.io/providers/docker/#provider-configuration ##
         - --providers.docker=true # <== Enabling docker as the provider for traefik
         - --providers.file.watch=true
         - --providers.docker.network=web # <== Operate on the docker network named web
         - --providers.docker.swarmMode=true # <== Enable Docker Swarm mode
         - --providers.file.filename=/dynamic.yaml # <== Referring to a dynamic configuration file
         - --providers.docker.exposedbydefault=false # <== Don't expose every container to traefik, only expose enabled ones
         - --providers.docker.constraints=Label(`traefik.constraint-label`, `traefik-public`) # <== Add a constraint to only use services with the label "traefik.constraint-label=traefik-public"
   
   
       container_name: traefik
   
       env_file:
         - .env
   
       deploy:
         mode: global
         placement:
           constraints:
             - node.role == manager
             # Make the traefik service run only on the node with this label
             # as the node with it has the volume for the certificates
             - node.labels.traefik-public.traefik-public-certificates == true
   
       environment:
         - TZ=Europe/Berlin
   
       image: traefik:v2.5
   
       labels:
         # Enable traefik on itself to view dashboard and assign subdomain to view it
         - traefik.enable=true 
         # Use the traefik-public network (declared below)
         - traefik.docker.network=traefik-public
         # Use the custom label "traefik.constraint-label=traefik-public"
         # This public Traefik will only use services with this label
         # That way you can add other internal Traefik instances per stack if needed
         - traefik.constraint-label=traefik-public
         # admin-auth middleware with HTTP Basic auth
         # Using the environment variables USERNAME and HASHED_PASSWORD
         - traefik.http.middlewares.admin-auth.basicauth.users=${USERNAME?Variable not set}:${HASHED_PASSWORD?Variable not set}
         # https-redirect middleware to redirect HTTP to HTTPS
         # It can be re-used by other stacks in other Docker Compose files
         - traefik.http.middlewares.https-redirect.redirectscheme.scheme=https
         - traefik.http.middlewares.https-redirect.redirectscheme.permanent=true
         # traefik-http set up only to use the middleware to redirect to https
         # Uses the environment variable DOMAIN
         - traefik.http.routers.traefik-public-http.rule=Host(`${DOMAIN?Variable not set}`)
         - traefik.http.routers.traefik-public-http.entrypoints=http
         - traefik.http.routers.traefik-public-http.middlewares=https-redirect
         # traefik-https the actual router using HTTPS
         # Uses the environment variable DOMAIN
         - traefik.http.routers.traefik-public-https.rule=Host(`${DOMAIN?Variable not set}`)
         - traefik.http.routers.traefik-public-https.entrypoints=https
         - traefik.http.routers.traefik-public-https.tls=true
         # Use the special Traefik service api@internal with the web UI/Dashboard
         - traefik.http.routers.traefik-public-https.service=api@internal
         # Use the "le" (Let's Encrypt) resolver created below
         - traefik.http.routers.traefik-public-https.tls.certresolver=le
         # Enable HTTP Basic auth, using the middleware created above
         - traefik.http.routers.traefik-public-https.middlewares=admin-auth
         # Define the port inside of the Docker service to use
         # Setting the domain for the dashboard
         - traefik.http.routers.api.rule=Host(`monitor.example.com`) 
         # Enabling the api to be a service to access
         - traefik.http.routers.api.service=api@internal 
   
       networks:
         - traefik-public
   
       ports:
         - "80:80" # <== http
         - "8080:8080" # <== :8080 is where the dashboard runs on
         - "443:443" # <== https
   
       restart: always
   
       volumes:
         # Add Docker as a mounted volume, so that Traefik can read the labels of other services
         - /var/run/docker.sock:/var/run/docker.sock:ro
         # Mount the volume to store the certificates
         - traefik-public-certificates:/certificates
   
   
     #### For the app containers
     whoami:
       build: /path/to/DockerFile
       container_name: "simple-service"
       env_file:
         - ./path/to/.envfile
       environment:
         VAR1: var1_value
       image: 
         traefik/whoami
       labels: # Labels set on the container only, not on the service
         - traefik.enable=true # <== Enable traefik to proxy this container
         - traefik.http.routers.web.rule=Host(`example.com`) # <== Your Domain Name goes here for the http rule
         - traefik.http.routers.web.entrypoints=web # <== Defining the entrypoint for http, ref: line 30
         - traefik.http.routers.web.middlewares=redirect@file # <== This is a middleware to redirect to https
         - traefik.http.routers.websecure.rule=Host(`example.com`) # <== Your Domain Name for the https rule 
         - traefik.http.routers.websecure.entrypoints=websecure # <== Defining entrypoint for https, ref: line 31
         - traefik.http.routers.websecure.tls.certresolver=mytlschallenge # <== Defining certsresolvers for https
       networks:
         - web
       restart: always
       volumes:
         - wordpress:/var/www/html
       deploy:
         replicas:
           1
         placement:
           constraints:
             - node.labels.mylabel==true # Will only run on nodes with this label
         labels: # These labels are set only on the service and not on any containers for this service
           - traefik.http.services.service_name.loadbalancer.server.port=8001 # Port via which the traefik load balancer will talk to this container
   
   
   
   networks:
     web:
       external: true # For joining preexisting networks
     backend:
       external: false
   
       
   volumes:
     # Create a volume to store the certificates, there is a constraint to make sure
     # Traefik is always deployed to the same Docker node with the same volume containing
     # the HTTPS certificates
     traefik-public-certificates:
   
   tls:
       certificates:
         - certFile: /data/traefik/certs/wildcard.crt
           keyFile: /data/traefik/certs/wildcard.key
         - certFile: /data/traefik/certs/another-wildcard.crt
           keyFile: /data/traefik/certs/another-wildcard.key
   
       stores:
           default:
           defaultCertificate:
               certFile: /data/traefik/certs/wildcard.crt
               keyFile: /data/traefik/certs/wildcard.key
   
   
   # Differences in deployment with docker-compose (local) and docker stack (distributed)
   # 1) In the 'local' version the labels must be under the <service>, i.e. they should
   #    be attached to the container. In the distributed version, the labels should be 
   #    under the 'deploy:', i.e. attached to the service
   # 2) In the 'local' version the - --providers.docker.swarmMode should be set to false 
   #    or be commented out
   
   
   # Start manually a web-app with its domain name
   docker service create \
   --replicas 15 \
   --name web-app \
   --constraint node.role!=manager \
   --network proxy \
   --label  traefik.enable=true \
   --label 'traefik.http.routers.traefik.rule=Host(`app.doma.in`)' \
   --label  traefik.http.routers.traefik.entrypoints=websecure \
   --label  traefik.http.routers.traefik.tls=true \
   --label  traefik.http.services.hostname.loadbalancer.server.port=80 \
   nginxdemos/hello
   
   # Call address with host
   curl -H Host:whoami.docker.localhost http://127.0.0.1
   
Traefik with Let's encrypt
**************************
To obtain Let's Encrypt SSL certificates with Traefik:

Add the following in the :code:`traefik` service

.. code-block:: yaml

  traefik:
    command:
      ....
      - --certificatesresolvers.acmeresolver.acme.email=<email_address>
      - --certificatesresolvers.acmeresolver.acme.storage=/etc/traefik/acme/acme.json
      - --certificatesresolvers.acmeresolver.acme.httpchallenge.entrypoint=web

This creates a certificate resolver named :code:`acmeresolver`, that is linked to the :code:`web` entrypoint, and stores the certificate in :code:`/etc/traefik/acme/acme.json`. The :code:`web` entrypoint should listen to the port 80, to enable the :code:`httpChallenge`.

You might also want to mount the :code:`/etc/traefik/acme/` directory, so that the certificates are available on the host. This can be done by adding

.. code-block:: yaml

  traefik:
    volumes:
      ....
      - "./path/to/acme/on/host:/etc/traefik/acme"


To allow certificate generation for a specific service, you should add in the CLI mode

.. code-block:: yaml

  <service_name>:
    deploy:
      labels:
        ...
        - traefik.http.routers.<secure-router-name>.tls=true
        - traefik.http.routers.<secure-router-name>.tls.certresolver=acmeresolver

In the YAML mode this can be 

.. code-block:: yaml

    http:
      routers:
        <secure-router-name>:
          ...
          tls:
            certResolver: acmeresolver

Some extra points:

* When testing you should use the staging server by adding in traefik's configuration

.. code-block:: yaml

  traefik:
    command:
      ....
      - --certificatesresolvers.myresolver.acme.caserver=https://acme-v02.api.letsencrypt.org/directory

This line can be removed when you're ready to go to production


* If the :code:`/etc/traefik/acme/acme.json` is mounted on the host and you are experiencing problems with certificate generation, deleting this file and restarting Traefik can help.

General OpenSSL Commands
########################

https://www.sslshopper.com/article-most-common-openssl-commands.html

These commands allow you to generate CSRs, Certificates, Private Keys and do other miscellaneous tasks.

Generate a new private key and Certificate Signing Request

.. code-block:: bash

    openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privateKey.key

Generate a self-signed certificate 

.. code-block:: bash

    openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt

Generate a certificate signing request (CSR) for an existing private key

.. code-block:: bash

    openssl req -out CSR.csr -key privateKey.key -new

Generate a certificate signing request based on an existing certificate

.. code-block:: bash

    openssl x509 -x509toreq -in certificate.crt -out CSR.csr -signkey privateKey.key

Remove a passphrase from a private key

.. code-block:: bash

    openssl rsa -in privateKey.pem -out newPrivateKey.pem



Check a Certificate Signing Request (CSR)

.. code-block:: bash

    openssl req -text -noout -verify -in CSR.csr

Check a private key

.. code-block:: bash

    openssl rsa -in privateKey.key -check

Check a certificate

.. code-block:: bash

    openssl x509 -in certificate.crt -text -noout


Redirection Rules
#################
RewriteRule Pattern Substitution

The Pattern is the :code:`REQUEST_URI`. if the RewriteRule is in a virtual host, it includes the leading forward slash. If it's in a :code:`.htaccess` file, it does not.

:code:`HTTP_HOST` and :code:`REQUEST_URI` examples

.. code-block:: bash

   http://%{HTTP_HOST}%{REQUEST_URI}
   http://test.com/path/to/resource
   %{HTTP_HOST} =  test.com
   %{REQUEST_URI} = /path/to/resource

If the 'Substitution' starts with :code:`http:// or https://`, then the redirection happens at this exact literal. Otherwise the 'Substitution' is appended to the original path. Examples

.. code-block:: bash

   RewriteRule ^(.*)$ https://test.com
   Redirects to https://test.com
   
   RewriteRule ^(.*)$ somethingelse
   Redirects to http(s)://original_host/original_uri/somethingelse



:code:`%{REQUEST_FILENAME}` is the full path on the server of the file that was requested e.g.
for :code:`http://a.com/hello.txt` the :code:`%{REQUEST_FILENAME}` is :code:`/path/in/the/server/to/hello.txt`


