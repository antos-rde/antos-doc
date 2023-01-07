# Build the front-end API
AntOS front-end API is developed from the ground, it does not use any existing frameworks, the only runtime dependency is
JQuery.

The core AntOS front-end API is developed using Typescript, therefore, the following compile-time dependencies should be installed:

* `Typescript` compiler
* `JEST` (optional for Unit Test)
* `teaser` for code minifying
* `uglifycss` for CSS minifying
* `@types/jquery`
* `typedoc` to generate the API documentation from source code (optional)

Unlike traditional node-js based project, AntOS using **Make** as its build system. The choice relies only on the author preference.
Thus, It requires that **make** is installed on the system. To build the project:

```bash
git clone https://github.com/lxsang/antos
cd antos
# install all compile-time dependencies
# globally
npm install terser -g
npm install uglifycss -g
npm install typescript -g
# this is optional, for unit tests
npm install jest @types/jest ts-jest -g

#locally
npm install @types/jquery
# this is optional for documentation generation
npm install typedoc

# configure build destination
export BUILDDIR = /path/to/build/location

# optional
# configure API documentation destination
export DOCDIR = /path/to/api/doc

# to generate prettified javascript API (usually for debugging)
make

# to generate minified javascript API (usually for final release)
make release

# to generate the API documentation
make doc

# to perform unit tests
make test
```

Upon performing the `make` or `make release` command, the output of the build will be something similar to
the following structure:

```text
/path/to/build/location
|----- index.html
|----- packages
|   |----- CodePad
|   |   |----- ...
|   |----- Files
|   |   |----- ...
|   |----- MarketPlace
|   |   |----- ...
|   |----- ...
|   |----- Syslog
|       |-----...
|----- resources
|   |----- languages
|   |   |----- en_GB.json
|   |   |----- fr_FR.json
|   |   |----- vi_VN.json
|   |----- themes
|       |----- antos_dark
|       |   |----- antos_dark.css
|       |----- antos_light
|       |   |----- antos_light.css
|       |----- system
|           |-----...
|----- scripts
    |
    |----- antos.js
    |----- jquery-3.2.1.min.js
    |----- ...

20 directories, 479 files
```

Explanation
* The core API and its dependencies (as well as some external libraries) are store in **scripts**.
* The entry point of the system is `index.html`.
* Localization data is defined in **resources/languages**, see more in the [localization section](https://doc.iohub.dev/antos/Ym9vazovLy9jXzUvSU5UUk8ubWQ/Localization_and_language.md)
* System themes defined in **resources/themes**, see more in the [theming section](https://doc.iohub.dev/antos/Ym9vazovLy9jXzQvc18xL0lOVFJPLm1k/UI_theming.md)
* Default system applications are stored in **packages**, see more in the [application section](https://doc.iohub.dev/antos/Ym9vazovLy9jXzIvSU5UUk8ubWQ/Default_Applications.md)

The builds output a complete working  front-end system ready to be deployed.
The next section will discuss on some conventions that should be made by any back-end API design to connect to
AntOS front-end API.