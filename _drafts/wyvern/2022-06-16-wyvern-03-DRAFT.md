---
layout:     post
title:      "How to Write An Assembler in Racket, Part 3: Context-Free Grammar"
date:       2022-06-16
excerpt:    "We'll look at designing a context-free grammar for the 65C02 instruction set that will serve as a basis for implementing a lexer and parser for the Wyvern assembler"
tags:       [programming, 6502, assembler, tutorial, Racket, Wyvern, context-free grammar]
published:  true
comments:   true
---
# DRAFT

[Last time][wyvern02], we looked at the three main parts of the assembler. Next, we'll look at the [65C02 instruction set architexture][6502], and how to translate a [non-orthogonal ISA][orthogonal] into a [context-free grammar (CFG)][cfg]. The goal is to have a complete description of the 65C02 instruction set. We will then transform and optimize it for a [top-down parsing][tdp] in the next post.

But first, we'll consider some particular problems that arise when we express assembly language as CFGs.

## Context-Free Grammars and Assembly Languages

Translating assembly languages into CFGs poses some particular challenges that rarely occur with higher-level languages. The following list is not exhaustive, as many assembly dialects will come with their own particularities (looking at you, [RCA1802][1802]). There are two points I want to point out before we start designing a 65C02 CFG.

**The [abstract syntax tree][ast] can be harder to define.** This point only applies to Wyvern since I want to support multiple architectures (it is not generally true for single-architecture assemblers is what I'm trying to say). So there will be a "common grammar" of directives (think `.db`, `.subroutine`, `.import`, etc.) that must live next so several "subgrammars" for each supported processor(65C02, Z80, etc.) This will almost certainly result in some kind of [grammar ambiguity][ambi], as different processors define the same addressing mode (more or less) differently, as an example.

The other aspect is that most assembly language parse trees will be rather "flat" - that is, their height will be rather low. The number of possible tree nodes will be higher than with most higher-level languages.

**Non-orthogonal instruction sets.** Most older instruction sets are not orthogonal. That means, not every instruction supports every addressing mode. For example, the 65C02 grammar we will design later down below combines 70 different instructions (or opcodes, if you prefer) with 16 different addressing modes (we'll include the full extensions of the [WD65C02S by Western Design Center][wdc65], which is fully backward compatible with earlier 6502 models). This can quickly bloat the CFG immensely. I'll present a semi-formal approach to this problem inspired by [subset partitions][setpart].

**Whitespace can be context-sensitive.** Handling whitespace correctly in a context-free grammar can be hairy. I hold that it is actually easier to describe a language that *isn't* whitespace-sensitive. Usually, we need to introduce terminals for each relevant whitespace symbol, mostly space, newline, and tab. The [grammar of Python](https://docs.python.org/3/reference/grammar.html) for example defines `NEWLINE`, `INDENT`, and `DEDENT` (among others).

Many assemblers in the past have used whitespace and/or indentation to differentiate labels, opcodes, operands, and comments. Wyvern is no such assembler. The only whitespace symbol Wyvern will care about is newline.

But enough of the prelude, let's get our hands dirty.

## The 65C02 Instruction Set Architecture

For demonstration purposes, I'll start by adding support for the [WD65C02S][wdc65] to Wyvern. It is a well know architecture that is simple and small enough to represent in full here. I plan to do the same for the [Z80][z80] and [68k][68k] in a later post, albeit in a more abridged form. To follow along, I suggest you download the [WD65C02S datasheet][wdcds].

### Notational Convention

Here's a short overview of the notation I'll use in this and future related posts to describe context-free grammars.

A **context-free grammar** is a four-tuple $$G = (V, \Sigma, R, S)$$, where

* $$V$$ is a finite set of *variables* or *nonterminals*
* $$\Sigma$$ is a finite set of *terminals*, this set is disjoint from $$V$$
* $$R$$ is a finite set of relations $$V \times (V \cup \Sigma)\ast$$, where $$\ast$$ is the [Kleene star operator][kleene], elements of this set are called *productions* or *rewrite rules*
* $$S$$ is called the *start symbol*, where $$S\in V$$

**Nonterminals** will be $$CamelCase$$, with the first letter capitalized.

**Terminals** will be $$\T{flatcase}$$, with a block letter style.

**Productions** will be written as

$$
\begin{alignat*}{3}

&Lines              &&\pro &&Identifier \quad \T{colon}  \\
&                   &&\or  &&Mnemonic \quad Register  \\
&                   &&\or  &&Mnemonic \quad Register \quad \T{comma} \quad Number  \\
\\

&Mnemonic           &&\pro &&\T{mov}  \\
&                   &&\or  &&\T{inc}  \\
\\

&Register           &&\pro &&\T{a}  \\
\\

\end{alignat*}
$$

where $$\pro$$ separates the *head* of the production on the left of it from its *body* on the right. Alternatives are denoted with $$\or$$.

There is no special concatenation operator, a space between nonterminals and terminals represents concatenation. There will be an *implicit whitespace consumer* that "eats" all whitespace except newline. Newline is its own terminal denoted as $$\T{eol}$$ (end of line). This may be a bit weird if you've worked with [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) in the past but bear with me. This is the result of whitespace being weird to handle for assembly language dialects mentioned above.

The [Kleene closure][kleene] is a nonterminal that can repeat zero or more times; positive closure is a nonterminal that repeats at least once:

$$
\begin{alignat*}{3}

&KleeneClosure      &&\pro ZeroOrMore^{\ast}  \\
&PostiveClosure     &&\pro AtLeastOnce^{+}  \\
\\

&Nonterminal^{+}    &&\equiv \quad Nonterminal \quad Nonterminal^{\ast}  \\
\\

\end{alignat*}
$$

I use superscript numbers to denote terminals that can be concatenated up to $$n$$ times. So $$Terminal^{3}$$ means that $$Terminal$$ can be concatenated up to three times (including zero).

Finally, to avoid confusion, special symbols like `( ) , [ ]`, etc. are underlined if they're a terminal of the language described:

$$
\begin{alignat*}{3}

&ZPIndexedIndirectX &&\pro \T{\underline{(}} \; ByteExpr \; \T{\underline{,}} \; \T{xreg} \; \T{\underline{)}}  \\
\\

\end{alignat*}
$$

Terminals like $$\T{\underline{(}}$$ and $$\T{lparens}$$ are equivalent and may be used interchangeably where clarity is necessary.

### Representing Addressing Modes

The 65C02 comes with 16 addressing modes, and the common syntax for them has stayed pretty much the same across assemblers and platforms for the last 40+ years:

$$
\begin{alignat*}{3}

&Absolute           &&\pro WordExpr  \\
&AbsIndexedIndirectX&&\pro \underline{\T{(}} \; WordExpr \; \underline{\T{,}} \; \T{xreg} \; \underline{\T{)}}  \\
&AbsIndexedX        &&\pro WordExpr \; \underline{\T{,}} \; \T{xreg}  \\
&AbsIndexedY        &&\pro WordExpr \; \underline{\T{,}} \; \T{yreg}  \\
&AbsIndirect        &&\pro \underline{\T{(}} \; WordExpr \; \underline{\T{)}}  \\
&Accumulator        &&\pro \T{areg}  \\
&Immediate          &&\pro \underline{\T{\#}} ByteExpr  \\
&Implied            &&\pro \epsilon  \\
&PCRelative         &&\pro ByteExpr  \\
&Stack              &&\pro \epsilon  \\
&ZeroPage           &&\pro ByteExpr  \\
&ZPIndexedIndirectX &&\pro \underline{\T{(}} \; ByteExpr \; \underline{\T{,}} \; \T{xreg} \; \underline{\T{)}}  \\
&ZPIndexedX         &&\pro ByteExpr \; \underline{\T{,}} \; \T{xreg}  \\
&ZPIndexedY         &&\pro ByteExpr \; \underline{\T{,}} \; \T{yreg}  \\
&ZPIndirect         &&\pro \underline{\T{(}} \; ByteExpr \; \underline{\T{)}}  \\
&ZPIndirectIndexedY &&\pro \underline{\T{(}} \; ByteExpr \; \underline{\T{)}} \; \underline{\T{,}} \; \T{yreg}  \\
\\

\end{alignat*}
$$

We introduce the two terminals $$WordExpr$$ and $$ByteExpr$$ to represent arithmetic expressions as well as labels, which we will define shortly.

### The Subset of Supported Addressing Modes

Here comes the semiformal/-heuristic approach for creating a correct grammar that combines only valid opcode-addressing mode pairs.

**1: Sort all opcodes by the number of addressing modes they support into sets.** In the case of the WD65C02S, the 70 opcodes support between one and nine addressing modes (none support six or seven addressing modes):

$$
\begin{alignat*}{3}

&\T{mono}           &&\pro &&\text{clc, sec, cli, sei, dey, tya, tay, clv, iny, cld,}  \\
&                          &&\text{inx, sed, txa, tsx, tax, txs, dex, wai, stp, nop,}  \\
&                          &&\text{jsr, brk, php, plp, pha, pla, rti, rts, phy, ply,}  \\
&                          &&\text{phx, plx, rmb, smb, bpl, bmi, bvc, bvs, bra, bcc, bcs, bne, beq, bbr, bbs} \\
&\T{duo}            &&\pro &&\text{trb, tsb} \\
&\T{tri}            &&\pro &&\text{jmp, cpx, cpy, sty, stx} \\
&\T{tetra}          &&\pro &&\text{stz} \\
&\T{penta}          &&\pro &&\text{ldy, bit, lsr, asl, ror, rol, dec, inc, ldx} \\
&\T{octo}           &&\pro &&\text{sta} \\
&\T{ena}            &&\pro &&\text{adc, and, cmp, eor, lda, ora, sbc} \\
\\

\end{alignat*}
$$

I use Greek number names to denote the number of addressing elements *each element* of the seven sets support; these addressing modes don't necessarily have to be the same, we'll fix that in the next step.

**2: For each set from step 1, create a set of addressing modes supported by each opcode (in that set). If an opcode adds a new addressing mode, expand that set in terms of previous sets. Else, give the set a unique new name. Skip opcodes whose addressing mode you have already found.** This is actually the hard part of my approach. That's why I call it "semiformal" since you'll still need to think along a bit. Let's see an example to (hopefully) make this step clear.

It is easiest to start with the $$\T{mono}$$ set, aka, the opcodes that only support one addressing mode. Using the [datasheet][wdcds], we find that the first opcode `clc` supports only $$Implied$$ addressing, since we have not seen it yet, we create a new set called $$Alpha$$ and add $$Implied$$ to it. Some of the following opcodes `sec`, `cli`, `tya`, etc. all support implied addressing, so we can eliminate them. When we come across `jsr`, for example, we add a new set $$Beta$$ with $$Absolute$$ in it.

If we repeat this for all elements/opcodes in $$\T{mono}$$ we get the following sets:

$$
\begin{alignat*}{3}

&Alpha              &&\pro Implied  \\
&Beta               &&\pro Absolute  \\
&Gamma              &&\pro Stack  \\
&Delta              &&\pro ZeroPage  \\
&Epsilon            &&\pro PCRelative  \\
\\

\end{alignat*}
$$

Next, we move on to the $$\T{duo}$$ group of instructions. Both `trb` and `tsb` support $$Absolute$$ and $$ZeroPage$$ addressing. Since we already have sets that include those, we can express them in terms of those existing sets:

$$
\begin{alignat*}{3}

&Zeta               &&\pro Beta &&\or Delta  \\
\\

\end{alignat*}
$$

Now rinse and repeat for each set, which will result in this final set partition of the addressing modes:

$$
\begin{alignat*}{3}

&Alpha              &&\pro Implied  \\
&Beta               &&\pro Absolute  \\
&Gamma              &&\pro Stack  \\
&Delta              &&\pro ZeroPage  \\
&Epsilon            &&\pro PCRelative  \\
&Zeta               &&\pro Beta &&\or Delta  \\
&Eta                &&\pro AbsIndirect &&\or AbsIndexedIndirectX \or Beta  \\
&Theta              &&\pro Immediate &&\or Zeta  \\
&Iota               &&\pro ZPIndexedX &&\or Zeta  \\
&Kappa              &&\pro ZPIndexedY &&\or Zeta  \\
&Lambda             &&\pro AbsIndexedX &&\or Iota  \\
&Mu                 &&\pro Immediate &&\or Lambda  \\
&Nu                 &&\pro Accumulator &&\or Lambda  \\
&Xi                 &&\pro Immediate &&\or Kappa  \\
&Omicron            &&\pro AbsIndexedY &&\or ZPIndexedIndirectX \or ZPIndirect \or ZPIndirectIndexedY \or Lambda  \\
&Pi                 &&\pro Immediate &&\or Omicron  \\
\\

\end{alignat*}
$$

Here you can also see why you still need to think along a bit: $$Iota$$ and $$Kappa$$ are necessary because the index registers of the 65C02 cannot operate on themselves. This is a quirk specific to that architecture. When you create your own grammar, keep these quirks in mind. (The other reason is that the sets above overlap, aka, they're not disjoint - as is required by set partitions.)

**3: Create set partitions of the sets from step 1 based on the addressing mode sets from step 2.** This basically means, sorting opcodes by the addressing modes they support:

$$
\begin{alignat*}{2}

&\T{monoalpha}      &&\pro \text{clc, sec, cli, sei, dey, tya, tay, clv, iny, cld, inx, sed, txa, tsx, tax, txs, dex, wai, stp, nop} \\
&\T{monobeta}       &&\pro \text{jsr} \\
&\T{monogamma}      &&\pro \text{brk, php, plp, pha, pla, rti, rts, phy, ply, phx, plx} \\
&\T{monodelta}      &&\pro \text{rmb, smb} \\
&\T{monoepsilon}    &&\pro \text{bpl, bmi, bvc, bvs, bra, bcc, bcs, bne, beq, bbr, bbs} \\
&\T{duozeta}        &&\pro \text{trb, tsb} \\
&\T{trieta}         &&\pro \text{jmp} \\
&\T{tritheta}       &&\pro \text{cpx, cpy} \\
&\T{triiota}        &&\pro \text{sty} \\
&\T{trikappa}       &&\pro \text{stx} \\
&\T{tetralambda}    &&\pro \text{stz} \\
&\T{pentamu}        &&\pro \text{ldy,  bit} \\
&\T{pentanu}        &&\pro \text{lsr, asl, ror, rol, dec, inc} \\
&\T{pentaxi}        &&\pro \text{ldx} \\
&\T{octoomicron}    &&\pro \text{sta} \\
&\T{enapi}          &&\pro \text{adc, and, cmp, eor, lda, ora, sbc} \\
\\

\end{alignat*}
$$

Now you in essence have sorted all opcodes in sets of opcodes that support the exact same set of addressing modes.

**4: Combine the sets from steps 2 and 3 into a grammar that produces all valid opcode-addressing mode pairs.** Don't forget to add a start symbol:

$$
\begin{alignat*}{3}

&Instr              &&\pro &&MonoInstr  \\
&                   &&\or  &&DuoInstr  \\
&                   &&\or  &&TriInstr  \\
&                   &&\or  &&PentaInstr  \\
&                   &&\or  &&OctoInstr  \\
&                   &&\or  &&EnaInstr  \\
\\

&MonoInstr          &&\pro &&\T{monoalpha} \quad Alpha  \\
&                   &&\or  &&\T{monobeta} \quad Beta  \\
&                   &&\or  &&\T{monogamma} \quad Gamma  \\
&                   &&\or  &&\T{monodelta} \quad Delta  \\
&                   &&\or  &&\T{monoepsilon} \quad Epsilon  \\
\\

&DuoInstr           &&\pro &&\T{duozeta} \quad Zeta  \\
\\
&TriInstr           &&\pro &&\T{trieta} \quad Eta  \\
&                   &&\or  &&\T{tritheta} \quad Theta  \\
&                   &&\or  &&\T{triiota} \quad Iota  \\
&                   &&\or  &&\T{trikappa} \quad Kappa  \\
\\

&TetraInstr         &&\pro &&\T{tetralambda} \quad Lambda  \\
\\

&PentaInstr         &&\pro &&\T{pentamu} \quad Mu  \\
&                   &&\or  &&\T{pentanu} \quad Nu  \\
&                   &&\or  &&\T{pentaxi} \quad Xi  \\
\\

&OctoInstr          &&\pro &&\T{octoomicron} \quad Omicron  \\
\\

&EnaInstr           &&\pro &&\T{enapi} \quad Pi  \\
\\

\end{alignat*}
$$

That's it, now you have a grammar that can correctly derive legal opcode-addressing mode pairs. Remember that we'll take care of whitespace separately. For now, assume that the assembler will recognize only one $$Instr$$ per line.

## Assembler Directives and Features

Now that we have a basic CFG for 65C02 code, we need to add some common features found in assemblers. We'll start with numbers and arithmetic expressions.

### Numbers

Wyvern will support four number notations: [hexadecimal][hexadecimal], [decimal][decimal], [octal][octal], and [binary][binary].

Here are a few examples:

```plaintext
$beef           hex with prefix
feebh           hex with postfix
1357            decimal
246o            octal with postfix
%1100           binary with prefix
0011b           binary with postfix
```

This results in this rather simple number CFG:

$$
\begin{alignat*}{3}

&Number             &&\pro &&HexNumber  \\
&                   &&\or  &&DecNumber  \\
&                   &&\or  &&OctNumber  \\
&                   &&\or  &&BinNumber  \\
\\

&HexNumber          &&\pro &&\T{hexprefix} \quad HexDigit^+   \\
&                   &&\or  &&HexDigit^{+} \quad \T{hexpostfix} \\
\\

&DecNumber          &&\pro &&Digit^{+} \\
\\

&OctNumber          &&\pro &&OctDigit^{+} \quad \T{octpostfix}  \\
\\

&BinNumber          &&\pro &&\T{binprefix} \quad BinDigit^{+}  \\
&                   &&\or  &&BinDigit^{+} \quad \T{binpostfix}  \\
\\

&HexDigit           &&\pro &&\T{[a-fA-F]} \or Digit  \\

&Digit              &&\pro &&\T{9} \or \T{8} \or OctDigit  \\

&OctDigit           &&\pro &&\T{7} \or \T{6} \or \T{5} \or \T{4} \or \T{3} \or \T{2} \or BinDigit  \\

&BinDigit           &&\pro &&\T{1} \or \T{0}  \\
\\

\end{alignat*}
$$

Separating pre- and postfixes into their own terminals ($$hexprefix$$, $$hexpostfix$$, $$octpostfix$$, $$binprefix$$, $$binpostfix$$) makes it easier to expand the number system later on.

Most assemblers support arithmetic expressions and so will Wyvern. CFGs for arithmetic expressions are pretty well understood, so I'm not going to elaborate here on how to enforce operator precedence. If you need a refresher, [here's a good overview](https://pages.cs.wisc.edu/~fischer/cs536.s08/course.hold/html/NOTES/3.CFG.html).

$$
\begin{alignat*}{3}

&AExpr              &&\pro &&Expr \\
\\

&Expr               &&\pro &&Expr \quad \T{+} \quad Term  \\
&                   &&\or  &&Expr \quad \T{-} \quad Term  \\
&                   &&\or  &&Term  \\
\\

&Term               &&\pro &&Term \quad \T{*} \quad Factor  \\
&                   &&\or  &&Term \quad \T{/} \quad Factor  \\
&                   &&\or  &&Factor  \\
\\

&Factor             &&\pro &&\T{\underline{(}} \quad Expr \quad \T{\underline{)}}  \\
&                   &&\or  &&\T{-} Factor  \\
&                   &&\or  &&Number  \\
&                   &&\or  &&\T{id}  \\
\\

\end{alignat*}
$$

You may have noticed that I don't really define terminals yet - that's on purpose. Terminals are easier to express in [regular expressions][re]. We'll use them in a later post when we start designing the lexer. Here, we focus on the context-free grammar that binds all parts of Wyvern together.

## Module Structure

This section describes the [module structure of Wyvern][wyvern01]. Modules will be the basic translation units of Wyvern, so all code must reside in a module. The text of this section will also include a lot of semantic information. Context-free grammars are not good at encoding semantic information and rules, we will use additional notational tools to do that in the next post. But for now, let's define what a module is.

A module resides in a single file. It has a module header and a code block. If the export list is omitted, all symbols from the module are exported (NB! This is a good example of semantic information that is hard to encode in CFG notation. As is the following:) If the parentheses are empty, no symbols are exported.

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

A code block includes import statements, directives, and code. All lines may contain one label and an optional comment.

$$
\begin{alignat*}{3}

&CodeBlock          &&\pro &&ImportStmt^{*} \quad CodeLine^{*}  \\
\\

&CodeLine           &&\pro &&Label^{1} \quad (Directive \or Instr)^{1} \quad Comment^{1}  \\
\\

&Comment            &&\pro &&\T{commentop} \quad \T{anychar}  \\
\\

\end{alignat*}
$$

Legal directives are Value aliases; data definitions; binary file imports; macros.

Data definitions allow for value aliases and "hardcoded" binary data.

$$
\begin{alignat*}{3}

&Directive          &&\pro &&DataDefinition \quad \T{eol} \\
&                   &&\or  &&Subroutine  \\
&                   &&\or  &&BinaryImport  \\
&                   &&\or  &&Macro  \\
\\

&DataDefinition     &&\pro &&Identifier \quad AssignOp \quad Number  \\
&                   &&\or  &&DataDeclarator \quad NumberList  \\
\\

&DataDeclarator     &&\pro &&\T{bytedecl}  \\
&                   &&\or  &&\T{worddecl}  \\
&                   &&\or  &&\T{longdecl}  \\
\\

&NumberList         &&\pro &&Number  \\
&                   &&\or  &&Number \quad \T{,} \quad NumberList  \\
\\

\end{alignat*}
$$

Subroutines can define local labels; these are only in scope for the subroutine itself.

$$
\begin{alignat*}{3}

&Subroutine         &&\pro &&\T{subroutinekeyword} \quad Identifier \quad \T{colon} \quad \T{eol}  \\
&                   &&     &&LabeledLine^{*}  \\
&                   &&     &&\T{endsubroutinekeyword} \quad \T{eol}  \\
\\

&LabeledLine        &&\pro &&Label^{1} \quad Instr \quad Comment^{1} \quad \T{eol}  \\
\\

\end{alignat*}
$$

Subroutines can define local labels; these are only in scope for the subroutine itself.

$$
\begin{alignat*}{3}

&Subroutine         &&\pro &&\T{subroutinekeyword} \quad Identifier \quad \T{colon} \quad \T{eol}  \\
&                   &&     &&LabeledLine^{*}  \\
&                   &&     &&\T{endsubroutinekeyword} \quad \T{eol}  \\
\\

&LabeledLine        &&\pro &&Label^{1} \quad Instr \quad Comment^{1} \quad \T{eol}  \\
\\

\end{alignat*}
$$

Binary imports allow for binary data import (duh.) A filename is an identifier enclosed in double quotes.

$$
\begin{alignat*}{3}

&BinaryImport       &&\pro &&\T{binimportop} \quad Filename \quad \T{eol}  \\
\\

&Filename           &&\pro &&\T{legal filename enclosed in double quotes}  \\
\\

\end{alignat*}
$$

The observant reader will once again notice that whitespace is handled rather nonchalantly here. At this stage, I'm interested in a rather rough outline. But we've actually defined enough of Wyvern's grammar that we can start refining it.

## Conclusion

We've created a first draft of the context-free grammar we'll use to implement a lexer and a parser for Wyvern. As of now, the only supported architecture is 65C02. Next time, we'll add proper definitions for the terminals in form of regular expressions, and refine the context-free grammar to ensure it isn't ambiguous.


[wyvern01]: {% post_url wyvern/2022-03-10-wyvern-01 %}
[wyvern02]: {% post_url wyvern/2022-06-02-wyvern-02 %}
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
