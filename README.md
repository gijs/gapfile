gapfile
=======

Gapfile is a set of convenience wrapper functions for the file API in PhoneGap/Apache Cordova. It lets you use single (and simple) functions for file reads, writes, directory listings, etc. avoiding the callback hell. The downside is that the error reporting is less fine-grained (you'll basically just get a single error callback if something goes wrong).

##Files included

* gapfile.js  -- the JavaScript object that implements all gapfile functions.
* index.html -- a basic PhoneGap index.html for testing gapfile.js.
* README.md -- this document
* NOTICE -- Apache 2.0 license information (the same as PhoneGap/Cordova itself).


##Potential gotchas

* This has been tested with Apache/Cordova 2.7.0. While it should work with later versions, it may not work with earlier ones.
* Not all devices support access to the file system (at least through PhoneGap), though most of the "majors" do.
* Not all devices use the same directory structure. iOS creates a separate and isolated root directory for each app (which can be accessed at /), while Android wants you to write in the /sdcard/ directory. I speculate that the other platforms that support file I/O (Blackberry, Windows Phone x.x, etc.) also vary in this respect, but I don't own any of them and have been unable to test it. Reports welcome -- if you can tell me the location of the logical and permitted place for users to write files on your platform, I'll add it to the docs and the test code.
* As can perhaps be inferred from the previous point, the code *has not been tested on anything other than iOS and Android*. It's been tested on iPhone, iPad, and a couple of Android devices (a cheap Huawei Ideos phone and an Amazon Kindle Fire). However, if the PhoneGap File API is fully implemented for your device, gapfile should also work (presuming you set the proper folder -- see previous point). Please file a bug report if you run across any exceptions.
* Don't forget to set up your PhoneGap configuration file(s) to grant file access permissions. Otherwise your app won't be able to write files on most platforms. See the [PhoneGap File API docs](http://docs.phonegap.com/en/2.7.0/cordova_file_file.md.html#Files) for details.
* If you want to transfer files via iTunes on iOS, you'll also need to set "Application supports iTunes file sharing" to "YES" in your (appname)-Info.plist file (under the yellow Resources folder in Xcode).

##Running the tests

Create a new PhoneGap/Cordova project, replace the generated index.html with the index.html from gapfile, and drop in the gapfile.js file. Build and run as normal. You may want to test the error handling by (e.g.) writing a file, deleting it, then attempting to read it. 

##Public API

Below are gapfile's public API functions with a brief summary of their use. See index.html for example usage.

Unless an extremely good reason presents itself, these should remain stable in any future versions of gapfile. Note that gapfile also has some other functions that aren't mentioned here. Those are intended for internal use only. Use them at your own risk; there's no guarantee that they will remain the same across releases.

**writeFile(*fullpath*, *data*, *success*, *fail*)**

Write data to a file.

Parameters: 

 fullpath: full path including file name (if no path portion is given, assumes /).

	Examples: test.txt /test.txt /some/folder/test.txt (note that iOS doesn't allow subdirectories at this time).

 data: the data to write.

 success: callback function on successful write.

 fail: callback function if error occurs.


**appendFile(*fullpath*, *data*, *success*, *fail*)**

Identical to writeFile, except appends the data to an existing file.


**readFile(*fullpath*, *asText*, *success*, *fail*)**

Read data from a file.

Parameters: 

 fullpath: full path including file name (if no path portion is given, assumes /).

     Examples: test.txt /test.txt /some/folder/test.txt (note that iOS doesn't allow subdirectories at this time).

 asText: boolean specifying whether to read as text or a data URI. If true, calls success with the text as the parameter. If false, calls success with the data URI as the parameter.

 success: callback function on successful read.

 fail: callback function if error occurs.


**deleteFile(*fullpath*, *success*, *fail*)**

Delete a file

Parameters: 

 fullpath: full path of file to delete, including file name (if no path portion is given, assumes /).

     Examples: test.txt /test.txt /some/folder/test.txt (note that iOS doesn't allow subdirectories at this time).

 success: callback function on successful delete.

 fail: callback function if error occurs.


**readDirectory(*dirname*, *success*, *fail*)**

Get a list of the files in a directory.

Parameters:

 dirname: full path to directory

 success: callback function, called with an array of file names from the directory.

 fail: callback function if error occurs.


**fileExists(*fullpath*, *callback*, *fail*)**

Check for file existence.

Parameters:

 fullpath: full path of name to check

 success: called with true if file is found, false if file is not found.

 fail: callback function if error occurs.

