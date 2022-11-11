---
layout:     post
title:      "How to Write An Assembler in Racket, Part 5: Context-Free Grammars and Parsers"
date:       2022-09-08
excerpt:    "Transform a given context-free grammar to be fit for top-down parsing"
tags:       [programming, 6502, assembler, tutorial, Racket, Wyvern, context-free grammar]
published:  true
comments:   true
---
# DRAFT

[Last time][wyvern04], we revised the [context-free grammar][gfc] for modules and [65C02][6502] architecture. In this post we'll discuss what modifications we must make to them to ensure they are fit for [top-down parsing][tdp]. Our goal here is to transform the existing context-free grammar into a so called $$LL(1)$$ grammar fit for recursive-decent parsing. So let's first see how context-free grammars and parsers interact.

## Context-Free Grammars and Parsers

In the [first post about Wyvern][wyvern01], I shortly touched on the differences between [top-down][tdp] and [buttom-up parsing][bup]. Don't worry, I'm not reiterating that here. Rather, we'll first give some definitions for properties of context-free grammars. These properties will help us determine whether a given grammar can be parsed by an $$LL$$ parser, as we desire. Here's an incomplete list of properties a context-free grammar can have. For a more complete description, see ["Properties of deterministic top-down grammars"](https://www.sciencedirect.com/science/article/pii/S0019995870904468?via%3Dihub). Each property will be annoted whether it is [decidable][decision] or not. This won't really affect our implementation, but it's useful to know if you plan to write your own little grammar tool.

##### Context-Free Grammar
A context-free grammar is a 4-tuple $$G = (V, \Sigma, R, S)$$, where
* $$V$$ is a finite set of *variables* or *nonterminals*
* $$\Sigma$$ is a finite set of *terminals*, this set is disjoint from $$V$$
* $$R$$ is a finite set of relations $$V \times (V \cup \Sigma)^{\ast}$$, where $$^{\ast}$$ is the [Kleene Closure or Kleene star operator][kleene], elements of this set are called *productions* or *rewrite rules*
* $$S$$ is called the *start symbol*, where $$S\in V$$

Context-free grammars are a form of [formal grammar][formal], the key difference being that the head of any rewrite rule may only contain a single nonterminal $$X\in V$$. This restriction is non-trivial. That is, this restriction gives context-free grammars special powers but also limits their applications.

###### Ambiguity
A context-free grammar is **ambiguous** iff for string $$\alpha \in L(G)$$ there are two or more derivations (aka, a context-free grammar is unambiguous if each string's derivation is unique). Undecidable.

###### Productivity
A nonterminal $$X \in V$$ is **productive** iff there is a derivation $$X \; \xRightarrow{\ast} \; \alpha$$ of some string $$\alpha \in \Sigma^{\ast}$$. Decidable.

###### Reachability
A nonterminal $$X \in V$$ is **reachable** if there is a derivation $$S \; \xRightarrow{\ast} \; \alpha X \beta$$, where $$\alpha$$ and $$\beta$$ are strings of nonterminals and terminals, and $$S$$ the start symbol. Decidable.

###### Usability
A nonterminal $$X \in V$$ is **useless** if it is *unproductive* or *unreachable*. Decidable by extension.

###### Nullability
A nonterminal $$X \in V$$ is **nullable** if there is a derivation $$X \; \xRightarrow{\ast} \; \epsilon$$. Decidable.

###### Cyclicity
A derivation $$X \; \xRightarrow{+} \; X$$ is called a **cycle**. Decidable.

###### Left and Right Recursion
A grammar $$G$$ is **left recursive** if it has a nonterminal $$X$$ such that there is a derivation $$X \; \xRightarrow{+} \; X\alpha$$ for any string $$\alpha$$.

A grammar $$G$$ is **right recursive** if it has a nonterminal $$X$$ such that there is a derivation $$X \; \xRightarrow{+} \; \alphaX$$ for any string $$\alpha$$.

The two important properties here are nullable and cyclic. To transform


[wyvern01]: {% post_url wyvern/2022-03-10-wyvern-01 %}
[wyvern02]: {% post_url wyvern/2022-06-02-wyvern-02 %}
[wyvern03]: {% post_url wyvern/2022-06-16-wyvern-03 %}
[wyvern04]: {% post_url wyvern/2022-08-11-wyvern-04 %}
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
[semact]: https://lambda.uta.edu/cse5317/notes/node27.html
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
[projrep]: https://github.com/georgjz/wyvern-assembler
[formal]: https://en.wikipedia.org/wiki/Formal_grammar
[decision]: https://en.wikipedia.org/wiki/Decision_problem