GapFile
=======

GapFile is a set of convenience wrapper functions for the file API in PhoneGap/Apache Cordova. It lets you use single (and simple) function calls for file reads, writes, directory listings, etc. avoiding the callback hell. The downside is that the error reporting is less fine-grained (you'll basically just get a single error callback if something goes wrong).

GapFile was written by [Tony Hursh](http://people.phonegap.com/developer/anthony-hursh/). I'm currently available for remote contract work.

## Recent (significant) changes

Repackaged as 3.0-compatible plugin.

The gapFile object has been renamed GapFile to be more consistent with other plugins.

As a convenience, GapFile.writeFile now calls success callback with the file's URL as a parameter (e.g., writing /test.txt will send file://(some horrifically ugly platform-dependent path)/test.txt to the success callback).

## Files included

* gapfile.js  -- the JavaScript object that implements all GapFile functions.
* index.html -- a basic PhoneGap index.html for testing gapfile.js.
* README.md -- this document.
* NOTICE -- Apache 2.0 license information (the same as PhoneGap/Cordova itself).
* plugin.xml -- For PhoneGap/Cordova 3.0 and above.


## Usage

If you are using Cordova/PhoneGap 3.0 or above, you can install this with:

cordova plugin add https://github.com/tonyhursh/gapfile.git

Remove with:

cordova plugin remove com.contraterrene.GapFile

You will also need the base Cordova File plugin:

cordova plugin add https://git-wip-us.apache.org/repos/asf/cordova-plugin-file.git

If you want to run the test index file (and probably for your own stuff) you'll need:

cordova plugin add https://git-wip-us.apache.org/repos/asf/cordova-plugin-device.git

so that the deviceready event fires correctly.

If you are using an earlier version, just add the www/gapfile.js file to your project and include the script in your index.html file. Also, earlier versions will typically need to have file access enabled in the configuration file(s) for the project. See the documentation for your particular device/version of PhoneGap.

##Potential gotchas

* This has been tested with Phonegap/Cordova 2.7.0 and 3.0.x. While it should work with later versions, it may not work with earlier ones.
* Not all devices support access to the file system (at least through PhoneGap), though most of the "majors" do.
* Not all devices use the same directory structure. iOS creates a separate and isolated root directory for each app (which can be accessed at /), while Android wants you to write in the /sdcard/ directory. I speculate that the other platforms that support file I/O (Blackberry, Windows Phone x.x, etc.) also vary in this respect, but I don't own any of them and have been unable to test it. Reports welcome -- if you can tell me the location of the logical and permitted place for users to write files on your platform, I'll add it to the docs and the test code.
* As can perhaps be inferred from the previous point, the code *has not been tested on anything other than iOS and Android*. It's been tested on iPhone, iPad, and a couple of Android devices (a cheap Huawei Ideos phone and an Amazon Kindle Fire). However, if the PhoneGap File API is fully implemented for your device, GapFile should also work (presuming you set the proper folder -- see previous point). Please file a bug report if you run across any exceptions.
* If you want to transfer files via iTunes on iOS, you'll also need to set "Application supports iTunes file sharing" to "YES" in your (appname)-Info.plist file (under the yellow Resources folder in Xcode).

##Running the tests

Create a new PhoneGap/Cordova project, install the plugins (or manually add gapfile.js and enable file access in the config for pre-3.0 versions), and replace the generated index.html with the index.html from this project. Build and run as normal. You may want to test the error handling by (e.g.) writing a file, deleting it, then attempting to read it. 

##Public API

Below are GapFile's public API functions with a brief summary of their use. See index.html for example usage.

Unless an extremely good reason presents itself, these should remain stable in any future versions of GapFile. Note that GapFile also has some other functions that aren't mentioned here. Those are intended for internal use only. Use them at your own risk; there's no guarantee that they will remain the same across releases.

**GapFile.writeFile(fullpath, data, success, fail)**

Write data to a file.

Parameters: 

 *fullpath*: full path including file name (if no path portion is given, assumes /).

	Examples: test.txt /test.txt /some/folder/test.txt

 *data*: the data to write.

 *success*: callback function on successful write. Called with the file URL as the sole parameter. 

 *fail*: callback function if error occurs.


**GapFile.appendFile(fullpath, data, success, fail)**

Identical to writeFile, except appends the data to an existing file.


**GapFile.readFile(fullpath, asText, success, fail)**

Read data from a file.

Parameters: 

 *fullpath*: full path including file name (if no path portion is given, assumes /).

     Examples: test.txt /test.txt /some/folder/test.txt 

 *asText*: boolean specifying whether to read as text or a data URI. If true, calls success with the text as the parameter. If false, calls success with the data URI as the parameter.

 *success*: callback function on successful read.

 *fail*: callback function if error occurs.


**GapFile.deleteFile(fullpath, success, fail)**

Delete a file.

Parameters: 

 *fullpath*: full path of file to delete, including file name (if no path portion is given, assumes /).

     Examples: test.txt /test.txt /some/folder/test.txt 

 *success*: callback function on successful delete.

 *fail*: callback function if error occurs.


**GapFile.readDirectory(dirname, success, fail)**

Get a list of the files in a directory.

Parameters:

 *dirname*: full path to directory

 *success*: callback function, called with an array of file names from the directory.

 *fail*: callback function if error occurs.


**GapFile.mkDirectory(dirName, success, fail)**

Create a subdirectory.

Parameters: 

 *dirName*: full path to directory

 *success*: callback function for successful create.

 *fail*: callback function if error occurs.


**GapFile.rmDirectory(dirName, success, fail)**

Delete a subdirectory.

Parameters: 
 *dirName*: full path to directory

 *success*: callback function for successful delete.

 *fail*: callback function if error occurs.


**GapFile.fileExists(fullpath, callback, fail)**

Check for file existence.

Parameters:

 *fullpath*: full path of name to check

 *success*: called with true if file is found, false if file is not found.

 *fail*: callback function if error occurs.

