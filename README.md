# shopware 6 config
Quick start with shopware 6, to install and deploy it on a server in a short space of time :rocket:

Simple past all those commands in your terminal the first time you access your server.

``` sh
apt update; \
apt install -y php-common php-cli php-fpm php-mysql php; \
service apache2 stop; \
rm -rf /etc/apache2; \
apt install -y nginx ;\
apt-get install unzip ;\
ufw allow 'Nginx HTTP' ;\
> /etc/nginx/sites-enabled/default ;\
nano /etc/nginx/sites-enabled/default
```

Paste that and make sure to change both [SERVER_IP] and [PHP_VERSION]

```
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	# SSL configuration
	#
	# listen 443 ssl default_server;
	# listen [::]:443 ssl default_server;
	#
	# Note: You should disable gzip for SSL traffic.
	# See: https://bugs.debian.org/773332
	#
	# Read up on ssl_ciphers to ensure a secure configuration.
	# See: https://bugs.debian.org/765782
	#
	# Self signed certs generated by the ssl-cert package
	# Don't use them in a production server!
	#
	# include snippets/snakeoil.conf;

	root /var/www/html/public;

	# Add index.php to the list if you are using PHP

	server_name [SERVER_IP];

    index shopware.php index.php;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

	# pass PHP scripts to FastCGI server
	#
	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
	#
	#	# With php-fpm (or other unix sockets):
		fastcgi_pass unix:/var/run/php/php[PHP_VERSION]-fpm.sock;
	#	# With php-cgi (or other tcp sockets):
#		fastcgi_pass 127.0.0.1:9000;
	}

	# deny access to .htaccess files, if Apache's document root
	# concurs with nginx's one
	#
    location /recovery/install {
      index index.php;
      try_files $uri /recovery/install/index.php$is_args$args;
    }
	location ~ /\.ht {
		deny all;
	}
}


# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#	listen 80;
#	listen [::]:80;
#
#	server_name example.com;
#
#	root /var/www/example.com;
#	index index.html;
#
#	location / {
#		try_files $uri $uri/ =404;
#	}
```


Save and paste the other commands

``` sh
cd /var/www/html/ ;\
rm -rf * ;\
wget -O shopware.zip https://releases.shopware.com/sw6/install_6.0.0_ea1_1563354247.zip  ;\
unzip shopware.zip ;\
rm shopware.zip ;\
chmod 777 . ;\
chmod 777 var/logs/ ;\
chmod 777 var/cache/ ;\
chmod 777 var/queue/ ;\
chmod 777 public/ ;\
chmod 777 config/jwt/ ;\
chmod 777 public/recovery/install/data ;\
apt-get -y install php-xml php-curl php-mbstring php-zip php-intl ;\
systemctl reload nginx 
```

Start to use your shop :dollar:
