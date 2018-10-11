---
layout: post
title: Conan: First Impressions
tags: [diary, owl, conan]
---

I have been experimenting with Conan on my Max. I deally I would like to use it with [Owl][1] instead of the manual solution I have right now. This is essentially required if I want to get builds on Travis-CI and Appveyor working

The very first thing I tried was adding boost[2], however this immediately created a problem. For some reason `conanbuildinfo.cmake` file is changing/overwriting the binary folder, which causes a problem in Owl's final build step when it tries to copy the default `owl.ini` file into the binary's package.

More specifically I get the error:

(1)
```
[100%] Linking CXX executable ../bin/Owl.app/Contents/MacOS/Owl
Copying owl.ini
cp: /Users/adalidclaure/src/owlbin/Owl/Owl.app/Contents/Resources: No such file or directory
make[2]: *** [bin/Owl.app/Contents/MacOS/Owl] Error 1
make[2]: *** Deleting file `bin/Owl.app/Contents/MacOS/Owl'
make[1]: *** [Owl/CMakeFiles/Owl.dir/all] Error 2
make: *** [all] Error 2
[ ~/src/owlbin ]  
$> 
```

Which is caused simply by adding the following two lines to Owl's root `CMakeLists.txt` file:

(2)
```
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup(TARGETS)
``` 
