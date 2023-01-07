# A brief introduction to AntOSDK
An AntOS application project is recognized by AntOSDK via a special project file called  `project.json` stored on top of the project folder. Basically, this file is in the following format:

```json
{
	"name": "Project name",
	"root": "home://project/path",
	"css": ["css/main.css"],
	"javascripts": ["js/lib.js"],
	"coffees": ["coffees/main.coffee"],
	"copies": ["assets/scheme.html", "package.json", "README.md"]
}
```

This meta-data allows the SDK to gain knowledge on the project structure and its different kinds of file. The following diagram sums up the building process of an AntOS application project using AntOSDK:

[[@book:image:/c_3/s_1/antosdk-dia.svg]]

Project files are processed differently based on their kind. Basically, project files are classed into the following categories:
* CSS files: for application GUI styling, all CSS files are combined and minified into one finale file: **main.css**
* Coffeescript files: application code written in Coffeescript. A Coffeescript file are combined to a single file that will the be compiled to javascript thanks to the integrated Coffeescript to Javascript compiler.
* Javascript files: application code written in javascript or application dependency javascript libraries. All javascript files are combined and minified (including the javascript code compiled from coffeescript) to a single: **main.js** file.
* Asset files such as scheme file, image, icon, etc. are copied directly to the final built directory
* Application meta-data: *package.json* contain application meta-data required by AntOS API to manage and run application. This file is copied to the final build directory


As shown in the diagram, AntOSDK is able to build and run AntOS application project directly from the browser. Integrating the SDK into the **CodePad** application offers a powerful in-browser development environment for AntOS applications.
The SDK is also able to package the built application to a single ZIP archive for final distribution. The ZIP package can be installed locally or be published on a repository that can be accessed from the default system application **MarketPlace**.

## AntOS package

Typical AntOS package (ZIP archive) contains the following files:
* **main.js**: all-in-one compiled application code (javascript)
* **main.css**: Application style sheet
* **scheme.html**: Application UI scheme defined in AFXML (AntOS Markup Language)
* **package.json**: Package meta data
* Any other application dependency files

The following figure shows the content of the **Booklet** package as an example (the package is open using the **Archive** application, available on MarketPlace):

[[@book:image:/c_3/s_0/pkg-archive.jpg]]

### Package meta-data

Package meta-data file contains all the information necessary by the AntOS API to manage (install, uninstall, update, etc.) and run an application. Its has the following format:

```json
{
    "app":"ApplicationClassName",
	"services": ["Service1ClassName", "Service2ClassName"],
	"pkgname": "PackageName"
    "name":"Application name",
    "description":"Application description",
    "info":{
        "author": "Author fullname",
        "email": "Author email"
    },
    "version":"0.0.4-a",
    "category":"Other",
    "iconclass":"fa fa-adn",
    "mimes":["text/.*"]
}
```

The following meta-data properties are important:
* **app**: The main class name of the appliacation.
* **pkgname**: The package name.
* **services**: If the application comes with services, all services main class names should be listed in this property.
* **version**: in format: **MAJOR.MINOR.PATCH-branch** (`branches are: a (alpha), b (beta), r (release)`), the system uses this version number to determine whether the application update is available on the MarketPlace
* **mimes**: specifies all file mime types that the application can handle