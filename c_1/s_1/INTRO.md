# User API
Table of content:

1. [Login API](#h2_2)
2. [Logout API](#h2_3)
3. [User information API](#h2_4)

-----

## Base URI
All URI to the user API should start with `/user`.
This API performs three main basic operations:
* Get user information
* Login
* Logout

### Request

```
GET /user HTTP/1.1
```

### Response

This operation return the API description in JSON format as follow:

```json
{
	"result": {
		"description": "This api handle the user authentification",
		"actions": {
			"/auth": "Return user information if a user is alreay logged in",
			"/login": "Perform a login operation",
			"/logout": "Perform a logout operation"
		}
	},
	"error": false
}
```

## 1. Login API

Perform the login operation before opening a user session. It is expected that the user credential using to log in is an existing system user on the server side (usually linux OS user).

### Request

```json
POST /user/login HTTP/1.1
Content-Type: application/json

{
	"username": "username",
	"password": "password"
}
```

### Response

An error JSON object should be returned on operation error.
On a success login:
* The server side API should open a new user session and store a session id on the client side (e.g. cookie). This session ID should help the server side API verifying user authentication before performing any server side operation after login.
* The server side API look for the user setting in `home://.setting.json`, if the file is not found, it will be created with default setting. The API returns the content of this file as an JSON object, an example response:

```json
{
	"error": false,
	"result": {
		"applications": {
			"MarketPlace": [],
			"CodePad": [],
			"Syslog": [],
			"About": [],
			"Files": {
				"showhidden": false,
				"nav": true,
				"sidebar": true
			},
			"Setting": []
		},
		"desktop": {
			"menu": [],
			"path": "home://.desktop",
			"showhidden": false
		},
		"user": {
			"username": "demo",
			"groups": {
				"0": "root",
				"27": "sudo",
				"110": "lxd",
				"1002": "demo"
			},
			"name": "demo",
			"id": 1002
		},
		"appearance": {
			"theme": "antos_dark",
			"wp": {
				"repeat": "repeat",
				"url": "os://resources/themes/system/wp/wp3.jpg",
				"size": "cover"
			},
			"wps": [],
			"themes": [{
					"text": "AntOS light",
					"name": "antos_light",
					"selected": false
				},
				{
					"text": "AntOS dark",
					"name": "antos_dark",
					"selected": true
				}
			]
		},
		"VFS": {
			"mountpoints": [{
					"iconclass": "fa  fa-adn",
					"path": "app://",
					"type": "app",
					"text": "__(Applications)"
				},
				{
					"iconclass": "fa fa-home",
					"path": "home://",
					"selected": true,
					"type": "fs",
					"text": "__(Home)"
				},
				{
					"iconclass": "fa fa-desktop",
					"path": "home://.desktop",
					"selected": false,
					"type": "fs",
					"text": "__(Desktop)"
				},
				{
					"iconclass": "fa fa-inbox",
					"path": "os://",
					"selected": false,
					"type": "fs",
					"text": "__(OS)"
				},
				{
					"iconclass": "fa fa-share-square",
					"path": "shared://",
					"type": "fs",
					"text": "__(Shared)"
				}
			]
		},
		"system": {
			"packages": {
				"MarketPlace": {
					"locales": [],
					"version": "0.0.1-a",
					"iconclass": "fa fa-adn",
					"category": "System",
					"path": "os://packages/MarketPlace",
					"mimes": [
						"none"
					],
					"description": "Application store",
					"name": "Application store",
					"app": "MarketPlace",
					"info": {
						"email": "xsang.le@gmail.com",
						"author": "Xuan Sang LE"
					},
					"text": "Application store",
					"filename": "MarketPlace",
					"type": "app",
					"mime": "antos/app"
				},
				"Syslog": {
					"version": "0.0.1-a",
					"iconclass": "fa fa-bug",
					"category": "System",
					"pkgname": "Syslog",
					"mimes": [],
					"path": "os://packages/Syslog",
					"app": "Syslog",
					"description": "Core services and system log",
					"name": "System log",
					"services": [
						"Calendar",
						"PushNotification"
					],
					"info": {
						"licences": "GPLv3",
						"credit": "dedicated to some one here",
						"email": "xsang.le@gmail.com",
						"author": "Xuan Sang LE"
					},
					"text": "System log",
					"filename": "Syslog",
					"type": "app",
					"mime": "antos/app"
				},
				"Files": {
					"locales": [],
					"version": "0.0.3-a",
					"iconclass": "fa fa-hdd-o",
					"category": "System",
					"path": "os://packages/Files",
					"mimes": [
						"dir"
					],
					"description": "System files manager",
					"name": "Files manager",
					"app": "Files",
					"info": {
						"email": "xsang.le@gmail.com",
						"author": "Xuan Sang LE"
					},
					"text": "Files manager",
					"filename": "Files",
					"type": "app",
					"mime": "antos/app"
				},
				"CodePad": {
					"version": "0.0.2-a",
					"iconclass": "fa fa-pencil-square-o",
					"category": "Developments",
					"path": "os://packages/CodePad",
					"mimes": [
						"text/.*",
						"[^/]*/json.*",
						"[^/]*/.*ml",
						"[^/]*/javascript",
						"dir"
					],
					"description": "Code editor",
					"name": "Code",
					"app": "CodePad",
					"info": {
						"licences": "GPLv3",
						"email": "xsang.le@gmail.com",
						"author": "Xuan Sang LE"
					},
					"text": "Code",
					"filename": "CodePad",
					"type": "app",
					"mime": "antos/app"
				},
				"Setting": {
					"version": "0.0.1-a",
					"iconclass": "fa fa-wrench",
					"category": "System",
					"path": "os://packages/Setting",
					"mimes": [
						"none"
					],
					"description": "System setting",
					"name": "System setting",
					"app": "Setting",
					"info": {
						"email": "xsang.le@gmail.com",
						"author": "Xuan Sang LE"
					},
					"text": "System setting",
					"filename": "Setting",
					"type": "app",
					"mime": "antos/app",
					"selected": false
				}
			},
			"pkgpaths": {
				"system": "os://packages",
				"user": "home://.packages"
			},
			"repositories": [{
				"text": "AntOS Github Repository",
				"selected": false,
				"url": "https://raw.githubusercontent.com/lxsang/antosdk-apps/master/packages.json"
			}],
			"menu": [],
			"startup": {
				"services": [
					"Syslog/PushNotification",
					"Syslog/Calendar"
				],
				"apps": []
			},
			"locale": "en_GB",
			"error_report": "https://os.iohub.dev/report"
		}
	}
}
```

More information on the setting object can be found in the **System API**

## 2. Logout API

Perform a log out operation to close a user session.
### Request

```json
POST /user/logout HTTP/1.1
Content-Type: application/json

```

### Response

On success the response should be:

```
{"result":true,"error":false}
```

The server side API should close the current user session, remove session id stored on client side.

## 3. User information API

This operation allows to check whether the user is logged in and get back the user system setting.

### Request


```json
POST /user/auth HTTP/1.1
Content-Type: application/json

```

### Response

If user is logged in, then return the JSON user setting object as detailed in the [Login API](#1loginapi), otherwise return an error object with error message

