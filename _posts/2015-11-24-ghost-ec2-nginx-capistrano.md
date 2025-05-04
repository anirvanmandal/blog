---
layout: post
title: Ghost Deployment on EC2 with nginx, capistrano and PM2
date: '2015-11-24T12:50:50.000Z'
author: Anirvan Mandal
categories: [Ghost, Nginx, Deployment]
tags: ghost nginx capistrano
---

* You need to create an [AWS](http://aws.amazon.com/) EC2 server (Ubuntu 14.04) with add an EBS.
* Make sure you have port 80 opened in the inbound section of the security groups.
* Follow [instructions](https://www.thefanclub.co.za/how-to/how-secure-ubuntu-1204-lts-server-part-1-basics) to make your server secure. (optional)
* I have mounted my EBS at /mnt/data and I am using /mnt/data/blog/current as the root directory for the ghost application.
* Install nginx

```bash
sudo apt-get install nginx
```

* Create cache directories

```bash
sudo mkdir /var/cache/nginx
sudo chown www-data:www-data /var/cache/nginx
```

* Here's my nginx config. It's pretty barebones just to make stuff work. You can copy it as it is or make more advanced changes to it.

```sh
sudo vim /etc/nginx/nginx.conf
```

```bash
user www-data;
worker_processes 4;
pid /run/nginx.pid;

events {
	worker_connections 768;
	# multi_accept on;
}

http {
	sendfile on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	include mime.types;
	default_type application/octet-stream;

	proxy_cache_path  /var/cache/nginx levels=1:2 keys_zone=one:8m max_size=3000m inactive=600m;
	proxy_temp_path /var/tmp;

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	gzip on;

	gzip_vary on;
	gzip_proxied any;
	gzip_comp_level 6;
	gzip_buffers 16 8k;
	gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
	gzip_min_length 1000;

	upstream ghost_upstream {
		server 127.0.0.1:2368;
		keepalive 64;
	}

	server {
		listen 80;
		server_name code.anirvan.me;

		location / {
			root '/mnt/data/blog/current';
			proxy_redirect off;
			proxy_set_header   X-Real-IP            $remote_addr;
			proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
			proxy_set_header   X-Forwarded-Proto $scheme;
			proxy_set_header   Host                   $http_host;
			proxy_set_header   X-NginX-Proxy    true;
			proxy_set_header   Connection "";
			proxy_http_version 1.1;
			proxy_cache one;
			proxy_cache_key ghost$request_uri$scheme;
			proxy_pass         http://ghost_upstream;
		}

	}

	include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}
```


* Reload nginx

```bash
sudo /etc/init.d/nginx restart
```

* Install [node version manager](https://github.com/creationix/nvm) on the server and install node v0.10.42.

* Install [pm2](https://github.com/Unitech/pm2) globally on the node server

* Install [ghost](https://ghost.org/download/) to your local machine. Follow the instructions [here](http://support.ghost.org/installation/).

* Initiate a git repo

```bash
git init
```

Make sure your you ignore the files not required

```bash
content/images/*
content/data/*
node_modules/
```

* Add [capistrano](http://capistranorb.com/) to your project to help with deployments.

I am using ruby 2.1.5 using rvm in my local environment

The Gemfile looks like this

```ruby
ruby '2.1.5'

source 'https://rubygems.org'

gem 'bundler'
gem 'capistrano'
gem 'capistrano-npm'
```

Initialize capistrano

```bash
cap install
```

Only the production environment was of relevance for me, so I deleted config/deploy/staging.rb

The config/deploy/production.rb should looks like this

```ruby
server "YOUR SERVER IP", user: "ubuntu", roles: %{web}

set :scm, :git
set :branch, "master"
set :application, "YOUR_APPLICATION_NAME"
set :repo_url, "YOUR_GIT_REPO"

set :deploy_to, "/mnt/data/blog"

set :user, 'ubuntu'

set :app_command, 'start.sh'
set :process_name, 'node_server_code_blog'
```

The config/deploy.rb looks like this

```ruby
set :keep_releases, 5

namespace :deploy do

  after :publishing, :install_packages do
    on roles(:web) do
       within current_path do
         execute :npm, :install, '--production'
       end
    end
  end

  after :install_packages, :restart do
       invoke 'pm2:restart'
  end

end
```

Add a file /lib/tasks/pm2.cap. This basically takes care of monitoring your node application.

```ruby
require 'json'

namespace :pm2 do

  def app_status
    within current_path do
      ps = JSON.parse(capture :pm2, :jlist, fetch(:process_name))
      if ps.empty?
        return nil
      else
        return ps[0]["pm2_env"]["status"]
      end
    end
  end

  def restart_app
    within current_path do
      execute :pm2, :restart, fetch(:process_name)
    end
  end

  def start_app
    within current_path do
      execute :pm2, :start, fetch(:app_command) --name=fetch(:process_name)
    end
  end

  desc 'Restart app gracefully'
  task :restart do
    on roles(:web) do
      case app_status
      when nil
        info 'The application is not registered'
        start_app
      when 'stopped'
        info 'The application is not running'
        restart_app
      when 'errored'
        info 'The application is in an erroneous state'
        restart_app
      when 'online'
        info 'The application is already running'
        restart_app
      end
    end
  end

end
```

* Add capistrano-npm and pm2.cap to the Capfile

```ruby
...
require 'capistrano-npm'
import 'lib/tasks/pm2.cap'
...
```

Add a start.sh script to your project root

```bash
#!/bin/bash
cd /mnt/data/blog/current
npm start --production
```

* Make sure your latest changes are present in the master branch of your git repository

* Deploy from your local environment

```bash
cap production deploy
```

* If there are no errors your ghost blog should be available at the public ip of your EC2 instance
