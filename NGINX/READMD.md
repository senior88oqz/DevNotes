# NGINX

## Installation

## Configurate virtual hosts

- [ ] `cd /etc/nginx/sites-available`
- [ ] `cp default montebello.lexxtechnologies.com ; cp default schneider.lexxtechnologies.com`

Now we have two _Virtual Hosts_ (one per domain, or several if they all forward to the same port) having a configuration like this:

```init
server {
	listen 80;
	listen [::]:80;
	server_name schneider.lexxtechnologies.com;
	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		# try_files $uri $uri/ =404;
		# proxy_set_header Host $host;
		proxy_pass http://localhost:9988;
	}
}
```

- [ ] `sudo systemctl restart nginx` (restart nginx web server)

## Enable Https

### Install SSL certificate via [certbot](https://certbot.eff.org/)

```bash
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository universe
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx
## After installation
$ sudo certbot --nginx
```

**NOTE: Remember to redirect HTTP to HTPPS**

## Checklist

- [ ] Install [pageSpeed module](https://www.modpagespeed.com/doc/configuration)
- [ ] Setup Reverse proxy
- [ ] Install ssl credentials
- [ ] enable gzip compression, as in `server-configs-nginx/nginx.conf`
- [ ] Check [gzip compression](https://checkgzipcompression.com) and perform [loadtest](https://loadtestertool.com)

```init
...
http{
...
  # Enable gzip compression.
  include h5bp/web_performance/compression.conf;
}
...
```

- [ ] Add pagespeed module and other module that improves site performance

## Important Nginx Files and Directories

### Content

`/var/www/html`: The actual web content, which by default only consists of the default Nginx page you saw earlier, is served out of the `/var/www/html` directory. This can be changed by altering Nginx configuration files.

### Server Configuration

By default, the configuration file is named nginx.conf and placed in the directory `/usr/local/nginx/conf`, `/etc/nginx`, or `/usr/local/etc/nginx`.

**NOTE: When adding new modules/recompling nginx, be careful about the installation path; pass configuration settings from `nginx -V` output; when asking whether to use existing binary file, choose `NO`**

`/etc/nginx`: The Nginx configuration directory. All of the Nginx configuration files reside here.

`/etc/nginx/nginx.conf`: The main Nginx configuration file. This can be modified to make changes to the Nginx global configuration.

`/etc/nginx/sites-available/`: The directory where per-site "server blocks" can be stored. Nginx will not use the configuration files found in this directory unless they are linked to the sites-enabled directory (see below). Typically, all server block configuration is done in this directory, and then enabled by linking to the other directory.

`/etc/nginx/sites-enabled/`: The directory where enabled per-site "server blocks" are stored. Typically, these are created by linking to configuration files found in the sites-available directory.

`/etc/nginx/snippets`: This directory contains configuration fragments that can be included elsewhere in the Nginx configuration. Potentially repeatable configuration segments are good candidates for refactoring into snippets.

### Server Logs

`/var/log/nginx/access.log`: Every request to your web server is recorded in this log file unless Nginx is configured to do otherwise.

`/var/log/nginx/error.log`: Any Nginx errors will be recorded in this log.

## Cheatsheet

To stop your web server, you can type:

```bash
sudo systemctl stop nginx
```

To start the web server when it is stopped, type:

```bash
sudo systemctl start nginx
```

To stop and then start the service again, type:

```bash
sudo systemctl restart nginx
```

If you are simply making configuration changes, Nginx can often reload without dropping connections. To do this, this command can be used:

```bash
sudo systemctl reload nginx
```

By default, Nginx is configured to start automatically when the server boots. If this is not what you want, you can disable this behavior by typing:

```bash
sudo systemctl disable nginx
```

To re-enable the service to start up at boot, you can type:

```bash
sudo systemctl enable nginx
```

To check on `nginx.conf` settings

```bash
sudo service nginx configtest
```

Starting, Stopping, and Reloading Configuration

```bash
nginx -s signal
```

Where signal may be one of the following:

```bash
stop — fast shutdown
quit — graceful shutdown
reload — reloading the configuration file
reopen — reopening the log files
```

Listing nginx processes

```bash
ps -ax | grep nginx
```

## Debugging

1. To troubleshoot why CSS is not minified, you can use the standalone binary css_minify_main. It's shipped with mod_pagespeed, so [build it from source](https://www.modpagespeed.com/doc/build_mod_pagespeed_from_source#debug-css) and then run:

```bash
  ./out/Release/css_minify_main FILENAME.css > REWRITTEN.css
```

1. HTTPS resources can currently only be rewritten if they are origin-mapped or loaded from directly from the file-system. See [HTTPS support](https://www.modpagespeed.com/doc/https_support)

## Refrence

1. [Compiling Third-Party Modules Into Nginx](https://serversforhackers.com/c/compiling-third-party-modules-into-nginx)
2. [PageSpeed Configuration](https://www.modpagespeed.com/doc/configuration)
3. [Check Gzip Compression](https://checkgzipcompression.com/)
4. [Route 53 Record Set on Different Port](https://stackoverflow.com/questions/19349287/route-53-record-set-on-different-port)
5. [What is Reverse Proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)
6. [How to Install an SSL Certificate](https://sucuri.net/guides/how-to-install-ssl-certificate)
7. [Redirect HTTP Requests to HTTPS with NGINX](https://bjornjohansen.no/redirect-to-https-with-nginx)
8. [JavaScript Start-up Optimization](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/javascript-startup-optimization/)
9. [Build NGINX with PageSpeed From Source](https://www.linode.com/docs/web-servers/nginx/build-nginx-with-pagespeed-from-source/)
10. [Pagespeed FAQ](https://www.modpagespeed.com/doc/faq#not-rewriting)