# Windows Azure Node.js Style Guide

This style guide outlines the coding conventions for Node.js at the Windows Azure SDK team.

## Introduction

Here are some of the documents from where we derived our styling guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [Felix's Node.js Style Guide](http://nodeguide.com/style.html)
* [Google Javascript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [Code Conventions for the Javascript Programming Language](http://javascript.crockford.com/code.html)

Another font of inspiration / guidance were the popular tools:
* [JsLint](http://www.jslint.com/)
* [JsHint](http://www.jshint.com/)

The usage of these tools, and their integration as part of the project's test run / CI server, together with the proper styling definition file, later defined in this document, will help ensure that most of the styling conventions presented are correctly followed.

## Table of Contents

* [Language Style Conventions](#language-styles)
  * [Spacing](#spacing)
  * [Semicolons](#semicolons)
  * [Conditionals](#conditionals)
    * [Ternary Operator](#ternary-operator)
  * [Trailing whitespaces](#trailing-whitespaces)
  * [Variable and property names](#variable-and-property-names)
  * [Class names](#class-names)
  * [Constants](#constants)
  * [Object / Array creation](#object-array-creation)
  * [Equality Operator](#equality-operator)
  * [Named closures](#named-closures)
  * [Nested closures](#nested-closures)
  * [Strings](#strings)
  * [Comments](#comments)
* [Object Oriented Style Conventions](#oo-styles)

## Language Style Conventions

This section describes the prefered style for writting general Node.js code.

### Spacing

* [Ryan Dahl](http://nodeguide.com/community.html#ryan-dahl) has chosen to indent using 2 spaces. We chose to do the same as most of the community. Never indent with tabs. Be sure to set this preference in your text editor.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement and close on a new line.

**For example:**
```javascript
if (condition) {
//Do something
} else {
//Do something else
}
```

### Semicolons

Semicolons aid readability by [eliminating ambiguity](https://news.ycombinator.com/item?id=1547647). As such, every statement should be trailed with one.


### Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g. one liners) to prevent mistakes and / or difficulty in reading.

**For example:**
```javascript
if (!error) {
    return success;
}
```

**Not:**
```javascript
if (!error)
    return success;
```

or

```javascript
if (!error) return success;
```

#### Ternary Operator

The Ternary operator should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables.

**For example:**
```javascript
result = a > b ? x : y;
```

**Not:**
```javascript
result = a > b ? x = c > d ? c : d : y;
```

### Trailing whitespaces

Trailing whitespaces are unecessary. They should be cleaned up before committing.

### Variable and property names

Variables and properties should use [lower camel case capitalization](http://en.wikipedia.org/wiki/CamelCase#Variations_and_synonyms) and be descriptive. Single character variables and abbreviations should be avoided.

**For example:**
```javascript
if (streamLength > MAXIMUM) {
  // Do something
}
```

**Not:**
```javascript
if (stream_length > MAXIMUM) {
  // Do something
}
```

or

**For example:**
```javascript
if (length > MAXIMUM_LENGTH) {
  // Do something
}
```

**Not:**
```javascript
if (l > MAXIMUM_LENGTH) {
  // Do something
}
```

### Class names

Class names should be capitalized using [upper camel case](http://en.wikipedia.org/wiki/CamelCase#Variations_and_synonyms).

**For example:**
```javascript
function BlobService() {
}
```

**Not:**
```javascript
function blob_service() {
}
```

### Constants

Constants should be declared as variables or static class properties, using all uppercase letters and underscore between words.

Node.js / V8 actually supports mozilla's const extension, but unfortunately that cannot be applied to class members, nor is it part of any ECMA standard.

**For example:**
```javascript
var PROTOCOL = 'http:';

function ServiceSettings() {
}
ServiceSettings.DEFAULT_PROTOCOL = 'http:';
```

**Not:**
```javascript
const PROTOCOL = 'http:';

function ServiceSettings() {
}
ServiceSettings.DEFAULT_PROTOCOL = 'http:';
```

### Object / Array creation

Use trailing commas and put short declarations on a single line. Only quote keys when your interpreter complains:

**For example:**
```javascript
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
};
```

**Not:**
```javascript
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

### Equality Operator

Comparisons in Javascript can be a bit [confusing](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FOperators%2FComparison_Operators). To avoid that, always use the strict (tripple equality) operators.

**For example:**
```javascript
var a = 0;
if (a === '') {
  console.log('winning');
}
```

**Not:**
```javascript
var a = 0;
if (a == '') {
  console.log('losing');
}
```

### Named closures

As a general rule, name your closures as it will produce better stack traces:

**For example:**
```javascript
req.on('end', function onEnd() {
  console.log('winning');
});
```

**Not:**
```javascript
req.on('end', function() {
  console.log('losing');
});
```

### Nested Closures

Use closures, but avoid nesting them more than 2 / 3 levels. Otherwise your code will become a mess.

**For example:**
```javascript
setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
```

**Not:**
```javascript
setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```

### Strings

For consistency, single-quotes (') should be used instead of double-quotes ("). This is helpful when creating strings that include HTML:

**For example:**
```javascript
var msg = 'Hello world';
var htmlString = '<a href="link" />';
```

**Not:**
```javascript
var msg = "Hello world";
var htmlString = "<a href=\"link\" />";
```

### Comments

When they are needed, comments should be used to explain why a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

For documentation comments (i.e. comments used to generate the APIs documentation) [JsDoc 3](https://github.com/jsdoc3/jsdoc) should be used. A listing of available tags can be found in [here](http://usejsdoc.org/).

## Object Oriented Style Conventions

This section described general patterns and styles for Object Oriented Programming in Node.js

### Inheritance

Node.js provides a built-in function to implement inheritance. It should be used.

**For example:**
```javascript
var util = require('util');

function BlobService() {
}

function StorageServiceClient() {
}

util.inherits(BlobService, StorageServiceClient);
```

### Method declaration

Two styles are prefered for declaring class methods.

If using [underscore.js](http://underscorejs.org/), the extend function should be used:

**For example:**
```javascript
var _ = require('underscore');

function BlobService() {
}

_.extend(BlobService.prototype, {
  getServiceProperties: function () {
    // Do something
  },

  setServiceProperties: function () {
    // Do something
  }
});
```

If underscore.js is not used by the project, the following style should be used:

**For example:**
```javascript
function BlobService() {
}

BlobService.prototype.getServiceProperties = function () {
  // Do something
}

BlobService.prototype.setServiceProperties = function () {
  // Do something
}
```

### Private methods

Methods that should not be invoked by the consumers of the API directly should be prefixed with the underscore character. This is enough to make these methods identifiable while, at the same time, making them easier to test / etc.

**For example:**
```javascript
function BlobService() {
}

BlobService.prototype._privateMethod = function () {
  // Do something
}
```

## JsHint configuration

One of the advantages of using JsHint versus JsLint is that you can select which rules you want to enforce for your project and, for some, even how. The following ".jshintrc" file is the prefer setup for our projects:

```javascript
{
  "bitwise": true,
  "camelcase": true,
  "curly": false,
  "eqeqeq": false,
  "forin": true,
  "immed": true,
  "indent": 2,
  "latedef": true,
  "maxparams": false,
  "maxdepth": false,
  "maxstatements": false,
  "maxcomplexity": false,
  "newcap": true,
  "noarg": true,
  "node": true,
  "noempty": true,
  "nonew": true,
  "plusplus": false,
  "quotmark": "single",
  "regexp": true,
  "sub": true,
  "strict": false,
  "trailing": true,
  "undef": true,
  "unused": true
}
```

Each of these options are described in great detail in [JsHint's webpage](http://www.jshint.com/docs/options/).