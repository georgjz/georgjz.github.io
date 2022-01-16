---
layout: post
title: "My new grammar package for Atom"
date: 2018-07-23
excerpt: "Learn about a new Atom grammar package for the Intel 8080"
tags: [programming, atom, intel 8080, project ideas, 6502]
comments: true
---

[Atom][1] is one of my favorite tools. I use it for pretty much
for anything that involves coding or writing(I'm writing this in Atom right
now).

There are thousands of packages for Atom to expand its functionality. Atom is
designed to be hackable by everyone to mold it to their own needs. So far, I
have written three packages for Atom: They add syntax highlighting for the
[Motorola 6809 processor][2], the [ARMIPS assembler][3], and my newest for the
[Intel 8080 processor][4].

<figure>
    <a href="https://upload.wikimedia.org/wikipedia/commons/3/3a/KL_Intel_i8080_Black_Background.jpg">
        <img src="https://upload.wikimedia.org/wikipedia/commons/3/3a/KL_Intel_i8080_Black_Background.jpg">
    </a>
    <figcaption>
        <a href="https://commons.wikimedia.org/wiki/File:KL_Intel_i8080_Black_Background.jpg">Photo</a> of a Intel 8080 by Konstantin Lanzet, Licensed under <a href="https://creativecommons.org/licenses/by-sa/3.0/deed.en">CC-BY-SA 3.0</a>
    </figcaption>
</figure>

I have a knack for assembly programming on old computers and hardware.
Recently, I found an [Altair 8800 emulator][5], downloaded the manual and
started writing code for it. Hand assembling code on paper and entering it via
toggle switches might sound crazy, but the result is very satisfying.

<figure>
    <a href="https://upload.wikimedia.org/wikipedia/commons/8/8b/CPU_Altair_8800.jpg">
        <img src="https://upload.wikimedia.org/wikipedia/commons/8/8b/CPU_Altair_8800.jpg">
    </a>
    <figcaption>
        <a href="https://commons.wikimedia.org/wiki/File:CPU_Altair_8800.jpg">Photo</a> of a Altair 8800 by Stahlkocher, Licensed under <a href="https://creativecommons.org/licenses/by-sa/3.0/deed.en">CC-BY-SA 3.0</a>
    </figcaption>
</figure>

I even got a [KIM Uno][6], a clone of the [KIM-1][7](also seen as background
image of this blog), another famous computer from the same time. And it uses
the [6502][8], a very successful processor used in many later home computers
and the NES. There is an [Altair 8800 clone][9] that looks fun, I might get
one of those too.

But writing and translating all code by hand on paper can get tiring. So I
thought about writing a package for Atom to take some of that work off me.
There was no grammar package that added syntax highlighting for Intel 8080
assembly to Atom, so I wrote this one to have that.

I hope to expand this into some kind of a live assembler. The idea is to write
the code in Atom and have it translated into machine code live by the editor as
you type. I imagine it to look like the listing outputs some assemblers
generate:

<figure>
    <a href="/assets/contents/liveasmmock.png">
        <img src="/assets/contents/liveasmmock.png">
    </a>
    <figcaption>
        Think of this as a mock up
    </figcaption>
</figure>


I'm not really fluent in JavaScript and the whole Node.js ecology, so this
seems like a nice project to learn more about Atom, Electron and all that jazz.

[1]: https://atom.io
[2]: https://atom.io/packages/language-6809
[3]: https://atom.io/packages/language-armips
[4]: https://atom.io/packages/language-armips
[5]: https://s2js.com/altair/
[6]: http://obsolescence.wixsite.com/obsolescence/kim-uno-summary-c1uuh
[7]: https://en.wikipedia.org/wiki/KIM-1
[8]: https://en.wikipedia.org/wiki/MOS_Technology_6502
[9]: https://www.altairduino.com
[10]: https://commons.wikimedia.org/wiki/File:KL_Intel_i8080_Black_Background.jpg
[11]: https://creativecommons.org/licenses/by-sa/3.0/deed.en
[12]: https://commons.wikimedia.org/wiki/File:CPU_Altair_8800.jpg
