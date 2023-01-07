# AntOS REST-based API Specification

The front end API is developed using the specification detailed in this chapter and can connect to any server side API that respects this specification.
The following global conventions should be taken in to account in any server side implementation:

* All requests issued by the front-end API to the REST based API are called **system calls**
* Server side API must provide the exact URI resources specified in this document
* Before performing any system call (except login), the API must check whether the user is authenticated to perform such call
* All system calls except file download are performed as **application/json** requests, data exchange in JSON format
* All JSON responses issued by the back-end API should in the following format
```json
{
	"error": ERROR_MESSAGE_STRING | false,
	"result": ANY_JSON_OBJECT
}
```
* IF there is error when performing a system call, the `error` property should be set by the error string, the `result` property can be ignored
*  Otherwise, the `error` property is alway set as `false` and the `result` property  contains the result of the call

There are mainly 4 categories of system calls which will be detailed in 4 separated sections in this chapter:
* **System**: perform system-wide operation such as package manager, load and save user setting, custom application specific API gateway
* **User**: operations related to user manager such as authentication, login, logout etc.
* **Virtual File System (VFS)**: all operations related to the VFS APIs
* **Virtual Database**: All operation related to the VDB API