---
title: Linking errors with boost on macOS
date: '2020-11-20'
categories:
  - blog
tags:
  - boost
  - cmake
published: true
---
I recently revived [an old project](https://github.com/zethon/arcc) and tried to get it to build on my Mac. I was immediately met with this error:

```
Undefined symbols for architecture x86_64:
  "boost::system::detail::system_category_instance", referenced from:
      boost::system::system_category() in ConsoleApp.cpp.o
      boost::system::system_category() in OAuth2Login.cpp.o
      boost::system::system_category() in RedditSession.cpp.o
      boost::system::system_category() in utils.cpp.o
  "boost::system::detail::generic_category_instance", referenced from:
      boost::system::generic_category() in arcc.cpp.o
      boost::system::generic_category() in CommandHistory.cpp.o
      boost::system::generic_category() in ConsoleApp.cpp.o
      boost::system::generic_category() in OAuth2Login.cpp.o
      boost::system::generic_category() in RedditSession.cpp.o
      boost::system::generic_category() in Settings.cpp.o
      boost::system::generic_category() in utils.cpp.o
      ...
ld: symbol(s) not found for architecture x86_64
```

This was perplexing because everything looked fine in my CMake files and I wasn't getting this error on Windows or Ubuntu. The first thing I did was print out verbose output to make sure I was in fact linking what I thought I was linking:

```
cd /Users/zethon/src/arcc/build/arcc && /usr/local/Cellar/cmake/3.18.3/bin/cmake -E cmake_link_script CMakeFiles/arcc.dir/link.txt --verbose=1
... (ommitted) ...
/lib -lboost_program_options -lboost_timer -lboost_thread -lboost_chrono -lboost_filesystem -lboost_system -lboost_stacktrace_addr2line -lboost_stacktrace_basic -lboost_stacktrace_noop -lboost_unit_test_framework -lcurl -lfmtd -lform_g -lmenu_g -lncurses++_g -lncurses_g -lpanel_g -lz /usr/local/lib/libboost_system.a /usr/local/lib/libboost_thread-mt.a /usr/local/opt/openssl/lib/libssl.dylib /usr/local/opt/openssl/lib/libcrypto.dylib
```

In case it's not obvious, the functions in my error are part of the Boost System library (which I found out through a Google search), and there it is, I'm definitely *am* linking to `boost_system`. So.. back to Google! 

Then I found [this link](https://stackoverflow.com/questions/13467072/c-boost-undefined-reference-to-boostsystemgeneric-category) on StackOverflow:

```
Adding add_definitions(-DBOOST_ERROR_CODE_HEADER_ONLY) for cmake file.
```

I added this to my CMake and it worked! 

Full admission, this is one of those answers that work I don't understand why or what's going on. But my build is working, so I'm ready to move on.
