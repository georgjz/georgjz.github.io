---
layout:     post
title:      "How to Write An Assembler in Haskell, Part 2"
date:       2020-01-09
excerpt:    "This explains the motivation for writing a new Z80 assembler in the 20th century"
tags:       [programming, Z80, assembler, tutorial, Haskell]
feature:    /assets/snesaa/02/saa02_featurecard.gif
published:  false
comments:   true
---
# DRAFT

### Functionality Overview

Last time, I wrote about my motivation to create a new assembly toolchain for crossdevelopment targeting

Problems with assembly languages:

* AST is harder to define
* Non-orthogonal instruction sets
* Assemblers are limited, because they ever embraced structured programming. The high-level
