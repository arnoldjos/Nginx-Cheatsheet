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
