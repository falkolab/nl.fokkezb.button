# Button widget [![Titanium](http://www-static.appcelerator.com/badges/titanium-git-badge-sq.png)](http://www.appcelerator.com/titanium/) [![Alloy](http://www-static.appcelerator.com/badges/alloy-git-badge-sq.png)](http://www.appcelerator.com/alloy/)
This widget for the [Appcelerator](http://www.appcelerator.com) Titanium Alloy MVC framework provides a view that mimics `Ti.UI.Button`, but with more flexible styling and support for icon fonts.

![demo](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/demo.png)

## The repository
The repository contains two branches. This master branch contains the widget. The other [test](https://github.com/FokkeZB/nl.fokkezb.button/tree/test) branch has a complete Titanium Alloy demo/test project.

## Quick Start

### Get it [![gitTio](http://gitt.io/badge.png)](http://gitt.io/component/nl.fokkezb.button)
Download this repository and consult the [Alloy Documentation](http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_XML_Markup-section-35621528_AlloyXMLMarkup-ImportingWidgets) on how to install it, or simply use the [gitTio CLI](http://gitt.io/cli):

`$ gittio install nl.fokkezb.button`

### Use it

* Require the widget in a view:

```xml
<Widget src="nl.fokkezb.button" onClick="myCallback" icon="icon-on" title="My title" style="ios" />`
```
	
If you want to use icons:

* Specificy the font family to use via `iconFont`.
* Specificy the icon to use via `icon`. You probably have to give a unicode. Take a look at the demo to see how I use the `fa.js` generated by [FontAwesomeJS](https://github.com/FokkeZB/FontAwesomeJS) to map icon names to unicodes.
* Follow the Custom Fonts guide to add a custom font to your app.

See the reference way down this document for all available properties.

## Pre-defined styles
The widget comes with pre-defined styles that can be applied to a button just the `style` property, either in `<Widget />` or via `TSS`.

For example:

```xml
<Widget src="nl.fokkezb.button" title="Primary" style="bs-primary" />
```

| Style | Example | Description |
| ----- |:-------:| ----------- |
| ios   |![ios](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/ios.png)| Like `Ti.UI.Button` on iOS |
| bs-default |![bs-default](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-default.png)| [Twitter Bootstrap's](http://twitter.github.com/bootstrap/base-css.html#buttons) default button |
| bs-primary |![bs-primary](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-primary.png)| Twitter Bootstrap's primar button |
| bs-info |![bs-info](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-info.png)| Twitter Bootstrap's info button |
| bs-success |![bs-success](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-success.png)| Twitter Bootstrap's success button |
| bs-warning |![bs-warning](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-warning.png)| Twitter Bootstrap's warning button |
| bs-danger |![bs-danger](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-danger.png)| Twitter Bootstrap's danger button |
| bs-inverse |![bs-inverse](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-inverse.png)| Twitter Bootstrap's inverse button |
| bs-link |![bs-link](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/bs-link.png)| Twitter Bootstrap's link button |

## Adding styles and setting the default
Add styles by requiring the widget's styles-lib in your app's `alloy.js` and use the `set()` method to set (or override) a pre-defined style like this:

```javascript
var Styles = require('nl.fokkezb.button/styles');
Styles.set('my-style', {
	borderWidth: 1,
	borderColor: '#000',
	backgroundColor: '#ccc',
	activeStyle: {
		backgroundColor: '#000',
		color: '#fff'
	}
});
```

The lib also exposes `has()` and `get()`. Use the latter to get and extend an existing style before overwriting it using `set()`.

You can also set the default style to be used for any button that doesn't specify a `style` itself. On iOS the `ios` style is the default. Change it in your app's `alloy.js` like this:

```javascript
var Styles = require('nl.fokkezb.button/styles');
Styles.setDefault('bs-default');
```

An example can be found in the demo app's `app/alloy.js` file.

## TSS styling
Of course you can also style the widget like any other view using the `TSS` file of the controller-view where your require the widget. Just set a `class` or `id` to the `<Widget />` and style it like this:

```javascript
"#myButton": {
	borderWidth: 1,
	borderColor: '#000',
	backgroundColor: '#ccc',
	activeStyle: {
		backgroundColor: '#000',
		color: '#fff'
	},
	iconFont: 'FontAwesome',
	icon: Alloy.Globals.fa.powerOff
}
```

## Run-time styling
You can use `$.myId.applyProperties()` to apply any new properties to the widget after it has been automatically initialized:

```javascript
$.myId.applyProperties({
	color: 'red'
});
```

## Event listeners
Listen to the click-event like you would do with a regular button. 

Either in `<Widget />`:

```xml
<Widget onClick="myCallback" title="My title" id="myId" />
```

Or in the requiring controller:

```javascript
$.myId.addEventListener('click', myCallback);
```

The callback receives an Object containing 2 properties:

* `type`: Always `click`.
* `bubbles`: Always `FALSE`.
* `source`: The widget's main controller, including all non-object properties defined via XML or TSS.

An example can be found in the demo app's `app/controllers/index.js` file.

Since 1.3, when listening to a parent of the button, the `e.source` will also contain all non-object properties defined for the widget via XML or TSS.

## Property reference
To understand what properties are available and what they do, you need to understand how the button is constructed. The next illustration shows the outer (purple), inner (red), icon (blue) and title (green) elements and where padding and spacing is applied.

![elements](https://github.com/fokkezb/nl.fokkezb.button/raw/master/docs/elements.png)

### Widget-specific properties

| Property | Type | Purpose |
| -------- | ---- | ------- |
| enabled | Boolean | If `false`, the disabledStyle will be applied and no events are triggerd |
| title | String | Optional text for the title of the button |
| titleid | String | Optional textid for an internationalized title of the button |
| icon | String | Optional icon image file name or the text/unicode if you use an icon font. For SVG icon support you must add additional module to your project: `gittio install com.geraudbourdin.svgview`.
| iconFont | String | The font family to use for the icon. |
| iconSize | Number | Font size for the icon, overruling the button fontSize |
| iconPosition | String | Positions the icon to the `left` (default) or the `right` of the title |
| padding | Number/String/Object | Padding between the outer and the inner. Either pass one dimension, a list of 1 to 4 dimensions or an object containing one of `top`, `right`, `bottom` and `left` as keys and their dimension as values. Don't use padding if you specify a width and height and want the inner to be centered in the outer. |
| spacing | Number/String | Spacing between the icon and the title |
| style | String | Pre-defined style to load before applying any of the common properties in the next table.
| activeStyle | Object | Object containing any of the common properties listed in the next table that will be applied while the user presses the button. |
| disabledStyle | Object | Object containing any of the common properties listed in the next table that will be applied if the button is not enabled. |

### Titanium-common properties

| Property | Type | Applied to | Comments |
| -------- | ---- | ---------- | -------- |
| width, height | Number/String | outer |
| top, right, bottom, left | Number/String | outer ||
| center | Point | outer ||
| borderWidth | Number/String | outer ||
| borderColor | String | outer ||
| borderRadius | Number/String | outer ||
| backgroundColor | Number/String | outer ||
| backgroundGradient | Gradient | outer ||
| backgroundImage | String | Outer ||
| backgroundTopCap | Number | Outer ||
| backgroundLeftCap | Number | Outer ||
| backgroundRepeat | Boolean | Outer ||
| color | String | title, icon ||
| shadowOffset | Object | title, icon ||
| shadowColor | String | title, icon ||
| font | Font | title, icon | For the icon, `font.fontFamily` is ignored and `font.fontSize` can be overrulled by `iconSize` |
| textAlign | String | title | Only when width is set |
| verticalAlign | String | title | Only when height is set
| opacity | Number | outer ||
| visible | Boolean | outer ||
| bubbleParent | Boolean | outer ||

### Public interface

| Method | Description |
| ------ | ----------- |
| applyProperties() | Main method for setting up and changing the button |
| show() | Shows button |
| hide() | Hides button |
| setIcon([icon], ]iconFont]) | Changes the icon |
| getIcon() | Returns an `{ icon:, iconFont }` object |
| setTitle(title) | Changes the title |
| getTitle() | Returns the title |
| setTitleid(titleid) | Changes the titleid |
| getTitleid() | Returns the titleid |
| setStyle(style) | Applies the style |
| getStyle() | Returns the style name |
| addEventListener(event, callback) | Attaches event listener |
| removeEventListener(event, callback) | Removes event listener |
| fireEvent(event, dictionary) | Fires event on the button |
| id | Holds the `id` of the widget, so you have this in events |

## Ideas for improvement
Feel free to help me improve this widget by forking and submitting pull requests or issues with more ideas. Here's my whishlist:

* Add more custom and run-time style examples to the 4th demo app tab.
* Add more iOS button styles, like those in the navigation bar.
* Add several Android system styles.
* Add Foundation and other well-known button styles.
* Add 'system' style that picks the right iOS or Android system style.
* Support more icon fonts.
* Support focusStyle for Android.

## Changelog

* 1.4 BREAKING: For font icons you now have to give the unicode, no longer supports names!
* 1.3 Exposing creation properties via `e.source`. Fixed Android pass throughs.
* 1.2.2 Fixed iconSize not being retained. Added bubbleParent.
* 1.2.1 Fixed icon-bug, added new setters and getters
* 1.2 Added image icons, titleid, setStyle, backgroundImage, textAlign, verticalAlign
* 1.1 Added iconSize, setTitle, setIcon
* 1.0 Initial version

## Licenses
This project is licensed under the Apache Public License (Version 2). Please see the LICENSE.txt file for the full license.

The Font Awesome font is licensed under the [SIL Open Font License](http://scripts.sil.org/OFL). The Font Awesome pictograms are licensed under the [CC BY 3.0 License](http://creativecommons.org/licenses/by/3.0/). Attribution is no longer required in Font Awesome 3.0, but much appreciated.

You can get [Ligature Symbols](http://kudakurage.com/ligature_symbols/) for free. This Font is licensed under the SIL Open Font License for download and using. Ligature Symbols has broad support for the modern browser (Chrome, Safari, Firefox, iOS - Mobile Safari, Android Browser).

Appcelerator, Appcelerator Titanium and associated marks and logos are 
trademarks of Appcelerator, Inc. 

Titanium is Copyright (c) 2008-2012 by Appcelerator, Inc. All Rights Reserved.

Titanium is licensed under the Apache Public License (Version 2). Please
see the LICENSE file for the full license.
