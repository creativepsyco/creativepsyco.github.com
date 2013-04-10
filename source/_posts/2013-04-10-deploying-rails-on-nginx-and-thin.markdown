---
layout: post
title: "Deploying Rails on Nginx and thin"
date: 2013-04-10 15:17
comments: true
categories: ["Nginx", "Rails"]
---
I have been busy lately doing my Final Year Project which comprises of building a web based service for processing analysis jobs on patient data.

I have developed the web service API using [Rails][rails] with MySQL being the backend database. 

For deployment purposes I had no idea which of the web servers to run as such I naturally googled online and found people talking about [Capistrano][caps], a great way to have deployment scripts being run on the server.

I needed something simple, I had seen the advantages of using SVN and [Nginx][ngix] as a means to deploy by SSH-ing to the server. This keeps things simple and lends us as much control as we need. So I explored.
<!--more-->

Here are instructions to setup a fresh copy of Ubuntu Precise Pangolin with the necessary goodies:

###RVM
I highly recommend RVM to manage rubies and gems

+ Install the pre-reqs
```bash
sudo apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion pkg-config
curl -L https://get.rvm.io | bash -s stable
exec $SHELL
rvm install 1.9.3 --default
ruby -v
```
+ Don't require rdoc and ri when installing gems:
```bash
echo "gem: --no-ri --no-rdoc" > ~/.gemrc
```
+ install the latest Rails (3.2.11):
```bash
gem i bundler rails
```

###Thin
Thin is a ruby server much like the default WebRIck that comes along packaged with the default version of rails.

* Install thin and make it have some sensible defaults 

```bash
gem install thin
thin install
/usr/sbin/update-rc.d -f thin defaults # this updates the service to automatically start at login
```

* Now assuming we have a rails app inisde the folder structure `/var/www/myapp.example.com` we now start the deployment process:

* Create a configuration
	
```bash
thin config -C /etc/thin/myapp.example.com -c /var/www/myapp.example.com --servers 3 -e development # or: -e production for caching, etc
```
###Nginx
Nginx is a Web Server and has better performance than Apache. 

* Install Nginx

```bash
sudo apt-get install nginx
```

* Create a vHost. To do this, edit `/etc/nginx/sites-available/myapp.example.com`

This is a sample Nginx configuration for `myapp`

```nginx
	upstream myapp {
	  server 127.0.0.1:3000;
	  server 127.0.0.1:3001;
	  server 127.0.0.1:3002;
	}
	server {
	  listen   80;
	  server_name .example.com;

	  access_log /var/www/myapp.example.com/log/access.log;
	  error_log  /var/www/myapp.example.com/log/error.log;
	  root     /var/www/myapp.example.com;
	  index    index.html;

	  location / {
	    proxy_set_header  X-Real-IP  $remote_addr;
	    proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
	    proxy_set_header  Host $http_host;
	    proxy_redirect  off;
	    try_files /system/maintenance.html $uri $uri/index.html $uri.html @ruby;
	  }

	  location @ruby {
	    proxy_pass http://myapp;
	  }
	}
```
And make it enabled as well:

```bash
ln -nfs /etc/nginx/sites-available/myapp.example.com /etc/nginx/sites-enabled/myapp.example.com
```
###Switch the app Live

- Restart the different daemons
```bash
/etc/init.d/thin restart
/etc/init.d/nginx reload
/etc/init.d/nginx restart
```

Now go visit `http://myapp` and it should show you the Rails page that you are supposed to see. Easy right?

[rails]: http://rubyonrails.org/
[caps]: https://github.com/capistrano/capistrano
[nginx]: http://nginx.com/