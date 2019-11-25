---
layout: post
published: false
title: More Conan Hacking
---
This was after pulling the recipe, cd'ing into the folder and `mkdir bin`. Then all of these commands were done in `bin`.
```
  774  conan search pdcurses/3.9@zethon/stable -r arcc
  775  rm -rf *
  776  conan install .. -d build_type=Debug
  777  conan install .. -s build_type=Debug
  778  conan source ..
  779  conan build .. -sf . -s build_type=Debug
  780  conan build .. -sf .
  781  conan package .. -sf . -s build_type=Debug
  782  conan package .. -sf .  build_type=Debug
  783  conan package .. -sf .
  784  conan export-pkg .. pdcurses/3.9@zethon/stale -pf .
  785  conan upload pdcurses/3.9@zethon/stable -r arcc --all
  786  conan search pdcurses/3.9@zethon/stable -r arcc
```