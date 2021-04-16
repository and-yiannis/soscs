########################
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


Apache 
#######
Apache can be used as a reverse proxy a load balancer and a web server

A web server is a software with a primary function to store, process and deliver web pages to clients.

Useful tutorials: https://httpd.apache.org/docs/2.4/howto/

Installation
************

Install on ubuntu

.. code-block:: bash

   sudo apt-get update && sudo apt-get upgrade
   sudo apt-get install apache2
   service apache2 status

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


Apache Web Server
#################

Can be used as a

* Proxy server
* Load Balancer
* Web server

Install and start


In Debian

.. code-block:: bash

   apt install apache2
   systemctl status apache2.service

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

:code:`a2ensite` enables a site. :code:`a2dissite` disables it.
:code:`a2enmod` enables a module. :code:`a2dismod` disables it.
:code:`apachectl configtest` checks if the configuration files are ok

The above utilities are in :code:`/usr/sbin`

Apache and php-fpm

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

https://www.sitepoint.com/how-to-install-apache-on-windows/
***********************************************************

* Make sure none is listening on port 80 (IIS)
* Download Microsoft Visual C++2015 Redistributable
    https://www.microsoft.com/en-us/download/details.aspx?id=48145
* Download Apache
    www.apachelounge.com/download
* Extract the Apache2x directory in the location you want to install the webserver. (C:\Apache24)
* in the conf/httpd.conf file:
    * (ln 37) Set the server root in `Define SRVROOT`
    * (ln 60) listen to all requests on port 80 `Listen *:80`
    * (ln 162) `LoadModule rewrite_module modules/modrewrite.so`
    * (ln 227) `ServerName localhost:80`
    * (ln 224) `AllowOverride All` for allowing `.htaccess` overrides.
    * (ln 251) Set the root `DocumentRoot "D:/WebPages"`
    * (ln 252) Set the root `<Directory "D:/WebPages">`
* Test the installation
    * Run in `Apache24/bin` directory
        `httpd -t`
* Install as windows service 
    * Run in `Apache24/bin` directory
        `httpd -k install` 

https://www.sitepoint.com/how-to-install-php-on-windows/
********************************************************

Download the thread save php (zip)
Extract it in e.g. `C:\php`
Add the directory to the windows' path
Copy php.ini-development to php.ini and uncomment the following:
    extension=curl
    extension=gd
    extension=mbstring
    extension=pdo_mysql
* Configure php as an apache module by adding at the end of the 
  apache httpd.conf file the following
  `PHPIniDir "C:/php"
   LoadModule php7_module "C:/php/php7apache2_4.dll"
   Addtype application/x-httpd-php .php`
* Check the httpd.conf file
   `Apache24/bin/httpd -t` 
* Restart apache
* Test php (`localhost/index.php`) with `index.php` being
    `<?php
    phpinfo();
    ?>`


    * 

* Open the conf/httpd.conf
