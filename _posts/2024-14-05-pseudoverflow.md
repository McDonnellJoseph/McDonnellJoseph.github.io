---
title: '404-CTF-2024 Write Ups: Pseudoverflow'
date: 2024-05-14
permalink: /posts/2024/01/404-writeup-1/
tags:
  - pwn
  - 404CTF
  - buffer overflow
---

Pseudoverflow
=============

Pseudoverflow is an introduction level binary exploitation challenge from the 2024 edition of the [404 CTF](https://www.404ctf.fr/).

This is a sample blog post. Lorem ipsum I can't remember the rest of lorem ipsum and don't have an internet connection right now. Testing testing testing this blog post. Blog posts are cool.

Pre-requisites:
---------------

This is an excellent introduction challenge to the pwn category but you'll still need to install a bit of tooling to get started:
  - A python environment with [pwntools](https://github.com/Gallopsled/pwntools).
  - Access to decompiler (this is what reverse engineers machine code to C) either [online](https://dogbolt.org/) or locally. I personnally use [ghidra](https://github.com/NationalSecurityAgency/ghidra).
  - GDB debugger set-up. You can also extend GDB functionnalities with [gef](https://github.com/hugsy/gef) to make the long hours searching through the debugger to find the light more enjoyable.

Now we're all set let's get to it!

First Steps:
-------------

Let's start by learning more about our binary and checking what protections are enabled. 

```console
$ file course
course: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=74c3be86c1c3aa9eaa680f3a81f6925ed3ec0fd6, for GNU/Linux 4.4.0, not stripped

$ pwn checksec course
Arch:     amd64-64-little
RELRO:    Partial RELRO
Stack:    No canary found
NX:       NX enabled
PIE:      PIE enabled
```
The `file` command tells us we are working with *64-bit* binary this will be important later when crafting our malicious payload. 
The `checksec` shows which security protectiosn have been deactivated at compile time. In our case the stack is canary free 🐤. Canary is a simple but effective way of protecting a program from buffer overflows. Combining this information with the name of the challenge we deduce that we're aiming for a buffer overflow. 

With this in mind we can run the program:

```console
$ chmod +x course
$ ./course
```

You can have many headings
======

Aren't headings cool?
------
