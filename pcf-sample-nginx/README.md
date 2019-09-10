# Using `nginx_buildpack`
Using nginx_buildpack and app manifest to deploy applications

Refer [here](https://github.com/cloudfoundry/nginx-buildpack/tree/master/fixtures/mainline) for folder-structure to use nginx-buildpack

```
├── buildpack.yml (Required)
├── manifest.yml
├── mime.types (Required)
├── nginx.conf (Required)
├── public (Required)
│   ├── error
│   │   ├── 403.html
│   │   ├── 404.html
│   │   └── 500.html
│   ├── health.html
│   └── ui-app-a
│       └── index.html
└── vars-dev.yml
```

__Deploy app__

```
cd pcf-sample-static
cf push --vars-file vars-dev.yml
```
> * Use templates
> * Create var files per environment

To understand folder structure deployed with above configuration
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
