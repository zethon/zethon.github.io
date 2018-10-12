---
layout: post
published: false
title: Developer Notes for 2018-10-12
---
## `std::ref`

In one portion of our code, we use `std::async` to launch multiple work threads to talk to remote devices and gather results. I had to enhance this functionality so that total number of results does not exceed more than a parameterized number, or a *max*. To do this I use an `std::atomic_uint64_t` and pass it into the function. Since `std::atomic_uint64_t` is not copy-constructible (and it would make no sense to pass in a copy), I wanted to pass in a reference. Hence, `std::ref` generates a `std::reference_wrapper<T>` that essentially allows me pass my counter by reference.