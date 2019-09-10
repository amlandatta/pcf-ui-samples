# Configuring to deploy static UI applications

Cloud Foundry bundles [System Buildpacks](https://docs.cloudfoundry.org/buildpacks/system-buildpacks.html) which provides runtime support for apps.

For static contents following out-of-box buildpacks are recommended:
* [Staticfile Buildpack](https://github.com/cloudfoundry/staticfile-buildpack)
* [NGINX Buildpack](https://github.com/cloudfoundry/nginx-buildpack)

Both options configures NGINX to serve static content. Static file buildpack is simpler to configure but limits what can be configured. NGINX buildpack allows to implement custom nginx.conf but enforces developer to follow a specific folder structure.

Sample implementation of each buildpack:

* [Example using staticfile_buildpack](https://github.com/adtechworks/pcf-ui-samples/tree/master/pcf-sample-static)
* [Example using nginx_buildpack](https://github.com/adtechworks/pcf-ui-samples/tree/master/pcf-sample-nginx)


> Note: If requirement is to use custom `nginx.conf` then it will be recommended to use nginx_buildpack. Overriding nginx.conf is deprecated in staticbuildpack , as it breaks the functionality of the Staticfile and Staticfile.auth configuration directives.
