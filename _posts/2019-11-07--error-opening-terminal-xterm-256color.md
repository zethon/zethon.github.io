---
layout: post
published: true
title: '`Error opening terminal: xterm-256color`'
date: '2019-11-07'
---
PDCurses worked just fine on Windows, but running the same code on macOS with ncurses kept giving me this error:

```
arcc(86910,0x7fff96138380) malloc: *** error for object 0x14: pointer being freed was not allocated
*** set a breakpoint in malloc_error_break to debug
Abort trap: 6
```

Pretty nasty looking. So I figured I would start by building the samples that came with the ncurses. I cloned [the mirror on github](https://github.com/mirror/ncurses.git) and ran `configure` and then `make all --jobs=8`. Amazingly enough everything worked! Except when I went to run a test program I still got an error, but albeit a prettier error:

```
Error opening terminal: xterm-256color.
```

After trying a few fixes I found on Google it was [this Stack Overflow](https://stackoverflow.com/questions/6804208/nano-error-error-opening-terminal-xterm-256color) post that lead to my fix:

```
export TERMINFO=/usr/share/terminfo
export TERM=xterm-basic
```

After that the sample programs worked! More importatly, **my** program worked!