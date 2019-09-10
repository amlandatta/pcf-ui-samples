# Using `staticfile_buildpack`
Using staticfile_buildpack and app manifest to deploy applications

__manifest.yml__

```
---
applications:
- name: ui-app-a
  instances: ((instances)) # No. of instances after deployment. At least configure 2 in production for HA
  memory: ((memory)) # memory limit
  disk_quota: ((disk_quota)) # disk space for your app instance
  buildpacks:
  - ((buildpack-static))
  routes: # configure if custom routes are planned
  - route: ui-app-a.((domain))
  #services:
   #- instance_ABC
  health-check-type: http #
  health-check-http-endpoint: /health.html
  timeout: ((timeout))
  env:
    SKIP_SSL_VALIDATION: ((SKIP_SSL_VALIDATION))
```

__Deploy app__

> * Use templates
> * Create var files per environment

```
cd pcf-sample-static
cf push --vars-file vars-dev.yml
```

To understand folder structure deployed with above configuration
__Check files inside the container__

```
cf ssh ui-app-a

vcap@d774f8a2-c11f-438e-69fb-1c0d:~/app/public$ ll
total 4
drwx------ 3 vcap vcap 41 Sep  1 02:08 ./
drwxr-xr-x 1 vcap vcap 72 Sep  1 02:08 ../
-rw-r--r-- 1 vcap vcap  3 Sep  1 01:52 health.html
drwxr-xr-x 2 vcap vcap 24 Sep  1 01:50 ui-app-a/
```
View default `nginx.conf` with `staticfile_buildpack`

```
cat nginx/conf/nginx.conf

worker_processes 1;
daemon off;

error_log /home/vcap/app/nginx/logs/error.log;
events { worker_connections 1024; }

http {
  charset utf-8;
  log_format cloudfoundry '$http_x_forwarded_for - $http_referer - [$time_local] "$request" $status $body_bytes_sent';
  access_log /home/vcap/app/nginx/logs/access.log cloudfoundry;
  default_type application/octet-stream;
  include mime.types;
  sendfile on;

  gzip on;
  gzip_disable "msie6";
  gzip_comp_level 6;
  gzip_min_length 1100;
  gzip_buffers 16 8k;
  gzip_proxied any;
  gunzip on;
  gzip_static always;
  gzip_types text/plain text/css text/js text/xml text/javascript application/javascript application/x-javascript application/json application/xml application/xml+rss;
  gzip_vary on;

  tcp_nopush on;
  keepalive_timeout 30;
  port_in_redirect off; # Ensure that redirects don't include the internal container PORT - 8080
  server_tokens off;

  server {
    listen 8080;
    server_name localhost;

    root /home/vcap/app/public;

    location / {
        index index.html index.htm Default.htm;
    }

    location ~ /\. {
      deny all;
      return 404;
    }

  }
}
```

* For static_buildpack configuration. Refer: https://docs.cloudfoundry.org/buildpacks/staticfile/index.html

* Update `Staticfile`, to configure nginx.conf as below.
Refer [here](https://docs.cloudfoundry.org/buildpacks/staticfile/index.html) for further configurations

```
server {
  listen 8080;
  server_name localhost;

  root /home/vcap/app/public;

  set $updated_host $host;
  if ($http_x_forwarded_host != "") {
    set $updated_host $http_x_forwarded_host;
  }

  if ($http_x_forwarded_proto != "https") {
    return 301 https://$updated_host$request_uri;
  }

  location / {
      index index.html index.htm Default.htm;

      add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";

      error_page 403 /error/403.html;
      error_page 404 /error/404.html;
      error_page 500 /error/500.html;
  }
```
