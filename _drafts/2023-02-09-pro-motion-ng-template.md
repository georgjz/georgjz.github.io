---
layout:     post
title:      "A Plugin Template for Pro Motion NG"
date:       2023-02-09
excerpt:    "A simple project template for creating custom plugins for Pro Motion NG"
tags:       [programming, pixel art, Pro Motion NG]
published:  true
comments:   true
---
# A Plugin Template for Pro Motion NG

I've been using [Pro Motion NG][promotionng] for a while now. I originally used [Aseprite][aseprite], which comes with a nice [Lua scripting API][asepriteapi]. In fact, I made the title cards and graphics for [my SNES Assembly Adventure series][snesaa].

Creating graphics for SNES games can be challenging. You need to keep track of the color format and indices. I plan to write such a plugin for SNES (and potentially other platforms) for Pro Motion NG.

As a first step, I created this little template to build a [custom plugin][plugins] from source. It does absolutely nothing, but it does that extremely well.

You can find it on [GitHub][repository].


[snesaa]: {% post_url snesaa/2018-07-26-snesaa01 %}
[repository]: https://github.com/georgjz/pro-motion-ng-plugin-template
[aseprite]: https://www.aseprite.org
[asepriteapi]: https://github.com/aseprite/api
[promotionng]: https://www.cosmigo.com
[promotionngsteam]: https://store.steampowered.com/app/671190/Pro_Motion_NG/
[promotionngdoc]: https://www.cosmigo.com/pixel_animation_software/plugins/developer-interface
[plugins]: https://www.cosmigo.com/pixel_animation_software/plugins/available-plugins
