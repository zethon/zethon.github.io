---
title: "Simple CMake to Compile Resources"
date: '2020-12-08'
categories:
  - blog
tags:
  - cmake
  - resources
---
Recently I have been working on a project that has a [Simple Web Server](https://gitlab.com/eidheim/Simple-Web-Server), so I needed a way to serve HTML files. 

I went to Google and searched `c++ cmake compile resources`. The first project I found was [this](https://vector-of-bool.github.io/2017/01/21/cmrc.html). However this project was much more involved than I needed, going so far as to implement a mini-filesystem, much like Qt's resource system.

What I wasnted was more simple. I wanted to take a resource, compile it into a header as a `char` array. This was my solution:

```cmake
function(generate_header INPUTFILE OUTPUTFILE VARNAME)
    set(DONE FALSE)
    set(CURRENTPOS 0)

    file(READ ${INPUTFILE} FILEDATA HEX)
    string(LENGTH "${FILEDATA}" DATALEN)
    set(OUTPUT_DATA "")

    file(WRITE ${OUTPUTFILE} "#pragma once\n\nstatic const char ${VARNAME}[] = { ")

    foreach(BYTE_OFFSET RANGE 0 "${DATALEN}" 2)
        string(SUBSTRING "${FILEDATA}" "${BYTE_OFFSET}" 2 HEX_STRING)
        string(LENGTH "${HEX_STRING}" TEMPLEN)
        if ("${TEMPLEN}" GREATER 0)
            set(OUTPUT_DATA "${OUTPUT_DATA}0x${HEX_STRING}, ")
        endif()
    endforeach()

    file(APPEND ${OUTPUTFILE} "${OUTPUT_DATA}0")
    file(APPEND ${OUTPUTFILE} " };\n")
endfunction()

function(z_compile_resources RESOURCE_LIST)
    foreach(RESOURCE_NAME ${ARGN})
        set(RESOURCE_FILENAME "${CMAKE_CURRENT_SOURCE_DIR}/${RESOURCE_NAME}")

        get_filename_component(FILENAME ${RESOURCE_NAME} NAME_WE)
        get_filename_component(EXT ${RESOURCE_NAME} EXT)
        string(SUBSTRING ${EXT} 1 -1 EXT)
        set(OUTPUT_FILE "${CMAKE_CURRENT_BINARY_DIR}/${FILENAME}_${EXT}.h")
        set(VARIABLE_NAME "${FILENAME}_${EXT}")

        generate_header(${RESOURCE_FILENAME} ${OUTPUT_FILE} ${VARIABLE_NAME})
    endforeach()
endfunction()
```

Now I can compile resources in my CMake file like so:

```cmake
z_compile_resources(RESOURCE_FILES
    html/index.html
)
```

And when we run CMake header file that looks like this is generated:

```cpp
#pragma once

static const char index_html[] = { 0x3c, 0x68, ... , 0 };
```

This is awesome! And now in my source file I can use it like so:

```cpp
#include "index_html.h"

/* lots of code here */
std:::cout << index_html << '\n';
```

