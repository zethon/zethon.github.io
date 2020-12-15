---
title: Using a Conan Recipe (the basics)
date: '2020-11-21'
categories:
  - blog
tags:
  - conan
published: true
---
I'm using [Simple Web Server](https://gitlab.com/eidheim/Simple-Web-Server) for a project and I wanted a way to pull the library down via Conan. I found [a recipe](https://github.com/inexorgame-obsolete/conan-simple-web-server) for the library a couple months ago but was still unsure of how to use it. If I couldn't find the library being hosted in an existing repo, how exactly do I use the recipe such that I can host the resource in my own repo? I had done this multiple times before but it was always a bastardization of the process and it was always something I had to figure out through trial and error.

Well here are the steps I used in order to use the existing recipe in my own [bintray](https://bintray.com) repo. I put this here as much as a note to myself as a guide to anyone else looking.

Before I get started I want to mention that these steps assume you already have a repo on bintray and that you've already authenticated your repo on your machine so that you can push to it (like I talked about [here](https://zethon.github.io/2019-11-06-hacking-together-a-conan-package/)). 

The first thing I did was fork the recipe so I could update it to the latest version of the library. My fork can be found [here](https://github.com/zethon/conan-simple-web-server). After cloning and updating the library's version number I was ready to start.

One I pulled the recipe and changed into its folder, I used the recipe to pull the library's source: `conan source .`

Then I "installed" the package: `conan install .`

Next I was ready to package everything together: `conan package .`

Now I was ready to install the package locally: `conan export-pkg . Simple-Web-Server/v3.1.1@owl/stable`

Finally, I was ready to upload the package to my repo: `conan upload Simple-Web-Server/v3.1.1@owl/stable -r owl --all`