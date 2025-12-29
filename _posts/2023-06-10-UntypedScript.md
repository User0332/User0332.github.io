---
title: UntypedScript
date: 2023-06-10
categories: ["Projects"]
tags: ["Featured Projects"]
---

[UntypedScript](https://github.com/User0332/UntypedScript) is a project that branched off of the discontinued [PogScript](https://github.com/User0332/PogScript), with a considerably different JavaScript-like syntax. UntypedScript's goal is to become an easy-to-understand and easy-to-write language with speeds comparable to that of C or C++.

The default implementation compiler generates x86, which is assembled/linked by NASM/ld in the default toolchain. As per the name, includes no form of a type system - you are on your own! Type hints may be introduced in the future. 

# Sample "Hello, World!"

```js
import puts from "<libc>"

const main = () => {
	puts("Hello World!")

	return 0
}

export { main }
```

Compile it with `utsc -e helloworld.uts` and run `helloworld.exe`, or just run `utsc -r helloworld.uts`! (You need NASM and MinGW installed - currently only tested for Windows)

# More Examples!

UntypedScript supports structs, namespaces, runtime-mutable dynamic objects, arbitrary stack allocation, direct AST modification, threading, dynamically allocated functions, three types of local functions, and more! You can find examples for all of these on [GitHub](https://github.com/User0332/UntypedScript/tree/master/tests/completed)!

You can also learn more about the language on the [docs](https://untypedscript.readthedocs.io/)!