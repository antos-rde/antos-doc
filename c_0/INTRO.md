# Build and installation
The entire AntOS VDE (Virtual Desktop Environment) system consists of different components:

1. The client side API called AntOS which requires to connect to the server side REST-based API
2. The Server side API which is a REST Based API, It can be developed by any server side
(scripting) language and framework as long as the API respects the specification required by the client API.
The specification will be detailed lately in this documentation.
3. A web server that hosts and runs the server side API.

## Running your own AntOS VDE system using docker image

As an example of the entire working AntOS VDE system, a minimal docker image is available at [https://hub.docker.com/r/xsangle/antosaio](https://hub.docker.com/r/xsangle/antosaio)
which includes all necessary components (also developed by the author):

1. AntOS API
2. AntOS server side REST based API developed in lua, which is a part of this project [https://github.com/lxsang/antd-web-apps](https://github.com/lxsang/antd-web-apps) 
3. The Antd web-server [https://github.com/lxsang/ant-http](https://github.com/lxsang/ant-http) and its plug-ins

The easiest way to install and test the entire VDE system is to use this docker image.
The following manual requires docker to be installed on the host system.

First, we need to create the application moutpoint that will  be mounted to the container. The server configuration will be stored in this location, and accessible by the container. The mountpoint is also where user data is permanently stored, separated from the container.

For this example, the mount point is just simply a directory with the following structure:
```sh
mkdir -p /tmp/app/{home,tmp,database}
```

Second, create the server configuration in this directory

```sh
# create the server configuration
cat << EOF > /tmp/app/antd-config.ini
[SERVER]
plugins=/opt/www/lib/
plugins_ext=.so
database=/app/data/database/
tmpdir=/app/data/tmp/
maxcon=200
backlog=5000
workers = 2
max_upload_size = 10000000
gzip_enable = 1
gzip_types = text\/.*,.*\/css,.*\/json,.*\/javascript

[PORT:80]
htdocs=/opt/www/htdocs
ssl.enable=0
^/(.*)$ = /os/router.lua?r=<1>&<query>

[AUTOSTART]
plugin = tunnel

[MIMES]
image/bmp=bmp
image/jpeg=jpg,jpeg
text/css=css
text/markdown=md
text/csv=csv
application/pdf=pdf
image/gif=gif
text/html=html,htm,chtml
application/json=json
application/javascript=js
image/png=png
image/x-portable-pixmap=ppm
application/x-rar-compressed=rar
image/tiff=tiff
application/x-tar=tar
text/plain=txt
application/x-font-ttf=ttf
application/xhtml+xml=xhtml
application/xml=xml
application/zip=zip
image/svg+xml=svg
application/vnd.ms-fontobject=eot
application/x-font-woff=woff,woff2
application/x-font-otf=otf
audio/mpeg=mp3,mpeg

[FILEHANDLER]
lua = lua
EOF
```

Lastly, use the following command to setup AntOS user, mount the application data mountpoint, create and run a dedicated container for the user **demo**

```sh
docker run \
    -p 8080:80 \
    --mount type=bind,source=/tmp/app,target=/app \
    -e ANTOS_USER=demo \
    -e ANTOS_PASSWORD=demo \
    -it xsangle/antosaio:latest
```

From the host browser, the VDE can be accessed with user and password: demo/demo via

```
http://<host-ip>:8080
```

From the AntOS VDE, applications more applications can be installed using the MarketPlace application.

### Customize the image

The Docker layer for building the image is freely available at: [https://github.com/lxsang/antosaio](https://github.com/lxsang/antosaio).

The build support support multi-arch build with docker **buildx** plugin. Instruction to install buildx can be found here: [ https://github.com/docker/buildx#installing
](https://github.com/docker/buildx#installing
).

The following architectures are supported: **armv7**, **amd64** and **arm64**.

After installing the plug-in, execute the following commands before running the build script:

```sh
docker buildx create --use
# Register Arm executables to run on x64 machines
docker run --rm --privileged docker/binfmt:a7996909642ee92942dcd6cff44b9b95f08dad64 
```

Clone the repository, modify the Dockerfile to suit your own need and run the build script:

```sh
git clone --depth 1 https://github.com/lxsang/antosaio
cd antosaio
chmod +x bake.sh
./bake.sh
```

***The remainder of this chapter targets  those who want to use only the front-end  API and want to develop their own back-end API that
respects the AntOS REST-based specification.***
