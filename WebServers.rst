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

https://www.sitepoint.com/how-to-install-apache-on-windows/

* Make sure none is listening on port 80 (IIS)
* Download Microsoft Visual C++2015 Redistributable from https://www.microsoft.com/en-us/download/details.aspx?id=48145
* Download Apache from www.apachelounge.com/download
* Extract the Apache2x directory in the location you want to install the webserver. (:code:`C:\Apache24`)
* Edit the conf/httpd.conf file as follows:

    * (ln 37) Set the server root in `Define SRVROOT`
    * (ln 60) listen to all requests on port 80 `Listen *:80`
    * (ln 162) `LoadModule rewrite_module modules/modrewrite.so`
    * (ln 227) `ServerName localhost:80`
    * (ln 224) `AllowOverride All` for allowing `.htaccess` overrides.
    * (ln 251) Set the root `DocumentRoot "D:/WebPages"`
    * (ln 252) Set the root `<Directory "D:/WebPages">`
* Test the installation by running :code:`./httpd.exe -t` in the :code:`Apache24/bin` directory.
* Install as windows service by running :code:`./httpd.exe -k install` in the :code:`Apache24/bin` directory.


https://www.sitepoint.com/how-to-install-php-on-windows/
********************************************************

* Download the thread save php (zip)
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
* Download the PHP versions you want from http://windows.php.net/download. Choose the Non Thread Safe releases.
* Create a folder to store the PHPs, e.g. :code:`C:\PHP`. 
* Make one folder for each php within there, e.g. :code:`C:\PHP\5.4`, :code:`C:\PHP\7.4` etc
* Inside each PHP folder copy :code:`php.ini-development` to :code:`php.ini` and edit to match your preferences.
* Make the following change

.. code-block:: php

    ; extension_dir="ext"
    ; to
    extension_dir = "C:\PHP\5.4\ext"

for version 5.4, etc.

* Open the Windows Environment Variables and append the :code:`PATH` variable with the names of the php version folders, e.g. :code:`C:\PHP\5.4;C:\PHP\7.4` etc

To test that the php versions are working, open a command terminal in each PHP folder and run

.. code-block:: 

    ./php.exe --version

You should get the version number with no errors. 

Setting up Apache
=================
* Install Apache following the instructions in the **Installing Apache on Windows** section, up to the point where we test the installation.
* Download the the :code:`mod_fcgid` module from https://www.apachelounge.com/download/, extract its contents and move the :code:`mod_fcgid.so` file in the :code:`C:\Apache24\modules` folder.
* Edit the :code:`C:\Apache24\conf\extra\httpd-default.conf` adding the following lines at the end

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

* Edit the :code:`C:\Apache24\conf\httpd.conf` removing the leading :code:`#`.

.. code-block::

    # LoadModule include_module modules/mod_include.so
    # LoadModule fcgid_module modules/mod_fcgid.so
    # LoadModule vhost_alias_module modules/mod_vhost_alias.so
    ...
    # Include conf/extra/httpd-default.conf

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
