# Fix some terrible bug
I think nobody real use this project
1. You can't get wysihtml5 editor, I ask question in stackoverflow: [this](http://stackoverflow.com/questions/31016428/bootstrap3-wysiwyg-editor-object-aways-undefined/31016780#31016780), at the end I fix this bug by myself, the author forgot return.


# Overview

Bootstrap-wysihtml5 is a javascript plugin that makes it easy to create simple, beautiful wysiwyg editors with the help of [wysihtml5](https://github.com/xing/wysihtml5) and [Twitter Bootstrap](http://twitter.github.com/bootstrap/).

## About this fork

This fork provides the same functionality as the original by [jhollingworth](https://github.com/jhollingworth/bootstrap-wysihtml5) but skips bootstrap 2 support and adds bootstrap 3 support.

Besides it uses bower for client side dependency management.

### Why a fork again?

The old repository seems to be dead. Pull Requests are not accepted anymore since some time. The other bootstrap-wysihtml5 packages in the bower registry do NOT provide bootstrap 3 support, even if the name so suggests. They are not maintained either.

Furthermore the packages on bower do not use proper dependency management.

I will maintain this repo and I will accept pull requests.

## Dependencies

Additionally to the existing dependencies I added handlebars in version 0.2.0. You only need the handlebars.runtime.min.js. The templates are precompiled during build. This adds less than 7kB to your client (~3kB gzipped). Thus it is easier to maintain the code.

Because maintaining code requires maintainable code.

## About us

 - [Waxolunist](https://github.com/Waxolunist) 
 - [schnawel007](https://github.com/schnawel007) 

## Development

Install all development dependencies via

    npm install

Additionaly you need grunt, grunt-cli and bower as global packages installed.

Also install the client dependencies via

    bower install

There is a grunt task for building. Just run

    grunt

on the command line and everything should build fine.

## Installation

If you are using bower use the “bootstrap3-wysihtml5-bower” package.

    bower install bootstrap3-wysihtml5-bower

If using Rails, use ["bootstrap-wysihtml5-rails"](https://github.com/Nerian/bootstrap-wysihtml5-rails):

    gem bootstrap-wysihtml5-rails

## Examples

-   [http://waxolunist.github.io/bootstrap3-wysihtml5-bower/](http://bootstrap-wysiwyg.github.io/bootstrap3-wysiwyg/)

For use with requirejs see the examples in the repo.

### Files to reference

In the folder dist you will the plain unminified javascript files and two kinds of minified files.

**bootstrap3-wysihtml5.min.js**: This file contains the jquery plugin, the templates and the english translations. If you are referencing this file, you have to reference jquery, bootstrap jquery plugin, handlebars runtime and wysihtml.js.

**bootstrap3-wysihtml5.all.min.js**: This file contains all of the normal minified file plus the handlebars runtime and editor library. If you are referencing this file, you have to reference jquery and bootstrap jquery plugin.

**bootstrap3-wysihtml5.min.css**: This is the stylesheet for this plugin.

If you are using any other translation than english, you have to either build this plugin yourself, or reference it separately.

### Usage

#### Editable Div

This is the new style from version 0.3 on and will be supported on all browsers.

The javascript will keep the same. For example it is easier to change the content dynamically on runtime.

```javascript
$('.textarea').html('Some text dynamically set.');
```

#### Textarea

This is the old style usage and does use a sandboxed iframe, which will from version 0.3 on not work in IE.

```html
<textarea id="some-textarea" placeholder="Enter text ..." style="styles to copy to the iframe"></textarea>
<script type="text/javascript">
	$('#some-textarea').wysihtml5();
</script>
```

You can get the html generated by getting the value of the text area, e.g. 

```javascript
$('#some-textarea').val();
```

#### RequireJS Support

From version 0.3 on, requirejs is supported.

The usage with requirejs will be the same with a slightly difference to the localisation feature. English will not be pre loaded, so translations not in the used language will not fallback to english but are just empty.
Also if using english you have to load the locale dependency.

A minimum example:

```javascript
require.config({
  paths: {
    'domReady': '../components/requirejs-domready/domReady',
    'jquery': '../components/jquery/jquery.min',
    'bootstrap': '../components/bootstrap/dist/js/bootstrap.min',
    'bootstrap.wysihtml5': '../dist/amd/bootstrap3-wysihtml5.all',
    'bootstrap.wysihtml5.de-DE': '../dist/locales/bootstrap-wysihtml5.de-DE'
  },
  shim: {
    'bootstrap': {
      deps: ['jquery']
    }
  },
  deps: [
    './start'
  ]
});
```

```
define([
  'require',
  'domReady',
  'jquery',
  'bootstrap.wysihtml5.de-DE'
], function(require, domReady, $) {
  'use strict';

  domReady(function() {
    $('.textarea').wysihtml5({
      locale: 'de-DE'
    }); 
  });
});
```

# Advanced

## Options

An options object can be passed in to `.wysihtml5()` to configure the editor:

```javascript
$('#some-textarea').wysihtml5({someOption: 23, ...})
```

#### Buttons

To override which buttons to show, set the appropriate flags:

```javascript
$('#some-textarea').wysihtml5({
	"font-styles": true, //Font styling, e.g. h1, h2, etc. Default true
	"emphasis": true, //Italics, bold, etc. Default true
	"lists": true, //(Un)ordered lists, e.g. Bullets, Numbers. Default true
	"html": false, //Button which allows you to edit the generated HTML. Default false
	"link": true, //Button to insert a link. Default true
	"image": true, //Button to insert an image. Default true,
	"color": false, //Button to change color of font  
	"blockquote": true, //Blockquote  
  "size": <buttonsize> //default: none, other options are xs, sm, lg
});
```

Since v0.3.0 toolbar options are nested in toolbar. So use therefore following syntax:

```javascript
$('#some-textarea').wysihtml5({
  toolbar: {
    "font-styles": true, //Font styling, e.g. h1, h2, etc. Default true
    "emphasis": true, //Italics, bold, etc. Default true
    "lists": true, //(Un)ordered lists, e.g. Bullets, Numbers. Default true
    "html": false, //Button which allows you to edit the generated HTML. Default false
    "link": true, //Button to insert a link. Default true
    "image": true, //Button to insert an image. Default true,
    "color": false, //Button to change color of font  
    "blockquote": true, //Blockquote  
    "size": <buttonsize> //default: none, other options are xs, sm, lg
  }
});
```

#### Font Awesome

To use Font Awesome for the icons of the toolbar you can use since v0.3.1 following option:

```javascript
  toolbar: {
    "fa": true
  }
```

Don't forget to add the font awesome stylesheet in this case, which is a bower dependency since v0.3.1.

#### Custom Templates for Toolbar Buttons

see wikie: [Custom Templates](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/wiki/Custom-Templates)

##### Data Attributes

As of version 0.2.4 there are two new data attributes. 

<dl>
  <dt>data-wysihtml5-format-name</dt>
  <dd>Normally the inner html of the format command is used to display as current font style. This attribute overrides this value.</dd>
  <dt>data-wysihtml5-display-format-name</dt>
  <dd>Whether to show the current font style in the toolbar anyways.</dd>
</dl>

#### Stylesheets

It is possible to supply a number of stylesheets for inclusion in the editor `<iframe>`:

```javascript
$('#some-textarea').wysihtml5({
	"stylesheets": ["/path/to/editor.css"]
});
```

#### Events

Wysihtml5 exposes a [number of events](https://github.com/xing/wysihtml5/wiki/Events). You can hook into these events when initialising the editor:

```javascript
$('#some-textarea').wysihtml5({
	"events": {
		"load": function() { 
			console.log("Loaded!");
		},
		"blur": function() { 
			console.log("Blured");
		}
	}
});
```

#### Shallow copy by default, deep on request

Options you pass in will be added to the defaults via a shallow copy.  (see [jQuery.extend](http://api.jquery.com/jQuery.extend/) for details). You can use a deep copy instead (which is probably what you want if you're adding tags or classes to parserRules) via 'deepExtend', as in the parserRules example below.

#### Parser Rules

If you find the editor is stripping out tags or attributes you need, then you'll want to extend (or replace) parserRules.  This example extends the defaults to allow the <code><strong></code> and <code><em></code> tags, and the class "middle":

```javascript
$('#some-textarea').wysihtml5('deepExtend', {
  parserRules: {
    classes: {
      "middle": 1
    },
    tags: {
      strong: {},
      em: {}
    }
  }
});
```

There's quite a bit that can be done with parserRules; see [wysihtml5's advanced parser ruleset](https://github.com/xing/wysihtml5/blob/master/parser_rules/advanced.js) for details.  bootstrap-wysihtml5's default parserRules can be found [in the source](https://github.com/jhollingworth/bootstrap-wysihtml5/blob/master/src/bootstrap-wysihtml5.js) (just search for 'parserRules' in the file).

#### Shortcuts

You can map your own shortcuts to commands. For example if you want to map the underline command to Alt+T call the editor with following options:

```javascript
$('#some-textarea').wysihtml5({
  shortcuts: {
    '84': 'underline'
  }
});
```

The code executes the command with Alt, Meta or Ctrl pressed. In the example above Ctrl-T in Chrome is already occupied by "New Tab", thus not overridable.

#### Defaults

You can change bootstrap-wysihtml5's defaults by altering:

```javascript
$.fn.wysihtml5.defaultOptions
```

This object has the same structure as the options object you pass in to .wysihtml5().  You can revert to the original defaults by calling:

```javascript
$(this).wysihtml5('resetDefaults') 
```

Operations on the defaults are not thread-safe; if you're going to change the defaults, you probably want to do it only once, after you load the bootstrap-wysihtml plugin and before you start instantiating your editors.

### The underlying wysihtml5 object

You can access the [wysihtml5 editor object](https://github.com/xing/wysihtml5) like this:

```javascript
var wysihtml5Editor = $('#some-textarea').data("wysihtml5").editor;
wysihtml5Editor.composer.commands.exec("bold");
```

### I18n

You can use Bootstrap-wysihtml5 in other languages. There are some translations available in the src/locales directory. You can include your desired one after the plugin and pass its key to the editor. Example:

```html
<script src="src/locales/bootstrap-wysihtml5.pt-BR.js"></script>
<script type="text/javascript">
  $('#some-textarea').wysihtml5({locale: "pt-BR"});
</script>
```

It is possible to use custom translations as well. Just add a new key to $.fn.wysihtml5.locale before calling the editor constructor.

# Release Notes

* *0.3.4* (not yet released):
* *0.3.3* (2014/09/08):
  * Refined bower dependency versions (see [#64](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/64)).
  * Updated wysihtml5x to 0.4.13 (see [#80](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/80))
  * Added automated tests with karma
  * Switched create link and create image modals to use wysihtml5 events, solving [#61](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/61).
* *0.3.2* (2014/06/22):
  * Updated and better build (see [#60](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/60) and [#56](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/56)).
  * Corrected size option in templates (see [#58](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/58))
  * Updated wysihtml5x to 0.4.9 (see [#57](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/57))
* *0.3.1* (2014/06/11):
  * Updated locales es-ES and es-AR ([#49](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/pull/49))
  * Updated wysihtml5x to 0.4.8 ([#53](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/53))
  * Added toolbar option for enabling font awesome ([#51](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/pull/51))
  * Resolved custom shortcut issue ([#43](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/43))
* *0.3.0* (2014/05/04):
  * Changed wysihtml implementation to wysihtml5x-0.4.4
  * Added support for requirejs ([#40](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/40))
  * Fixed version of jquery to be higher or equal to 2.1.0
  * This release adds support for div tags instead of textarea as editor container. The div tag will be from now on the recommended way of starting an editor instance.
* *0.2.12* (not yet released):
* *0.2.11* (2014/06/22):
  * Updated and better build (see [#60](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/60) and [#56](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/56)).
* *0.2.10* (2014/06/11):
  * Added option for small modals (adding class `.modal-sm` to `.modal-dialog`) ([#42](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/42))
  * Fixed version of jquery to be lower than 2.1.0 because of path incompatibilities
  * Updated locales es-ES and es-AR ([#49](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/pull/49))
  * Resolved custom shortcut issue ([#43](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/43))
* *0.2.9* (2014/02/28):
	* Added hebrew translation
	* Updated spanish translations (es-ES, es-AR)
	* Provided easier way of adding custom template ([#39](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/39))
* *0.2.8* (2014/02/11):
	* Updated bootstrap to version 0.3.1 ([#32](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/32))
	* Fixed issue with IE10 ([#30](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/30))
	* Updated build dependencies (see package.json)
* *0.2.7* (2014/01/16):
	* Updated brazilian translation. ([#26](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/pull/26))
	* Updated french translation. ([#27](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/pull/27))
	* Updated Bootstrap to the latest version 3.0.3 ([#28](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/pull/28))
	* Updated handlebars to the latest version 1.3.0 ([#28](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/pull/28))
* *0.2.6* (2013/12/20): 
	* Ability to define own shortcuts ([#23](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/23)).
	* New command "small" ([#19](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/19)).
	* Normal text writes now "p" instead of "div" ([#21](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/21)).
	* If a text element has no translation it falls back to english instead of an empty string ([#24](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/24)).
	* Some refactoring to add new commands in the future with ease.
	* New build task for easier development.
* *0.2.5* (2013/12/17): 
	* Updated german translation
	* Fix issue [#8](https://github.com/bootstrap-wysiwyg/bootstrap3-wysiwyg/issues/8): Provide fallback if locale is not found.
* *0.2.4* (2013/12/13): 
	* Added blockquote format button
	* Added 2 new data attributes: data-wysihtml5-display-format-name and data-wysihtml5-format-name
* *0.2.3* (2013/12/11): 
	* Updated arabic translation.
	* Updated bulgarian translation.
	* Updated danish translation.
	* Updated czech translation.
	* Updated catalan translation.
	* Updated brazilian translation.
* *0.2.2* (2013/11/23): 
	* Updated polish translation.
* *0.2.1* (2013/11/23): 
	* Updated french translation.
	* Added hungarian translation.
	* Allow strong, p and em to the list of allowed tags.
* *0.2.0* (2013/11/22): 
	* Clean up code (jshint).
	* Add handlebars for better maintainability of templates.
* *0.1.0* (2013/11/20): 
	* Forked code and made it fit for bower.
	* Add build tasks for grunt.
	* Some bug fixes, to run tests again.

# Thanks for assistance and contributions

* Garito (https://github.com/Garito)
* jmlweb (https://github.com/jmlweb)
* corbinu (https://github.com/corbinu)
* Nerian (https://github.com/Nerian)
