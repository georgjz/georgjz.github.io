---
layout:     post
title:      "How to Write An Assembler in Racket, Part 3"
date:       2022-06-16
excerpt:    "Takes a look at the unique challenges of describing an assembly language as context-free grammar"
tags:       [programming, 65C02, assembler, tutorial, Haskell, context-free grammar]
published:  true
comments:   true
---
# DRAFT

## Context-Free Grammars and Assembly Languages

Problems with assembly languages:

* AST is harder to define
* Non-orthogonal instruction sets
* Assemblers are limited, because they never embraced structured programming. The high-level

[wyvern01]: https://cc65.github.io/doc/
[z80]: https://cc65.github.io/doc/
[6502]: https://cc65.github.io/doc/
[68k]: https://cc65.github.io/doc/
[6809]: https://cc65.github.io/doc/
