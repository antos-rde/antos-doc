# Virtual File System (VFS)
Table of content:
1. [Directory scanning API](#h2_2)
2. [File Information API](#h2_3)
3. [File moving API](#h2_4)
4. [Directory creation API](#h2_5)
5. [File sharing API](#h2_6)
6. [File downloading API](#h2_7)
7. [File deleting API](#h2_8)
8. [File writing API](#h2_9)

-----

## Base URI
All URI to the system API should start with `/VFS`
### Request

```
GET /VFS HTTP/1.1
```

### Response

The base request return the VFS API description:

```json
{
	"error": false,
	"result": {
		"description": "This api handle file operations",
		"actions": {
			"/scandir": "List all files and folders",
			"/fileinfo": "Get file information",
			"/get": "Get a file content",
			"/move": "Move file to a new destination",
			"/shared": "Get shared file content",
			"/mkdir": "Create directory",
			"/publish": "Share a file to all users",
			"/delete": "Delete a file",
			"/write": "Write data to file"
		}
	}
}
```
## 1. Directory scanning API
### Request

```json
POST /VFS/scandir HTTP/1.1
Content-Type: application/json

{
    "path": "VFS_PATH"
}

```

`VFS_PATH` is the VFS path to the directory to be scanned

### Response

The meta-data of all files in the requested directory, an example with the requested directory ```home://```:

```json
{
	"result": [{
			"path": "home://workspace",
			"perm": {
				"other": {
					"exec": true,
					"write": false,
					"read": true
				},
				"owner": {
					"exec": true,
					"write": true,
					"read": true
				},
				"group": {
					"exec": true,
					"write": true,
					"read": true
				}
			},
			"mime": "dir",
			"type": "dir",
			"uid": 1002.0,
			"mtime": "Tue, 23 Jun 2020 14:04:31 GMT",
			"filename": "workspace",
			"size": 4096.0,
			"gid": 1002.0,
			"permissions": " (775)",
			"ctime": "Tue, 23 Jun 2020 14:04:31 GMT"
		}, {
			"path": "home://README.md",
			"perm": {
				"other": {
					"exec": false,
					"write": false,
					"read": true
				},
				"owner": {
					"exec": false,
					"write": true,
					"read": true
				},
				"group": {
					"exec": false,
					"write": true,
					"read": true
				}
			},
			"mime": "text/markdown",
			"type": "file",
			"uid": 1002.0,
			"mtime": "Sun, 18 Feb 2018 22:21:01 GMT",
			"filename": "README.md",
			"size": 38.0,
			"gid": 1002.0,
			"permissions": " (664)",
			"ctime": "Sun, 18 Feb 2018 22:21:01 GMT"
		},
		{
			"path": "home://.bashrc",
			"perm": {
				"other": {
					"exec": false,
					"write": false,
					"read": true
				},
				"owner": {
					"exec": false,
					"write": true,
					"read": true
				},
				"group": {
					"exec": false,
					"write": false,
					"read": true
				}
			},
			"mime": "application/octet-stream",
			"type": "file",
			"uid": 1002.0,
			"mtime": "Sun, 18 Feb 2018 22:02:39 GMT",
			"filename": ".bashrc",
			"size": 3771.0,
			"gid": 1002.0,
			"permissions": " (644)",
			"ctime": "Sun, 18 Feb 2018 22:02:39 GMT"
		}
	],
	"error": false
}
```

## 2. File Information API
Get the VFS file meta-data
### Request
Example

```json
POST /VFS/fileinfo HTTP/1.1
Content-Type: application/json

{
    "path": "os://packages/Syslog/main.js"
}
```

### Response

Return the meta-data of the requested file, for example:

```json
{
	"result": {
		"permissions": " (644)",
		"perm": {
			"other": {
				"write": false,
				"read": true,
				"exec": false
			},
			"owner": {
				"write": true,
				"read": true,
				"exec": false
			},
			"group": {
				"write": false,
				"read": true,
				"exec": false
			}
		},
		"path": "os://packages/Syslog/main.js",
		"ctime": "Thu, 06 Aug 2020 21:19:32 GMT",
		"mime": "application/javascript",
		"name": "main.js",
		"size": 5938.0,
		"type": "file",
		"mtime": "Thu, 06 Aug 2020 21:19:32 GMT",
		"gid": 0.0,
		"uid": 0.0
	},
	"error": false
}
```

## 3.  File moving API
Move file/folder to another location
### Request
Example request that move a folder from `"home://doc/TestBook` to `"home:///TestBook`

```json
POST /VFS/move HTTP/1.1
Content-Type: application/json

{
    "src": "home://doc/TestBook",
    "dest": "home:///TestBook"
}
```

### Response
A result object that indicate whether the operation successes, for example

```json
{
	"error": false,
	"result": true
}
```

or 

```json
{
	"error": "Unable to move file",
	"result": false
}
```

## 4. Directory creation API

Create new directory

### Request

```json
POST /VFS/mkdir HTTP/1.1
Content-Type: application/json

{
    "path": "home:///test"
}
```

### Response

A result object that indicate whether the operation successes, for example

```json
{
	"error": false,
	"result": true
}
```

or 

```json
{
	"error": "Unable to create directory",
	"result": false
}
```

## 5. File sharing API

File sharing API allows to bypass the AntOS user authentication system.
Once published, every one that has the shared URI can access to this file.

This API allow to share/unshare a file

### Request

```json
POST /VFS/publish HTTP/1.1
Content-Type: application/json

{
    "path": "VFS_FILE",
    "publish": true/false
}
```

### Response

In case of successfully sharing/unsharing the file, the response return the share ID of the file, for example:

```json
{
	"result": "5d864f98899b972159e0818beef6b8f3a2907e16",
	"error": false
}
```

## 6. File downloading API

Get the content of an VFS file

### Request

```
GET /VFS/get/VFS_FILE_PATH HTTP/1.1
Content-Type: FILE_CONTENT_TYPE
```

for example:

```
GET /VFS/get/home://test.svg HTTP/1.1
Content-Type: image/svg+xml
```

### Response
The content of the file in octet-stream


## 7. File deleting API

### Request
Example request that delete `"home://doc/TestBook`

```json
POST /VFS/delete HTTP/1.1
Content-Type: application/json

{
    "path": "home://TestBook"
}
```

### Response
A result object that indicate whether the operation successes, for example

```json
{
	"error": false,
	"result": true
}
```

or 

```json
{
	"error": "Unable to delete file",
	"result": false
}
```

## 8. File writing API

Write data to the file, the file data should be encoded to Base64 by the client, in the following format

```
data:<MIME_TYPE>;base64,<BASE64_DATA>
```

### Request

```json
POST /VFS/write HTTP/1.1
Content-Type: application/json

{
    "path": "VFS_PATH",
    "data": "ENCODED_FILE_DATA"
}
```

For example:

```json
POST /VFS/write HTTP/1.1
Content-Type: application/json

{
    "path": "home://README.md",
    "data": "data:text/plain;base64,VGhpcyBpcyB3aGVyZSBpIHdvcmsuIE15IHByaXZhdGUgc3BhY2Uu"
}
```


### Response
A result object that indicate whether the operation successes, for example

```json
{
	"error": false,
	"result": true
}
```

or 

```json
{
	"error": "Unable to write data to file file",
	"result": false
}
```