---
layout:     post
title:      "SNES Assembly Adventure 03: "
date:       2018-08-11
excerpt:    "Create your first own ROM file and debug it on a SNES emulator"
tags:       [SNES Assembly Adventure, assembly, programming, SNES, tutorial]
feature:    /assets/snesaa/02/saa02_featurecard.gif
published:  true
comments:   true
---
# DRAFT

Welcome back, Adventurer. I hope the last time wasn't too hard on you. I admit it got a bit long (I strive to keep it under 2,500 words, the last one was 3,900). But we covered some important ground that will come in handy now.

In this article (should I start calling them dungeons instead? Levels? Quests? I'll think about it), we will go about things a bit differently. Since the last article was heavy on the theory I want to keep this one a bit more practical.

I promised you in the last article that you will finally display a sprite on the SNES, so in this article, we will

* discuss the graphics format of the SNES
* create a simple sprite by hand
* dissect the code of a simple demo that displays the sprite you just created

I will reserve a detailed discussion of the SNES architecture for the next article. I will give you a short overview when we dissect the example code of this article. Let's get started!

[[ saa03 card ]]

### Intertwined and Planar Graphic Formats
When displaying image data, the SNES relies on a technique called *(indirect) color indexing* (in fact, the NES and Game Boy used this technique too, as we will see shortly). Instead of saving the color of a certain pixel, the index number of a color is saved that references a color inside a palette.

Let's look at a simple 8 x 8 example. This first image is in *1 bit per pixel* format, or 1bpp:

[[ 1bpp ]]

Each pixel holds 1 bit. So this means, a pixel can have two colors, either color 0 or color 1. On the left, you see the sprite. To the right of it, you see the *color index* assigned to each pixel (i.e., the index of the color the pixel will have). The numbers under *Mem.* are the indices of the corresponding lines translated to hexadecimal. And finally, the palette that shows which index represents which color.

Now, only two colors are nice for certain art styles, but to relive the glory of 16-bit graphics we need a bit more (no pun intended). Let's extend the format to *2 bits per pixel* (i.e., 2bpp). Having two bits per pixel allows us to assign four different colors to each pixel (remember how binary numbers work?). Here is an example:

[[ 2bpp ]]

Let's introduce the concept of *bitplanes*. Since we now need two bits to store the information for one pixel, we need to think on how to store those bits in memory. Each byte in a bitplane represents one row of the sprite. Each bitplane holds one bit of the color index of the corresponding pixel. So to get the color index of a specific pixel we need to combine the data of each bitplane. For example, to get the color index of the first pixel of the first row (the pixel on the upper far-left), we look at the first bit of the first row of each bitplane and combine them to form a 2-bit number.

Let's do an example:

[[ 2 bpp example ]]

This can be hard to wrap one's head around at first. I suggest you take the data from the gif above and try to complete the sprite by hand on paper. Yes, it's tedious. But when we talk about the architecture of the SNES in the next article, a lot of the things described there will be easier to understand. Most of the graphical limitations of the SNES stem from the way it stores and processes graphics data.

Now is a good point to talk about the section title, *intertwined* and *planar* data storage. Once we have converted our graphics to a format the SNES understands. There are two ways to store the data in memory:

* Planar: Each row (i.e., byte) of a bitplane is stored consecutively, starting with the top row of the lowest bitplane; then the same is repeated with each row of the next bitplane
* Intertwined: Each pair of bytes consecutively stored represents a row of the sprite with the byte from the lower bitplane being stored first

This again might sound confusing, so let's try to clarify this with another picture:

[[ intertwined planar ]]

On top, you have the two bitplanes and their hexadecimal equivalent. Below you see the graphics data stored once in planar and once in intertwined format. In both cases, The bytes are stored in consecutive memory locations from top to bottom.

Why does the distinction matter? Nintendo has used both formats in their consoles. While the NES stored graphics data in planar format, the Game Boy used the intertwined format. Both consoles could display sprites with a maximum of four colors (why they used different formats on each console is anyone's guess; the overall architectures off both consoles aren't that different, to it probably came down to a factor like performance or code design).

So which format does the SNES use? The answer is both.

I know, more confusion. But before I explain that, there is an important piece of information to add: While I simply used white as the color 0 of the palette, this isn't entirely correct. The first color of a palette has a special function. The SNES considers the first color in any palette as *transparent* and will not render it. So any pixel that has the color index 0 will not be displayed. We will see this in action in the example code later on.

Now, back planar and intertwined. I just told you the SNES uses both formats. Why? The SNES (unlike the NES and Game Boy) can actually switch between different bits per pixel formats. In fact, the SNES supports a total of five bit formats: 2bpp, 3bpp, 4bpp, 8bpp, and the special Mode 7 format (this is the ominous mode used in some games to simulate some kind of pseudo 3D rendering, e.g. in Final Fantasy VI, Mario Kart, or Wipeout; I will cover Mode 7, but that is -sadly- still some way off).

You already know 1bpp and 2bpp. Extending 2bpp to 3bpp and 4bpp isn't hard if you understand how color indexing works by now. You simply add the extra bitplanes needed. So 3bpp will let you use a total of eight colors, 4bpp a total of 16 colors. The two most frequently used formats are 2bpp (HUD elements and background) and 4bpp (backgrounds and sprites).

This concludes the section on the color indexed graphics format. If you feel overwhelmed by it, there are two excellent resources that explain this in more detail. You will find links to them at the end of this article in the *Links and References* section.

Let's put this new knowledge to good use and actually create a 16 x 16 sprite (which is simply four 8 x 8 sprites combined) in 4bpp format by hand. Finally, we get practical, as promised!

(If you're wondering, "Wait! I have now all the color indices, but where are the *colors themselves*?" You are correct, we haven't covered that yet. This will happen in the next section.)

## Handcrafting a Sprite
Before we start, let me state that this is **not** the best way to create graphic assets for the SNES. There are a lot of tools on the web that will automatically convert assets in jpg, png, bmp, or any other format to 2bpp/4bpp for you. I will link some common tools in the *Links and Reference* section at the end of this article.

Since the victory of Sonicfox at Evo 2018 is all the rage right now,  will recreate the face of Super Kai from *Dragon Ball Z: Super Butouden 3*. I am a horrible pixel artist, so I won't take you through all steps of creating pixel arts, there are more than enough tutorials and guide around on the web. So, here are the finished sprites:

[[ super kai ]]
<figure>
    <a href="{{ "/assets/snesaa/03/saa02_arch.png" | absolute_url }}">
        <img src="{{ "/assets/snesaa/03/saa02_arch.png" | absolute_url }}">
    </a>
    <figcaption>65816 Architecture</figcaption>
</figure>


Since we're going to use 4bpp format, I can use up to 16 colors (remember to reserve one color for transparency). Let's start by creating the color indices:

[[ color indices ]]
<figure>
    <a href="{{ "/assets/snesaa/03/saa3_superkai02.png" | absolute_url }}">
        <img src="{{ "/assets/snesaa/03/saa3_superkai02.png" | absolute_url }}">
    </a>
    <figcaption>65816 Architecture</figcaption>
</figure>
