#exiv2-buffers

exiv2-buffers is a native c++ extension for [node.js](http://nodejs.org/) that provides
support for reading and writing image metadata via the [Exiv2 library](http://www.exiv2.org) with buffers.

This is a fork of Damian Beresford's [exiv2node](https://github.com/dberesford/exiv2node) which adds buffer support so you don't need to write to the filesystem.

## Dependencies

To build this addon you'll need the Exiv2 library and headers so if you're using
a package manager you might need to install an additional "-dev" packages.

### Debian

    apt-get install libexiv2 libexiv2-dev

### OS X

You'll also need to install pkg-config to help locate the library and headers.

[MacPorts](http://macports.org/):

    port install pkgconfig exiv2

[Homebrew](http://github.com/mxcl/homebrew/):

    brew install pkg-config exiv2

### Other systems

See the [Exiv2 download page](http://www.exiv2.org/download.html) for more
information.

## Installation Instructions

Once the dependencies are in place, you can build and install the module using
npm:

    npm install exiv2-buffers

You can verify that everything is installed and operating correctly by running
the tests:

    npm test

## Sample Usage

### Read tags:

    var ex = require('exiv2-buffers');

    ex.getImageTags(fs.readFileSync('./photo.jpg'), function(err, tags) {
      console.log("DateTime: " + tags["Exif.Image.DateTime"]);
      console.log("DateTimeOriginal: " + tags["Exif.Photo.DateTimeOriginal"]);
    });

### Load preview images:

    var ex = require('exiv2-buffers')
      , fs = require('fs');

    ex.getImagePreviews(fs.readFileSync('./photo.jpg'), function(err, previews) {
      // Display information about the previews.
      console.log(previews);

      // Or you can save them--though you'll probably want to check the MIME
      // type before picking an extension.
      fs.writeFile('preview.jpg', previews[0].data);
    });

### Write tags:

    var ex = require('exiv2-buffers')

    var newTags = {
      "Exif.Photo.UserComment" : "Some Comment..",
      "Exif.Canon.OwnerName" : "My Camera"
    };
    ex.setImageTags(fs.readFileSync('./photo.jpg'), newTags, function(err, buffer){
      if (err) {
        console.error(err);
      } else {
        console.log("setImageTags complete..");
        console.log(buffer);
      }
    });

### Delete tags:

    var ex = require('exiv2-buffers'),
      fs = require('fs');

    var tagsToDelete = ["Exif.Photo.UserComment", "Exif.Canon.OwnerName"];
    ex.deleteImageTags(fs.readFileSync('./photo.jpg'), tagsToDelete, function(err, buffer){
      if (err) {
        console.error(err);
      } else {
        console.log("deleteImageTags complete..");
        console.log(buffer);
      }
    });

Take a look at the `examples/` and `test/` directories for more.
