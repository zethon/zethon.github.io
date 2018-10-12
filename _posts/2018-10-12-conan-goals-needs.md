---
layout: post
published: true
title: Libraries to consider for Owl and arcc
---
### [`fmt`](https://github.com/fmtlib/fmt)

A formatting library that allows you to do things like `std::string s = fmt::format("{0}{1}{0}", "abra", "cad");`. Nearly as fast, if not faster, than standard `std::cout` or `boost::format`.

### [`spdlog`](https://github.com/gabime/spdlog)

"Fast C++ logging library." Uses `fmt`-type syntax. Supports standard log sinks such as the console, text files and text files with rollovers.

### [JSON for Modern C++](https://github.com/nlohmann/json)

Very simple and easy to use JSON functionality using intuitive syntax and operations. This is already used in arcc.
