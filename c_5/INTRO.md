# Localization and language
Basically, when the web desktop boots up, it loads the configured system locale file from server as a dictionary. This file is in JSON format which contains all the translations of UI' translatable texts for the target locale. 

User operations on the web desktop (Application opening, tab changing, language changing, etc.) will obviously cause an update on the UI system. This latter will trigger the render action on the UI engine which performs many rendering tasks including text rendering. The text rendering, with the help of the localization API, in turn translates all texts available  on the UI (Button, label, etc.) to the target locale before displaying to the end user. 

The translation involves querying the loaded system locale dictionary or the application meta-data `locale` property.

![](https://os.lxsang.me/VFS/shared/3981f4b156592c3f5a6f14af8b33f9694f8eb47a)

### *So, how does the system know which text should be translated?*

By using the localization API. Usually, there are two kinds of text which need to be translated in an application:
* **Static texts**: texts embedded in the UI element. This kind of texts can be found in the scheme file (HTML-like syntax) when designing the UI
* **Dynamic texts**: texts generated when user performs an action (e.g. opening a dialog, pushing a notification, etc.). The texts can be found in the Javascript/coffee code, which implements the application features. Dynamic texts are often mixed between the text that needs to be translated and the context data.

#### Static texts

To handle **static text** AntOS API introduces the directive `__(..)`, all texts inside this directive will be automatically translated to target locale when rendering. This directive can be used directly in the UI scheme or in application code (Javascript), for example, for the UI scheme:
```html
<!-- the text of this label will not be translated when rendering -->
<afx-label text="About"></afx-label>
<!-- the text of this label will be translated when rendering -->
<afx-label text="__(About)"></afx-label>
```
For application script:
```javascript
// The text 'Confirm delete' wont be translated
this.openDialog(
	"YesNoDialog",
	(d)=>{}, 
	{
		 text: "Confirm delete"
	}
)

// The text 'Confirm delete' will be translated
this.openDialog(
	"YesNoDialog",
	(d)=>{}, 
	{
		 text: "__(Confirm delete)"
	}
)
```

#### Dynamic texts
Dynamic texts are often mixed between the text that needs to be translated and the application data. In our API, a dynamic text can be represented as a `string_format` (which is variant between locales), and a list of dynamic `values` (independent to locale). For example:
```javascript
// the string format
str = "This is {0} version {1}"
// this is the dynamic text with application data
str.format("AntOS","0.2.0a") // --> 'This is AntOS version 0.2.0a' 
```
Since we cannot use the directive `__(..)` in this case, the API provides the global function  `__(string_format, values...)` to handle dynamic texts. This function always returns a `FormatedString` object which will be automatically translated and formatted  when rendering, for example:

```Javascript
"Welcome to {0}".format("AntOS") // produces a normal text and wont be translated

// The following methods will provide FormatedString object and can be translated automatic by our API
__("Welcome to {0}", "AntOS") // --> FormatedString
"Welcome to {0}".f("AntOS") // --> FormatedString
/*
	when the system locale is set to fr_FR, these objects will be rendered as:
	'Bienvenue a`  AntOS'
*/
```

A dialog with the following code will render different texts (static and dynamic) depending on different locales:

```javascript
...
app.openDialog(
	"YesNoDialog", 
	(d) =>{...},
	{
		title: "__(Delete)",  // static text
		text: __("Do you really want to delete: {0}", file.name) // dynamic text
	}
)
...
```

This is the result on three locales: *en_GB, fr_FR, vi_VN* :
![](https://os.lxsang.me/VFS/shared/2ca59dd0896aaec0c64878ab89ad093318c2147a)
