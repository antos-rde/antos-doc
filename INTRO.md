# AntOS v1.0.0-a

**This version 1.0.0a removes the dependencies on Riot.js by reimplementing the major API for GUI and Announcement system. The entire core API is also rewritten in TypeScript**

AntOS is a front-end API that mimics the traditional desktop environment on the web browser. The front-end can connect to a remote server and acts as a virtual desktop environment (VDE). The original purpose of AntOS is to provide visual tools to access and control resource on remote server
and embedded linux environment. With its application API and the provided SDK, AntOS facilitates the
development and deployment of user specific applications.

This repository contains only the front-end API. To have fully functional VDE, AntOS need to connect
to the corresponding server side API.

![https://os.lxsang.me/VFS/shared/d4645d65b3e4bb348f1bde0d42598ad9b99367f5](https://os.lxsang.me/VFS/shared/d4645d65b3e4bb348f1bde0d42598ad9b99367f5)


## Demo
A demo of the VDE is available at my page  [https://app.iohub.dev/antos/](https://app.iohub.dev/antos/) using username: demo and password: demo.

If one wants to run AntOS VDE locally in their system, a docker image (~24Mb) is available at:
[https://github.com/lxsang/antosaio](https://github.com/lxsang/antosaio)

## API Documentation

- API documentation: [https://doc.iohub.dev/antos/api/](https://doc.iohub.dev/antos/api/)

## AntOS applications
[https://github.com/lxsang/antosdk-apps](https://github.com/lxsang/antosdk-apps)

## Licence

Copyright 2017-2020 Xuan Sang LE <xsang.le AT gmail DOT com>

AnTOS is is licensed under the GNU General Public License v3.0, see the LICENCE file for more information

 This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

   This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

   You should have received a copy of the GNU General Public License
    along with this program.  If not, see <https://www.gnu.org/licenses/>.