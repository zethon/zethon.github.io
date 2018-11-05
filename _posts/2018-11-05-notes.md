---
layout: post
published: true
title: 'Notes for 2018-11-05 (Conan, GCC ABI)'
---
## Listing available Conan package configurations

The `conan search` command will list all of your installed packages. If you need to know what configruations of a package are availble on a particular remote you can use `conan search <package-name> -r <remote>`. For example:
```
$ conan search log4qt/0.3@owl/stable -r owl
Existing packages for recipe log4qt/0.3@owl/stable:

Existing recipe in remote 'owl':

    Package_ID: 0ab9fcf606068d4347207cc29edd400ceccbc944
        [settings]
            arch: x86_64
            build_type: Release
            compiler: gcc
            compiler.libcxx: libstdc++
            compiler.version: 7
            os: Linux
        Outdated from recipe: False

    Package_ID: 6cc50b139b9c3d27b3e9042d5f5372d327b3a9f7
        [settings]
            arch: x86_64
            build_type: Release
            compiler: Visual Studio
            compiler.runtime: MD
            compiler.version: 15
            os: Windows
        Outdated from recipe: False

    Package_ID: f8bda7f0751e4bc3beaa6c3b2eb02d455291c8a2
        [settings]
            arch: x86_64
            build_type: Release
            compiler: apple-clang
            compiler.libcxx: libc++
            compiler.version: 10.0
            os: Macos
        Outdated from recipe: True
```
