---
title: CMake Script Mode and Variables
date: '2024-07-25'
categories:
  - blog
tags:
  - cmake
published: true
---

## CMake Script Mode

CMake lets users write scripts in "Script Mode" that can do various tasks. I originally tried doing:

```shell
> cmake -P somescript.cmake -DSOME_VAR=some_value
```

However the `SOME_VAR` was never set. That's because in _Script Mode_ the variables have to come **before** the script:

```shell
> cmake -DSOME_VAR=some_value -P somescript.cmake
```

This may seem obvious, but when using CMake to generate, config and even build, this is not the case. For example something like the following is perfectly valid:

```shell
> cmake .. -DSOME_VAR=some_value
```

or

```shell
> cmake --build . -DSOME_VAR=some_value
```

