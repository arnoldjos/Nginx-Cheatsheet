# NGINX Custom Configuration

### Update Packages

```
apt-get updates
```

### Download Nginx Package

```
[Get Latest version from](http://nginx.org/en/download.html)
wget http://nginx.org/download/nginx-1.15.3.tar.gz
```

### Extract Nginx Package

```
tar -zxvf ***nginx.tar***
```

### Install Compiler

```
apt-get install build-essential
```

### Install Dependencies

```
apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
```

### Customize Nginx

```
./configure --sbin-path=/usr/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --pid-path=/var/run/nginx.pid --with-http_ssl_module
```

### Compile Configuration Source

```
make

Install Compiled source

make install
```

# Nginx Dynamic content

### For Php

```
apt-get install php-fpm
```

# Add Nginx as Systemd Service

## Minimum requirements of Ubuntu 15.0.4 or CENTOS 7.0.0

### Create Service File

```
nano /lib/systemd/system/nginx.service

copy and paste this script

[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/bin/nginx -t
ExecStart=/usr/bin/nginx
ExecReload=/usr/bin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target

check if it's running successfully
systemctl status nginx
```

### Enable Startup on Boot

```
systemctl enable nginx
```

# NGINX nginx.conf Directives

### Location Blocks

```
	=  exact match				- First Priority
	^~ preferential match	- Second Priority
	~* regex match				- Third Priority
		 Prefix match       - Last Priority



	Preferential Prefix match - case sensitive
	location ^~ /greet2 {
		return 200 'Hello from NGINX';
	}

	Exact match - case sensitive
	location = /greet {
		return 200 'Hello from NGINX - EXACT MATCH';
	}

	REGEX match - case sensitive
	location ~ /greet[0-9] {
		return 200 'Hello from NGINX - REGEX MATCH';
	}

	REGEX match - case insensitive
	location ~* /greet[0-9] {
		return 200 'Hello from NGINX - REGEX MATCH CASE INSENSITIVE';
	}
```

### Variables

```
	Setting Variables
	set $variable value;
```

### Rewrites and Redirects

```
	Redirects
	return - takes a status code and string response.
	return 200 'sample';

	if the return takes a 300 response variant the second parameter can be a uri.
	return 307 /sample/path;

	Rewrites
	rewrite - the first parameter is the uri to be rewritten and the second is the rewritten uri.
	rewrite /original /rewritten

	You can capture a uri segment by enclosing the segment in parenthesis () and access it by $1 for the first captured parameter, $2 for the second and so on.
	rewrite ^/user/(\w+) /greet/$1;

	Last flag tells nginx to make the current rewirte the last rewrite of the uri.
	rewrite ^/user/(\w+) /greet/$1 last;
	rewrite ^/user/(\w+) /thumb.png;
```

### Try Files

```
	Can be used in the server or location context.

	try_files check for a resource to respond with in any number of locations relative to the root directory with a final argument results in a rewrite.

	try_files path1 path2 final;
```
