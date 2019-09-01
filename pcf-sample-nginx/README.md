# pcf-sample-nginx
Using `nginx_buildpack` and app manifest to deploy applications

Refer [here](https://github.com/cloudfoundry/nginx-buildpack/tree/master/fixtures/mainline) for folder-structure to use nginx-buildpack

__Deploy app__
* Use var files for environment specific configurations

```
cd pcf-sample-static
cf push --vars-file vars-dev.yml
```

__Check files inside the container__

```
cf ssh ui-app-a

vcap@4f9c0abb-5bb3-474b-687d-5102:~/app$ ll
total 20
drwxr-xr-x 1 vcap vcap  184 Sep  1 07:25 ./
drwx------ 1 vcap vcap   93 Sep  1 07:25 ../
-rw-r--r-- 1 vcap vcap   31 Sep  1 07:15 buildpack.yml
drwx------ 2 vcap vcap    6 Sep  1 07:25 client_body_temp/
drwx------ 2 vcap vcap    6 Sep  1 07:25 fastcgi_temp/
drwxr-xr-x 2 vcap vcap   23 Sep  1 07:25 logs/
-rw-r--r-- 1 vcap vcap 2080 Sep  1 07:16 mime.types
-rw-r--r-- 1 vcap vcap  538 Sep  1 07:25 nginx.conf
drwx------ 2 vcap vcap    6 Sep  1 07:25 proxy_temp/
drwxr-xr-x 3 vcap vcap   41 Sep  1 07:23 public/
-rw-r--r-- 1 vcap vcap  190 Sep  1 07:17 vars.yml

vcap@4f9c0abb-5bb3-474b-687d-5102:~/app/public$ ll
total 4
drwxr-xr-x 3 vcap vcap  41 Sep  1 07:23 ./
drwxr-xr-x 1 vcap vcap 184 Sep  1 07:25 ../
-rw-r--r-- 1 vcap vcap   3 Sep  1 07:19 health.html
drwxr-xr-x 2 vcap vcap  24 Sep  1 07:23 ui-app-a/
```
