---
layout: post
published: true
title: 'Conan: Adding a new build_type to an existing package'
date: '2019-11-24'
---
I am still working my way through learning Conan. I only make changes to it when I have to, which luckily isn't often but whenever I do it's aways a learning experience.

In a [previous post](https://zethon.github.io/2019-11-06-hacking-together-a-conan-package/) I talked about how I had to create a PDCurses package for one of my projects. When I created the package I didn't think about different configurations since I was only building "Release" and "RelWithDebInfo" on Windows in my project. However, I recently added Codecov support to my project so I needed to build "Debug" builds on Windows. Suddenly I realized that I needed to add a "Debug" configuration to my PDCurses packages.

After some searching I found [this recipe](https://github.com/alex-precosky/conan-pdcurses) for PDCurses 3.6. I forked the repo, pulled it locally and started hacking at it. Here are the steps I ultimately followed to add "Debug" libraries to my existing requirements:

* `conan search pdcurses/3.9@zethon/stable -r arcc` - First I confirmed that I did indeed only have a `Release` package

```
$ conan search pdcurses/3.9@zethon/stable -r arcc
Existing packages for recipe pdcurses/3.9@zethon/stable:

Existing recipe in remote 'arcc':

    Package_ID: 6cc50b139b9c3d27b3e9042d5f5372d327b3a9f7
        [settings]
            arch: x86_64
            build_type: Release
            compiler: Visual Studio
            compiler.runtime: MD
            compiler.version: 15
            os: Windows
        Outdated from recipe: False
```

* Clone the recipe, cd into the recipe's folder, mkdir a `bin` folder and cd into the `bin`
* `conan install .. -s build_type=Debug` - Generate the build info
* `conan source ..` - Clone PDCurses's code locally
* `conan build .. -sf .` - Build everything! The `-sf .` is saying "the source files are in the current folder"
* `conan package .. -sf .` - Now we prepare everything a publishable package
* `conan export-pkg .. pdcurses/3.9@zethon/stale -pf .` - I believe this just installs the package locally. It may not strictly be necesary if it you just upload the package and then download it through the consuming project.
* `conan upload pdcurses/3.9@zethon/stable -r arcc --all` - Push everything up to our repo.
* `conan search pdcurses/3.9@zethon/stable -r arcc` - And nowe we verify we have a Debug package.

```
$ conan search pdcurses/3.9@zethon/stable -r arcc
Existing packages for recipe pdcurses/3.9@zethon/stable:

Existing recipe in remote 'arcc':

    Package_ID: 6cc50b139b9c3d27b3e9042d5f5372d327b3a9f7
        [settings]
            arch: x86_64
            build_type: Release
            compiler: Visual Studio
            compiler.runtime: MD
            compiler.version: 15
            os: Windows
        Outdated from recipe: True

    Package_ID: 8cf01e2f50fcd6b63525e70584df0326550364e1
        [options]
            shared: False
        [settings]
            arch: x86_64
            build_type: Debug
            compiler: Visual Studio
            compiler.runtime: MDd
            compiler.version: 15
            os: Windows
        Outdated from recipe: False
```

After this I was able to do Debug builds in my project.