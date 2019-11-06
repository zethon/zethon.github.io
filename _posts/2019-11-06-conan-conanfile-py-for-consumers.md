---
layout: post
published: true
title: Conan `conanfile.py` for consumers
date: '2019-11-06'
subtitle: Condition Conan includes
---
## Conan `conanfile.py` for consumers

I wanted to add [curses](https://en.wikipedia.org/wiki/Curses_(programming_library)) support to a cross-platform [consol project](https://github.com/zethon/arcc) I've been working on on and off for a couple years. The standard implmentation of ncurses for macOS and Linux was relatively easy to add via Conan, however there is no "ncurses for Windows" implementation. Instead there is [PDCurses](https://pdcurses.org/) which is a port of ncurses to Windows. 

So how could I resolve this in my `conanfile.txt` file? From what I could tell there was no way to tell the configuration "if we're on Windows then include *this*, but if we're on macOS then include *that*. I was right! There is no way to do it in `conanfile.txt` **but** `conanfile.txt` is not my only option!

When doing `conan install`, Conan will look for either `conanfile.txt` or `conanfile.py` (note: I have not experimented to see which one takes priority if they're both present). This might have been obvious to many but not to me!

Hence I was able to define my `conanfile.py` like so:

```
class MyConanFile(ConanFile):
    settings = "os", "compiler", "build_type", "arch"

    requires = (
        "boost/1.68.0@conan/stable",
    )

    generators = "cmake"

    default_options = {
        "boost:shared": False,
    }

    def requirements(self):
        if self.settings.os == "Windows":
            self.requires("pdcurses/3.6@arcc/stable")
        else:
            self.requires("ncurses/6.1@conan/stable")
```

The important part here being `def requirements`. Once I did this I was able to conditionally include files with Conan.