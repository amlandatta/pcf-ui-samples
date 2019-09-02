# pcf-ui-samples
Best practises in configuring and deploying static UI application to Cloud Foundry using application manifest.

* [Example using staticfile_buildpack](https://github.com/adtechworks/pcf-ui-samples/tree/master/pcf-sample-static)

* [Example using nginx_buildpack](https://github.com/adtechworks/pcf-ui-samples/tree/master/pcf-sample-nginx)


If requirement is to use custom `nginx.conf` then it will be recommended to use nginx_buildpack.
Note: overriding nginx.conf is deprecated in staticbuildpack , as it breaks the functionality of the Staticfile and Staticfile.auth configuration directives.
