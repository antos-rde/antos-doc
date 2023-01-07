# Application development with AntOSDK
AntOS provides an abstract API for application development. The core API contains three main elements: the **UI API**, the **VFS API** and the **VDB API**, as shown in the following graph:

![](https://os.lxsang.me/VFS/shared/4aa149ac0a861354a04b939c0c19ddea7bab2e9f)

The **UI API** defines the basic UI elements such as Window, List, Tree, Dialogs, etc,  and provides a generic interface for application and dialog UI development. UI design consists two steps: (1) the first step is to layout UI elements using antOS' scheme syntax (in XML format); (2) the second step is to handle user interaction using the coffeescript/ Javascript API.

The **VFS API** defines an abstract and generic file system interface. On one hand, it specifies an unique and independent virtual file system interface to AntOS applications and on the other hand, it allows to adapt different kinds of file system to AntOS VFS interface. This mechanism decouples the applications from underneath file system, thus applications can work flexibly with any file systems that has a compatible AntOS VFS interface adapter. For example, an AntOS application can access Google Drive  and Drobox files as the same way as the local files.

The **VDB API** has the same principle as the **VFS API**, but it applies on the relational database accessing. **VDB API** is useful for applications that require to store more complex date structure (than files) on the server. On the application side, different kinds of database can be accessed using an unique interface. On the low-level API side, an adapter interface is available for different database sources.

AntOSDK eases the application development process with AntOS API by providing tools necessary for developing, in browser testing and packaging applications. The SDK is not a standalone application or nor a library, it is a part of the **CodePad**  application in form of an extension. AntOS applications, therefore, can be developed, compiled, tested and packaged directly by CodePad