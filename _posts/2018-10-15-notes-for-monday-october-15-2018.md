---
layout: post
published: true
title: 'Notes for Monday, October 15, 2018'
---
## Conan `NO_OUTPUT_DIRS` and `KEEP_RPATHS`

Going back to a post a few days ago about problems I was having with [using Conan in my CMake project](https://zethon.github.io/2018-10-09-thursday-morning/), it turns out that by default Conan will adjust the output directories how it sees fit. If you look at the `conan_output_dirs_setup()` macro in `conanbuildinfo.cmake`, the `CMAKE_RUNTIME_OUTPUT_DIRECTORY` along with other directories are adjusted. This is easily skipped by adding `NO_OUTPUT_DIRS` to the `conan_basic_setup()` call. 

Likewise, Conan also adjusts some of the rpaths in the resulting executable. This was a surprise when I finally got my binary to build but then wouldn't run. Again, a simple argument to `conan_basic_setup()` fixed this, in this case `KEEP_RPATHS`.

Hence, the call in my project's CMakeLists.txt file ended up looking like such: `conan_basic_setup(NO_OUTPUT_DIRS KEEP_RPATHS)`.