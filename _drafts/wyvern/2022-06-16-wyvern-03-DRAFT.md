---
layout:     post
title:      "How to Write An Assembler in Racket, Part 3: Context-Free Grammar"
date:       2022-06-16
excerpt:    "We'll look at designing and optimizing a context-free grammar for the 65C02 instruction set that will serve as basis for implementing a lexer and parser for the Wyvern assembler"
tags:       [programming, 6502, assembler, tutorial, Racket, Wyvern]
published:  true
comments:   true
---
# DRAFT

## Context-Free Grammars and Assembly Languages

Problems with assembly languages:

* AST is harder to define
* Non-orthogonal instruction sets
* Assemblers are limited, because they never embraced structured programming. The high-level


[wyvern01]: {% post_url wyvern/2022-03-10-wyvern-01 %}
[z80]: https://en.wikipedia.org/wiki/Zilog_Z80
[6502]: https://en.wikipedia.org/wiki/MOS_Technology_6502
[68k]: https://en.wikipedia.org/wiki/Motorola_68000
[6809]: https://en.wikipedia.org/wiki/Motorola_6809
[elf]: https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
[elfhs]: https://hackage.haskell.org/package/elf-0.31/docs/Data-Elf.html
[lex]: https://en.wikipedia.org/wiki/Lexical_analysis
[ast]: https://en.wikipedia.org/wiki/Abstract_syntax_tree
[syn]: https://en.wikipedia.org/wiki/Parsing
[cfg]: https://en.wikipedia.org/wiki/Context-free_grammar
[sem]: https://en.wikipedia.org/wiki/Compiler#Front_end
[tdp]: https://en.wikipedia.org/wiki/Top-down_parsing
[bup]: https://en.wikipedia.org/wiki/Bottom-up_parsing
