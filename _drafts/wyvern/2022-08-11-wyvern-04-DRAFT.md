---
layout:     post
title:      "How to Write An Assembler in Racket, Part 4: Refining and Completing The Context-Free Grammar"
date:       2022-08-11
excerpt:    "The context-free grammar for the Wyvern assembler needs refinement and completion before it can be transformed for parser and lexer generation"
tags:       [programming, 6502, assembler, tutorial, Racket, Wyvern, context-free grammar]
published:  true
comments:   true
---
# DRAFT

[Last time][wyvern03], we designed a rough context-free grammar (CFG) for the Wyvern assembler. In this article, we'll refine and complete it. This includes dealing with whitespace correctly. The resulting CFG will almost be ready for implementation.

## Fixing Whitespace

Let's start by fixing the whitespace issue. That is, we need to make sure that our grammar/parser will correctly recognize newline in all situations. We'll use a top-down approach, so let's start at the highest level, the module:

$$
\begin{alignat*}{3}

&Module             &&\pro &&ModuleHeader \quad CodeBlock \quad \T{eof}  \\
\\

&ModuleHeader       &&\pro &&\T{module} \quad Identifier \quad ExportList \quad \T{eol}  \\
\\

&ExportList         &&\pro &&\T{lparen} \quad IdList \quad \T{rparen}  \\
&                   &&\or  &&\epsilon  \\
\\

&IdList             &&\pro &&Identifier  \\
&                   &&\or  &&Identifier \quad \T{,} \quad IdList  \\
&                   &&\or  &&\epsilon  \\
\\

\end{alignat*}
$$

Problems with assembly languages:

* AST is harder to define
* Non-orthogonal instruction sets
* Assemblers are limited, because they never embraced structured programming. The high-level


[wyvern01]: {% post_url wyvern/2022-03-10-wyvern-01 %}
[wyvern02]: {% post_url wyvern/2022-06-02-wyvern-02 %}
[wyvern03]: {% post_url wyvern/2022-06-16-wyvern-03 %}
[z80]: https://en.wikipedia.org/wiki/Zilog_Z80
[6502]: https://en.wikipedia.org/wiki/MOS_Technology_6502
[68k]: https://en.wikipedia.org/wiki/Motorola_68000
[6809]: https://en.wikipedia.org/wiki/Motorola_6809
[1802]: https://en.wikipedia.org/wiki/RCA_1802
[elf]: https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
[elfhs]: https://hackage.haskell.org/package/elf-0.31/docs/Data-Elf.html
[lex]: https://en.wikipedia.org/wiki/Lexical_analysis
[ast]: https://en.wikipedia.org/wiki/Abstract_syntax_tree
[syn]: https://en.wikipedia.org/wiki/Parsing
[cfg]: https://en.wikipedia.org/wiki/Context-free_grammar
[sem]: https://en.wikipedia.org/wiki/Compiler#Front_end
[tdp]: https://en.wikipedia.org/wiki/Top-down_parsing
[bup]: https://en.wikipedia.org/wiki/Bottom-up_parsing
[orthogonal]: https://en.wikipedia.org/wiki/Orthogonal_instruction_set
[setpart]: https://en.wikipedia.org/wiki/Partition_of_a_set
[ambi]: https://en.wikipedia.org/wiki/Ambiguous_grammar
[wdc65]: https://www.westerndesigncenter.com/wdc/w65c02s-chip.php
[wdcds]: https://www.westerndesigncenter.com/wdc/documentation/w65c02s.pdf
[kleene]: https://en.wikipedia.org/wiki/Kleene_star
[hexadecimal]: https://en.wikipedia.org/wiki/Hexadecimal
[decimal]: https://en.wikipedia.org/wiki/Decimal
[octal]: https://en.wikipedia.org/wiki/Octal
[binary]: https://en.wikipedia.org/wiki/Binary_number
[re]: https://en.wikipedia.org/wiki/Regular_expression