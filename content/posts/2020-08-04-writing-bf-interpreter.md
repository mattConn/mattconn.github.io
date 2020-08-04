---
title: "Writing a Brainf*** Interpreter In C"
date: 2020-08-04T02:59:12-04:00
draft: true
---

An esoteric language is, according to esolangs.org, "a computer programming language designed to experiment with weird ideas, to be hard to program in, or as a joke, rather than for practical use." Brainf*** would fall under this category, as it's hello world program would like this:

`++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.`

Brainf*** is an esoteric language that is very simple and terse. It's crass name makes it kind of hard to talk and write about, but I recently wrote an interpreter for it in C.

# About This Simple Language
This language has only 8 instructions that involve moving about a strip (array) of bytes (ASCII characters) with a single pointer and incrementing/decrementing the byte pointed to.

The operators are:

- **\>** and **<** will increment and decrement memory pointer
- **\+** and **\-** will increment and decrement value pointed to
- **,** and **.**  will input and output value pointed to
- **[** will jump past (over) matching ] if value pointed to is 0
- **]** will jump back to matching [ if value pointed to is not 0 



 and this is what made it so appealing to write an interpreter for. I had tried writing an interpreter for markdown a few years ago, and realized that, while markdown's syntax may be simple, parsing this language is not.

