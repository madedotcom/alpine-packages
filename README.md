# MADE.COM Public Alpine Linux (APK) packages.

This repo is used in combination with https://github.com/madedotcom/alpine-jazz-hands for automagic building of Alpine Linux Packages.

If you have no idea what you are doing, have a look here:
http://git.alpinelinux.org/cgit/aports/tree/main/

## How to use MADE.COM packages and public repository.
*I assume you are sensible enough to be using alpine.*

Update your `Dockerfile` adding this:

```
ADD https://s3-eu-west-1.amazonaws.com/madecom-alpine-repo/main/ops%40made.com-5677cf76.rsa.pub /etc/apk/keys/abuild.rsa.pub
RUN chmod -R 644 /etc/apk/keys/
RUN echo "@made http://madecom-alpine-repo.s3-website-eu-west-1.amazonaws.com/main" >> /etc/apk/repositories
```

Your final `Dockerfile` might look like this:

```
FROM alpine:3.3
ENV TERM=xterm
ENV PROJECT_NAME="portus"
ADD https://s3-eu-west-1.amazonaws.com/madecom-alpine-repo/main/ops%40made.com-5677cf76.rsa.pub /etc/apk/keys/abuild.rsa.pub
RUN chmod -R 644 /etc/apk/keys/
RUN echo "@made http://madecom-alpine-repo.s3-website-eu-west-1.amazonaws.com/main" >> /etc/apk/repositories
RUN adduser -S $PROJECT_NAME && addgroup -S $PROJECT_NAME && addgroup $PROJECT_NAME $PROJECT_NAME
RUN apk update && apk add nginx ca-certificates pongo-blender@made
ADD nginx.conf.tmpl mime.types site.conf.tmpl /etc/pongo-blender/
ADD ssl_crt.crt ssl_key.key /etc/nginx/
ADD entrypoint.sh /usr/bin/entrypoint.sh
ADD tools.sh /sbin/tools
RUN chmod +x /sbin/tools
```

The @made tells apk to use made's repo for that particular package.



