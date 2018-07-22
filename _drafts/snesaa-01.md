---
layout:     post
title:      "SNES Assembly Adventure 01: Develop for the SNES"
date:       2018-07-22
excerpt:    "Learn how to get started in writing your own game for the SNES by setting up a development environment"
tags:       [test, assembly, programming]
comments:   false
---
# DRAFT

This is the first of several articles on developing games for the Super Nintendo Entertainment System. Retro Gaming has been strong in the past years, with hundreds of games released. Most aim to re-create the graphic and music style of so-called 8- or 16-bit consoles like The SNES or Sega Genesis/Mega Drive. But how were those classics made that so many of today's games try to emulate? The aim of these articles is to show you the steps for programming your own game for the SNES. We will cover all topics necessary for writing a SNES game from the beginning to the end. This article will kick-start your project by helping you to set up a development environment. Read on to start your own SNES Assembly Adventure.

There are a few tutorials on programming the SNES on the web, so why write another one? The honest answer is: I need content for my tumblr. I'm not an awesome illustrator or artist, so I have to resort to what I'm good at, assembly programming. Hence, I will write about programming in assembly for the SNES. Many game devs use tumblr for promoting their indie projects or people showing their (pixel) art and mockups. I hope I can show you how to turn it into an actual SNES game. Not a 16-bit style game, an actual SNES 16-bit game that runs to the console as well as in an emulator.

### Who is this for?

I will assume that you have no prior experience in assembly programming. Also, you need no to little knowledge how a CPU works as we will cover this. But you should be familiar with some basic higher level programming concepts like functions, variables, pointers, etc. If you have a general idea what those terms mean, you will be able to follow this series. Moreover, you should be familiar with your system's command line and know how to invoke programs with it. Finally, a basic understanding of the binary and hexadecimal numbering system is necessary - but I will give you a quick refresher in the next article.

### Content

Before we start, I want to outline the contents I will cover in these articles. In this article we start with setting up a development environment; your playground to create SNES games. The next couple of articles will introduce you to the 65816 microprocessor and how to program it. Then we move on to the architecture of the SNES. Understanding the inner workings of the SNES is important to programming games for it. Once we have cleared the basics, I plan to continue with a guided game project. I will design and program a simple game clone for you to follow along. That includes topics that go beyond basic programming. Things like sprite animation, collision detection, and input handling will be covered here. And finally, I want to show you how to design and produce your own PCB and cartridge to run your game on an actual SNES. To summarize, these articles will guide you through the complete process of creating your own SNES game.

I'm going to use tools that run on all major platforms and are free of charge. So you should be able to follow this articles whether you're on Linux, Windows or OS X.

My goal is to keep these articles informative but short enough to be worked through within one hour(or likely less). I will try to keep each article as self-contained as possible. So people with some experience in program NES, SNES, or other consoles may learn a trick or two. I aim to post a new article each weekend, but this may vary depending on the topic and scope. At the end of each article, you will find a list of references and links. They provide additional resources and pieces of information on topics covered in the article. If you have questions or suggestions, I'm always ready to help and welcome feedback. Use the AMA function or drop me a message.

The goal of this first article is simple: Set up a development environment and program your first SNES game(albeit a very, <em>very</em> boring one). Don't worry if you don't understand everything yet. I plan to build up knowledge gradually throughout the articles.

So, let's embark on an Assembly Adventure!

[EMBARK IMAGE]

For developing and testing SNES game, we need an array of tools. These are the four basic tools: A text editor, an assembler(and linker), an emulator, and git. Let's have closer look at them.

Programming without a proper <strong>text editor</strong> to edit source code is an exercise in futility. If you've written code or programmed before, you probably have a preferred editor. For this series of articles, I will use the [Atom Editor][atom_link]. Atom is an open source editor that supports a vast range of languages. It has thousands of plugins/packages to expand the basic functionalities of the editor. Later I will show you how to install Atom and the packages I use. Other good editors are [Visual Studio Code][vsc_link], [Sublime Text][sublime_link] (costs $$$), and [Vim][vim_link]. I prefer slim and fast editors over full-blown IDEs, but that is up to you. As long as you know how to edit source files with it, you will be fine.

Our second essential tool is an **assembler(and linker)**. The assembler translates our source files into machine code that runs on the SNES. I will explain this process in-depth in future articles and how it relates to high-level languages like C/C++. Terms like “machine code” will be explained. We will use the cc65 toolchain to translate our code into machine code the SNES can execute. Several other assemblers for 6502/65816 exist, some geared towards NES and SNES development. But I prefer cc65 because it offers higher flexibility than other assemblers.

Third, we need a **SNES emulator** to test our code. My emulator of choice is bsnes+. It offers some extra functionality useful for debugging code. I will show you how to use the bsnes+ debugger to find errors in your code. Other emulators like [no$sns][nocash_link] or [9snesx][snesx_link] offer good debugging capabilities too. But bsnes+ works on all three major platforms without a hassle, so we'll use it.

Lastly, we need a source code management tool, **git**. We use git to download the source code for building some of the tools mentioned above. Git is a great tool for developers. You can find a lot of resources on the web about [using git][gitbook_link].

These are the basic tools we need for now. Later we will add some other tools for creating graphics, music, hex editor, etc.

Let's finally get started and install the tools.

### Installing the tools

To make it easier to follow, I will use three icons to mark which instructions you should follow on your platform:

<table style="width:50%" align="center">
  <tr>
    <td align="center"><i class="fa fa-linux"></i></td>
    <td align="center"><i class="fa fa-windows"></i></td>
    <td align="center"><i class="fa fa-apple"></i></td>
    <th></th>
  </tr>
  <tr>
    <td align="center">
        <span style="font-weight:bold">Linux</span>
    </td>
    <td align="center">
        <span style="font-weight:bold">Windows</span>
    </td>
    <td align="center">
        <span style="font-weight:bold">OS X</span>
    </td>
  </tr>
</table>

**Important note:** There are multiple versions of bsnes. You will need to install [bsnes+][bsnes_link](sometimes called bsnes-plus), bsnes+ adds debugging capabilities to bsnes, which we will use in later articles.

Windows users mind that Linux and OS X use forward slashes `/` instead of backslashes `\` in directory path names.

#### Installation Shortcuts

Experienced users can skip the in-detail installation instructions and use these shortcuts instead.

<i class="fa fa-linux"></i> Linux users use their distribution's package managers to install the following packages

<pre>cc65 atom git bsnes-plus</pre>

<p>Atom is optional if you already have a text editor you use for programming. Git is probably already installed. The cc65 toolchain includes the assembler and linker we need to write SNES games. Mind that the exact package names may vary across distributions, make sure you get the correct ones.</p>

<p><i class="fa fa-windows"></i> Windows users need to install these programs:</p>

* [git](https://git-scm.com/downloads)
* [Atom](https://atom.io)
* [cc65](http://sourceforge.net/projects/cc65/files/cc65-snapshot-win32.zip)
* [mingw-w64](http://mingw-w64.org/doku.php)

[Remember to add cc65 to your PATH](https://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/). Alternatively, build cc65 from source as described [here](https://github.com/cc65/wiki/wiki/Microsoft-Windows).

You can skip Atom if you have a preferred text editor.

*You will still need to follow the in-detail instructions to install the bsnes+ emulator below since it must be built from source.*

<i class="fa fa-apple"></i> OS X users need to install the Xcode command line tools first. Open a terminal and enter

<pre>$ xcode-select --install</pre>

This will install among other tools a C++ compiler and git, which you need to build the bsnes+ emulator.

Next, install **Brew**. [Homebrew](https://brew.sh) is a Linux-style package manager that makes installations much easier. Open the link in the last sentence and execute the command in a terminal.

Once it is installed, use brew to install the following packages. Open a terminal and enter

<pre>$ brew install cc65</pre>

This will install the cc65 toolchain. Next, install <a href="https://atom.io">Atom</a> if you don't have a text editor for programming yet.

*For building the bsnes+ emulator, please refer to the in-detail installation instructions below.*

<i class="fa fa-linux"></i> <i class="fa fa-windows"></i> <i class="fa fa-apple"></i> Check your installation by opening a terminal and running these commands:

<pre>$ git --version<br>$ cc65 --version<br>$ bsnes</pre>

If you decide to use Atom, read the in-detail installation instructions for suggested additional packages that will enhance the editor. If you get stuck, try the in-detail installation instructions below or send me a message. The linked homepages for each tool may be helpful too.

#### Detailed Installation Instructions

Start by installing **git**.

<i class="fa fa-linux"></i> Most Linux distributions ship with git installed. Else, use your distribution's package manager to install git. For Debian, Ubuntu and the like open a terminal and type

<pre>$ apt-get install git</pre>

<i class="fa fa-windows"></i> Windows users download the installer and execute it.

<i class="fa fa-apple"></i> For OS X users the simplest way to install git is to install the Xcode command line tool as described above. Open a terminal and type the command

<pre>$ xcode-select --install</pre>

This will install a C++ toolchain you need to build bsnes+.

<i class="fa fa-linux"></i> <i class="fa fa-windows"></i> <i class="fa fa-apple"></i> Test your git installation by opening a terminal and typing

<pre>$ git --version</pre>

Next, **the editor**. Installing it is as easy as it gets. If you have a preferred editor, you can skip this step.

<i class="fa fa-linux"></i> If you're on Linux, open a terminal and use your package manager to install Atom:

<pre>$ apt-get install atom</pre>

Alternatively, download the [.deb](https://atom.io/download/deb) or [rpm](https://atom.io/download/rpm) file and install it.

<i class="fa fa-windows"></i> <i class="fa fa-apple"></i> Windows and Mac users can download the appropriate Windows or OS X installer from the [Atom website][atom_link] and install it.

<i class="fa fa-linux"></i> <i class="fa fa-windows"></i>  <i class="fa fa-apple"></i> You're pretty much done with this step. I suggest you install at least two extra packages for Atom. The first is [language-65asm](https://atom.io/packages/language-65asm) which will add syntax highlighting for 6502/65816 assembly. I also recommend [vim-mode-plus](https://atom.io/packages/vim-mode-plus). If you're not familiar with vim or emacs, read [this](https://medium.com/@olitreadwell/on-why-you-should-learn-emacs-or-vim-as-soon-as-youre-comfortable-with-your-first-editor-4a444ae188a3). They greatly improve productivity for programmers, so choose one and learn to use it. Take some time to familiarize yourself with Atom by reading the [Atom Flight Manual](https://flight-manual.atom.io/getting-started/sections/atom-basics/). Choose a UI and syntax theme you like, and once you're happy with your editor, come back here.

Next, the **cc65 toolchain**.

This step is easiest on Linux and OS X, Windows users will need to take an extra step.

<i class="fa fa-linux"></i> Linux users again use their trusty package manager to install cc65:

<pre>$ apt-get install cc65</pre>

If you prefer to build from source, follow the [instructions of the cc65 GitHub page](http://cc65.github.io/cc65/getting-started.html).

<i class="fa fa-windows"></i> Windows users download and extract the most recent [snapshot of cc65](href="https://sourceforge.net/projects/cc65/files/cc65-snapshot-win32.zip/download), place it in a location of your choice(I suggest `C:\cc65` for simplicity). Don't forget to add `C:\cc65\bin` to [your PATH](https://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/).

<i class="fa fa-apple"></i> OS X users will enlist the help of their (new) package manager, brew. Open a terminal and type

<pre>$ brew install cc65</pre>

<i class="fa fa-linux"></i> <i class="fa fa-windows"></i> <i class="fa fa-apple"></i> Test your installation by opening a terminal and typing:

<pre>$ cc65 --version</pre>

If you run into any trouble, check out the [cc65 Wiki](https://github.com/cc65/wiki/wiki) on GitHub for installation instructions. They have detailed installation instructions for each system.

Finally, **the bsnes+ emulator**.

<i class="fa fa-linux"></i> Linux users use their package manager again to install bsnes+:

<pre>$ apt-get install bsnes-plus</pre>

Unfortunately, for Windows and Mac users the process is a bit more complicated. You will have to build the emulator from source. I'll try and guide you through every single step.

<i class="fa fa-windows"></i> Windows users need to take three steps:

* Install mingw-w64
* Install Qt 4.8.6
* Build bsnes+

Installing mingw-w64 is easy, download the [Installer](http://mingw-w64.org/doku.php) and execute it. Test it by opening a command line window and typing

<pre>$ g++</pre>

If Windows can't find the application, you need to add `MinGW\bin` to your path the same way you added `C:\cc65\bin`.

Next, download and install [Qt 4.8.6](https://download.qt.io/archive/qt/4.8/4.8.6/).

Now download the bsnes+ source code with git from GitHub. Open a command line window and navigate to a directory where you want to keep bsnes+, e.g. `C:\users\yourusername\assemblyadventure`. Once you're there, download the source code of bsnes+ with git:

<pre>$ mkdir assemblyadventure<br>$ cd assemblyadventure<br>$ git clone https://github.com/cc65/cc65.git</pre>

This will create a new directory called `bsnes-plus`. Navigate to it and start the build process:

<pre>$ cd bsnes-plus\bsnes<br>$ make</pre>

If no error occurs during building, you should find the bsnes+ executable in the `bin` subdirectory. If you have trouble building bsnes+, check the [bsnes-plus GitHub page](https://github.com/cc65/wiki/wiki#Host_platforms). Alternatively, download a [bsnes+ executable](http://sourceforge.net/projects/cc65/files/cc65-snapshot-win32.zip). But I recommend you build it yourself to get the latest version. Remember to add bsnes+ to your PATH.

<i class="fa fa-apple"></i> Now on to OS X. This build process will be a bit more complicated since OS X dropped support for Qt 4.8.6 but there is a workaround. OS X version 10.12 and later need this workaround:

<pre>$ brew install cartr/qt4/qt<br>$ brew linkapps qt</pre>

Make sure that the environmental variable `qtpath` is set by adding this to your `.bash_profile` file:

<pre>export qtpath=/usr/local/Cellar/qt/4.8.7_2</pre>

Now create a working directory anywhere you like. Open a terminal. Navigate to that directory and download the source code of bsnes+ with git:

<pre>$ git clone https://github.com/cc65/cc65.git</pre>

This will create a new directory called `bsnes-plus`, move into it and start the build process:

<pre>$ cd bsnes-plus/bsnes<br>$ make</pre>

If the build succeeds, there should be a new subdirectory called bin. There you will find bsnes+. If you encounter any problems, check the [bsnes-plus GitHub page](https://github.com/devinacker/bsnes-plus), scroll down to find installation instructions for help.

If nothing went wrong you now have git, an editor, the cc65 toolchain, and bsnes+ working on your system. Good work!

<i class="fa fa-linux"></i> <i class="fa fa-windows"></i> <i class="fa fa-apple"></i> Test your emulator by starting it (choose compatibility profile; we will talk about the difference between the compatibility and accuracy profile in the next articles) and running this test ROM I have written. Got the message? Excellent.

To finish off this article let's go and write your very first SNES game.

### Your First Game

You will now write your first lines of 65816 assembly. I suggest you keep all files in one directory. For this article, I will store all files in `~/assemblyadventure/lesson01` on my system. Windows users might use `C:\assemblyadventure\lesson01`.

Open your editor and create a new file `nihil.s` and enter this code:

{% highlight nasm %}
; -----------------------------------------------------------------------------
;   File: nihil.s
;   Description: Your very first SNES game!
; -----------------------------------------------------------------------------

;----- Assembler Directives ----------------------------------------------------
.p816                           ; this is 65816 code
.i16                            ; X and Y registers are 16 bit
.a8                             ; A register is 8 bit
;-------------------------------------------------------------------------------

;-------------------------------------------------------------------------------
;   This is were the magic happens
;-------------------------------------------------------------------------------
.segment "CODE"
.proc   ResetHandler            ; program entry point
        ; sei                     ; disable interrupts
        clc                     ; clear the carry flag...
        xce                     ; ...and switch to native mode

        lda #$81                ; enable...
        sta $4200               ; ...non-maskable interrupt

        jmp GameLoop            ; initialisation done, jump to game loop
.endproc

.proc   GameLoop                ; The main game loop
        wai                     ; wait for NMI interrupt
        jmp GameLoop            ; jump to beginning of main game loop
.endproc

.proc   NMIHandler              ; NMIHandler, called every frame/V-blank
        lda $4210               ; read NMI status
        rti                     ; interrupt done, return to main game loop
.endproc

;-------------------------------------------------------------------------------
;   Interrupt and Reset vectors for the 65816 CPU
;-------------------------------------------------------------------------------
.segment "VECTOR"
; native mode   COP,        BRK,        ABT,
.addr           $0000,      $0000,      $0000
;               NMI,        RST,        IRQ
.addr           NMIHandler, $0000,      $0000

.word           $0000, $0000    ; four unused bytes

; emulation m.  COP,        BRK,        ABT,
.addr           $0000,      $0000,      $0000
;               NMI,        RST,        IRQ
.addr           $0000 ,     ResetHandler, $0000
{% endhighlight %}


<p>Next, create another file called memmap.cfg, enter this code:</p>

<code data-gist-id="293ff9ba924da2530cbeedf0853e35aa"></code>

<p>Don't forget to save. Next, you will build your very first SNES game!</p>

<p>Open a terminal and navigate to the directory where the files reside you just created. Then enter the following command to invoke the cc65 toolchain to build your first game:</p>

<pre>$ ca65 --cpu 65816 -o nihil.o nihil.s<br>$ ld65 -C memmap.cfg nihil.o -o nihil.smc</pre>

<p>Make sure you use an upper-case C on the second command. Otherwise, ld65 will complain. You might wonder about ca65 and ld65. Those are the assembler and linker I told you earlier about. The cc65 toolchain is made up of several tools. We will take a closer look at how they work in the next articles. If everything worked, there should be a new file called nihil.smc.</p>

<p>Open bsnes+ and open your new file with it. On startup, select the compatibility profile. Now you should see this:</p>

<p><figure class="tmblr-full" data-orig-height="503" data-orig-width="500" data-orig-src="https://78.media.tumblr.com/6758d5992250acd8e9b3b096b50b74cb/tumblr_inline_p67u2tT2jE1re8b80_540.png"><img src="https://78.media.tumblr.com/eda644f6f91f6154e25c4756e5475a2c/tumblr_inline_p9xnq8fy6Z1re8b80_540.png" alt="" data-orig-height="503" data-orig-width="500" data-orig-src="https://78.media.tumblr.com/6758d5992250acd8e9b3b096b50b74cb/tumblr_inline_p67u2tT2jE1re8b80_540.png"></figure></p>

<p>Congratulations! This is your first SNES game. It is very nihilistic inasmuch it does absolutely <em>nothing</em>. (That's not entirely true, the code acknowledges that the so-called Non-Maskable Interrupt occurred - and returns to waiting/doing nothing. But more on this in the next articles). I told you your first game would be boring.</p>

<p>
But don't be disappointed, you have achieved a lot today. You have set up a development environment. This is often the hardest part of game development. You will use to learn all about SNES assembly programming. Your adventure has just started.
</p>

<p>That's all for today. In the next article, I will introduce you to 65816 assembly programming. I hope you enjoyed this first article of hopefully many more to come.</p>

<p>I'm always happy for comments and suggestions, use the AMA function or just drop me a message.</p>

<h3>References and Links</h3>

<ul><li>Here is a nice <a href="https://www.rollingstone.com/culture/news/super-nintendo-25-year-anniversary-why-snes-still-matters-w435671">article</a> about the history of the SNES</li>
<li>If you prefer watching videos check <a href="https://www.youtube.com/watch?v=RqfnreKMPE8">this one</a> out</li>
<li>The <a href="https://wiki.superfamicom.org">Super Famicom Development Wiki</a> is a great resource for SNES programming. I will reference it in future articles, so add it to your bookmark</li>
</ul></p>

[atom_link]: https://www.atom.io
[vsc_link]: https://code.visualstudio.com
[sublime_link]: https://www.sublimetext.com
[vim_link]: https://www.vim.org
[nocash_link]: http://problemkaputt.de/sns.htm
[snesx_link]: http://geigercount.net/crypt/
[gitbook_link]: https://git-scm.com/book/en/v2
[bsnes_link]: https://github.com/devinacker/bsnes-plus
