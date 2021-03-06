usefs
=====

A simple fluent API for HTML5 file access.


The HTML5 file API is an asynchronuous API to access local files and is based on callbacks. Because of its nested nature, the usage of the API quickly because a nightmare. Usefs will turn easily 100 lines of HTML5 File API code into a one liner. It is a great companion to Apache Cordova/PhoneGap based hybrid HTML5 mobile apps which usually need some local file access.

See this example to simple write some text  to a file:
```
useFS.fs().useDir(FILE_URL).useSubDir(DIR_NAME).useFile(FILE_NAME).writeText(DATA);
```

Compared to the HTML5 file API, using its typical nested structure of callbacks:
```
 window.requestFileSystem(LocalFileSystem.PERSISTENT, 1024 * 1024, function (fileSystem) {
        fileSystem.root.getDirectory(FILE_URL, {create: false, exclusive: false},
            function (dirEntry) {
                dirEntry.getDirectory(DIR_NAME, {create: false, exclusive: false},
                    function (dirEntry) {
                        dirEntry.getFile(FILE_NAME, {create: true, exclusive: false},
                            function (fileEntry) {
                                fileEntry.createWriter(
                                    function (fileWriter) {
                                        fileWriter.onwrite = function () {
                                            // SUCCESS, write completed
                                        };

                                        fileWriter.onerror = function () {
                                            // error
                                        };

                                        fileWriter.write(DATA);

                                    },
                                    function () {
                                        // error
                                    });
                            },
                            function (error) {
                                // error
                            });
                    },
                    function (error) {
                        // error
                    });
            },
            function () {
                // error
            });
    },
    function (err) {
        // error
    }
);
```

... OMG at least 40 lines of nested unmaintainable JavaScript for simple writing some text data to a file! The UseFS library helps to create readable and maintainable code.

Available operations on files
=========


  - readText();
  - writeText("string data");
  - readBinary();
  - writeBinary();
  - unlink();
  - rename("newName");



Available operations on directories
=======

  - createDir("dirName");
  - exists("fileName");
  - useSubDir("dirName");
  - removeAllFiles();
  - removeAll();
  - createSubDir("dirName");
  - copyIntoDir("dirName");
  - ls();    // listing of files and directories
  - useEntry();
  - unlink();
  - mkdir();
  - mkdirs();  



Available operations on file systems
=======

  - useDir("dirName or path");
  - useFile("fileName or path");

Available operations on path names
=======

  - UseFS.getFullPathNoEndSeparator();
  - UseFS.getPathParts();
  - UseFS.getExtension();
  - 









Installation
==

To use useFS in your own project you need to add the usefs.js Javascript file to your project. UseFS uses JavaScript promises, that is, it is required to have some Promise functionality in your project that can be used. AngularJS $q or the Q library from https://github.com/kriskowal/q can be used.

```
<script type="text/javascript" src="q.js"></script>
<script type="text/javascript" src="usefs.js"></script>
<script>
var useFS = new UseFS();
useFS.fs().useDir('file:///somepath').createDir('newdir').writeText('some text');
</script>
```


API design and usage
==

As promises are a great way to simplify asyncronuous APIs based on callbacks. If you have never heared of promises in JavaScript, see the readme of the q.js library at https://github.com/kriskowal/q/blob/v1/README.md for an introduction. 

All useFS methods returns a specialized promise with extra operations based wether the object in use is a file or a directory. And these methods returns again a promise, so calls can easily chained. That way, simple tasks as writing or reading a file remain simple.


Usage in AngularJS projects
==

In AngularJS projects it is adviced to inject AngularJS $q into useFS because this ensures that the promises are correctly integrated into the AngularJS digester.

```
<script type="text/javascript" src="angular.js"></script>
<script type="text/javascript" src="usefs.js"></script>
<script>

angular.module('myApp').controller('TestController', 
    function($q) {
        var useFS = new UseFS({q: $q});
        useFS.fs().useDir('file:///somepath').createDir('newdir').writeText('some text').then(
          function() {
            console.log("WRITE COMPLETED");
          });
    }
);
</script>
```









