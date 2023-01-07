# System locale
The system locale consists of all translatable texts from the core API, core GUI and all the default applications.

Currently the system supports only three locales: *enGB, frFR, viVN*, but you can add your own language to the project by following these steps:
1. Clone the project from github repository
2. At the top folder execute the command : `echo <your locale name> | make genlang `. For example for English `echo en_GB | make genlang`. This command will scan all available texts on the API and the packages, then generates a locale file in **src/core/languages** (e.g. en_GB.json for the previous command).
3. Modify the generated file by providing the translations to the texts
4. Re build the project with the newly created JSON locale file

At application-level, an application can be translated separately to different languages by providing different locales in the application meta-data, the next section will show this in detail.