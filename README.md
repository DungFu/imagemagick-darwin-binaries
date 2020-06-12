# imagemagick-darwin-binaries

imagemagick static binaries for Mac OSX x64

When you want to write text into images, you have to pass the font explicitly.

## Motivation
Use imagemagick in electron.atom applications for mac
For more information about electron atom see [https://electron.atom.io/](https://electron.atom.io/)

## Installation

This module is installed via npm:

``` bash
$ npm install imagemagick-darwin-binaries
```

## Example Usage

Returns the path of a statically linked graphicsmagick on the local filesystem.

``` js
var imagemagick = require('imagemagick-darwin-binaries');
console.log(imagemagick.path);

```

For Windows (32 and 64-bit) please use graphicsmagic-static.

Best usage is with npm module gm

``` js
var os = require("os");
var graphicsmagick = require("graphicsmagick-static");
var imagemagick = require("imagemagick-darwin-binaries");
var imageHandler = null;

if (os.platform() == "win32") {
    gm = require("gm").subClass({
        appPath: path.join(graphicsmagick.path, "/")
    })
} else {

    gm = require("gm").subClass({
        imageMagick: true,
        appPath: path.join(imagemagick, "/")
    })
}

// then do any stuff you need

gm("/path/to/image.png").resize(150,150).write("/new/path.jpg", (err) => {
    
    // here an example
    if (err) return callback(err, null);
});
```

## Electron and Asar
Thinks to keep in mind when packing electron apps with ASAR.
Using e.g. the electron-builder, this package and as well graphicsmagick-static should be placed in the **app.asar.unpacked** folder.
Electron builder does this automatically, because of the big binaries.

How ever, when done so, you will have to replace the path of the **app.asar** to app.**asar.unpacked**.

This could be achieved by a helper class as given:

``` js
import path from 'path';

export default class AppPaths {
    static replaceAsar(path = "") {
        return path.replace(".asar", ".asar.unpacked");
    }
}

...

// Usage:

import AppPaths from 'WhatYouCalledTheFile';
var imagemagickPath = require("imagemagick-darwin-binaries").path;
let fixedPath = AppPaths.replaceAsar(imagemagickPath);

// Example for using ASAR:

var os = require("os");
var graphicsmagick = require("graphicsmagick-static");
var imagemagick = require("imagemagick-darwin-binaries");
var imageHandler = null;

if (os.platform() == "win32") {
    gm = require("gm").subClass({
        appPath: AppPaths.replaceAsar(path.join(graphicsmagick.path, "/"))
    })
} else {

    gm = require("gm").subClass({
        imageMagick: true,
        appPath: AppPaths.replaceAsar(path.join(imagemagick.path, "/"))
    })
}

// then do any stuff you need

gm("/path/to/image.png").resize(150,150).write("/new/path.jpg", (err) => {
    
    // here an example
    if (err) return callback(err, null);
});
```

