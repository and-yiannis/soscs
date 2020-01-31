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
