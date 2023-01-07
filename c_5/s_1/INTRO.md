# Application locale

To help the system translate an application with texts that are not available in the system locale dictionary, developer should manually add translation to all translatable texts in the application to the application meta-data.

For example, the application **wTerm** provide translation to French with the following meta-data

```json
{
    "app":"wTerm",
    "name":"Terminal",
    "description":"Access Unix terminal from web",
    "info":{
        "author": "...",
        "email": "..."
    },
    "version":"0.0.5-a",
    "category":"System",
    "iconclass":"fa fa-terminal",
    "mimes":["none"],
    "locales":{
        "fr_FR": {
            "Open terminal": "Ouvrir terminal",
            "Edit": "Modifier",
            "Please enter terminal URI": "Merci d'entrer l'URI du terminal",
            "Unable to connect to: {0}": "Impossible de connecter a: {0}",
            "Terminal connection closed": "Connexion terminal est fermee"
        }
    }
}
```

All translatable texts need to be put in the `locale` property of the application meta-data.

When the translation is triggered by the UI update event, the translation API will first try to query the application meta-data to translate a text to the target locale. If the translation of such text is not available in the application meta-data, the API will secondly fallback to the system locale dictionary.