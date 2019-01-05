---
layout:     post
title:      "An Empire of The Sun After Action Report: Me vs Erasmus Bot, Turn 2"
date:       2019-01-05
excerpt:    "I play against the new version of the bot for solitaire play in Empire of The Sun"
tags:       [Wargaming, EoTS, Empire of The Sun, aar, after action report]
feature:    /assets/snesaa/02/saa02_featurecard.gif
published:  true
comments:   false
---
# DRAFT

### Introduction
First post in 2019, so Happy New Year! Besides assembly programming, I love to play wargames. One of my favorite games is [Empire of The Sun][1] by Mark Herman. This game simulates the Pacific Theater of World War II on an operational level. The Japanese must force the US into negotiating a peace treaty by lowering US political will, while the Allies must push back the Japanese and prevent them from taking precious resources that will fuel the Japanese war machine.

The game ships with a bot called *Erasmus* that is meant to simulate an opponent and as a teaching tool for the game. I decided to take on the [draft of the new *Card Driven Solitaire System*][2] and its [companion cards][3] one fine evening. (You have to be a member and logged into boardgamegeek to view these files)

I play as Japan against the Allied Erasmus. Here you'll find a fast & loose game I played against it. The idea was to get a quick overview of how *Erasmus* works. So please forgive me if I skip any rules or details. So you should be somewhat familiar with the [rules][4].

That being said, let's get into it!

### Setup
For simplicity, I play the 1942 Campaign.

<figure>
    <a href="{{ "/assets/eots/mevserasmus/T1_001_setup.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/mevserasmus/T1_001_setup.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>The setup for the 1942 Campaign</figcaption>
</figure>

As per scenario rules, the Allies get free Naval Emergency Movements to open the game. I use the "default" Allied moves:

* US DD Asia to Batavia (2018)
* US [CA Asia] to Biak (3319)
* Dutch CL to Soerabaja (2220)
* Australian CA Kent to Gili-Gili (4024)
* British CA Exeter to Dacca (1905)

Unit names in brackets denote reduced units. After the Emergency Movements, I draw the opening hands. I opt for the historical variant to ensure an aggressive Japanese opening. The Allies opt to take the *Arcadia Conference* card (\#4) since there really isn't a reason not to.

Here's the Japanese opening hand:

{% capture jp_opening_hand %}
    {{ "/assets/eots/cards/japanese cards69.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards47.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards03.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards70.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards82.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards57.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards74.gif" | uri_escape | absolute_url }}
{% endcapture %}
{% include gallery images=jp_opening_hand caption="Japanese opening hand" cols=4 %}

I get three offensive cards, one reaction card, and three political cards.

And here's the opening hand of the Allies:

{% capture al_opening_hand %}
    {{ "/assets/eots/cards/allies cards19.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/allies cards04.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/allies cards82.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/allies cards14.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/allies cards77.gif" | uri_escape | absolute_url }}
{% endcapture %}
{% include gallery images=al_opening_hand caption="Allies opening hand" cols=4 %}


### Japan Card 1: *VADM Kondo* (\#47) as EC
<figure>
    <a href="{{ "/assets/eots/cards/japanese cards47.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/japanese cards47.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Japan card 1, VADM Kondo</figcaption>
</figure>

I commence the festivities with a standard offensive: My goal is to take out the Allied air units in the Philippines, Malaya, and Dutch East Indies. This will prevent the Allies player from activating his HQs in Manila and Singapore (for a while at least). I activate the South Seas HQ in truk (4017) for 7 + 2 = 9 activations.

**Activations and Movements**: South Seas HQ for 9 activations

* The 3rd and 4th Air Division move from Nagoya (3407) to Aparri (2911) They'll attack Manila (2813); Manila will be a battle hex
* The 5th Air Division moves from Clark Field (2812) to Kuala Lumpur (1913) They attack Singapore (2015), so it will be a battle hex
* CVL Ryujo, CA Nachi, and [16th Army] invade Soerabaja (2220), so another battle hex (Use one ASP)
* BB Hiei moves to Tarakan (2612), as does [19th Army] for an invasion. Tarakan will be a battle hex (Use one ASP)

<figure>
    <a href="{{ "/assets/eots/T1_002_prebattle_jcard1.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/T1_002_prebattle_jcard1.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Situation prior to battle, declared battle hexes</figcaption>
</figure>

**Reactions**: Now *Erasmus* comes into play for the first time. *Erasmus* will tell you how your opponent acts and reacts according to a given set of decision matrices.

Open the [link I provided][3] earlier to follow the decision matrix on page 12:

**A**: No <span>&#8594;</span> **B**: Yes <span>&#8594;</span> **C**: Yes <span>&#8594;</span> **D**: No or **E**: No or **F**: No <span>&#8594;</span> Intel Roll: 1 - 2DRM = -1, Intercept <span>&#8594;</span> **H**: No and **I**: No <span>&#8594;</span> Execute PBM if applicable

There are no Allied reaction moves. Erasmus can't match my firepower (for now...)

**Battle Resolution**: There are four battle hexes to resolve:

* **Soerabaja** (A): Japan 20 (+2 from event) vs. Allies 3. Japan roll: 2 for 5 hits. Allies roll: 8 for 3 hits. All hits are lost, but Japanese total attack strength is higher after Air-Naval Combat, so the invasion is successful. Japan controls Soerabaja.
* **Singapore** (B): Japan 22 vs. Allies 6. Japan roll: 6 for 22 hits. Allies roll: 0 for 2 hits. Japan reduces and destroys MA air unit for 18 hits (4 hits lost), Allies again don't have enough hits (2 hits lost).
* **Tarakan** (C): No Air-Naval Combat, so we move directly to Ground Combat. Japan 9 vs. Allies 1. Japan roll: 2 + 2DRM = 4 for 9 hits. Allies roll: 0 + 3DRM = 3 for 1 hit. Dutch 3rd Regiment is destroyed (3 hits lost), Allies lose their hit. Japan now controls Tarakan.
* **Manila** (D): Japan 40 vs. Allies 4. Japan roll: 5 for 40 hits. Allies roll: 8 for 4 hits. FEAF Air Unit destroyed (30 hits lost), Allied hits have no effect (4 hits lost).

Dutch CL unit conducts Naval Emergency Movement to Tjilatjap (2019).

**Post Battle Movement**: The Allies have no active unit eligible for PBM, so Japan conducts PBM next.

* CA Nachi moves from Soerabaja (2220) to Miri (2415)
* BB Hiei moves back to Tokyo (3706)
* 4th Air Division moves to Tarakan (2616)
* All other naval and air units stay where they are; CVL Ryujo prevents the units in Tjilatjap (2019) from being activated.


### Allies Card 1: *Arcadia Conference* (\#4) as EC
<figure>
    <a href="{{ "/assets/eots/cards/allied cards04.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/allied cards04.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Allies card 1, Operation Culverin</figcaption>
</figure>

Here Erasmus uses the *Axis of Determination* to plot its first move, you can find it on [page 7][3]:

**A**: Yes <span>&#8594;</span> **B**: No <span>&#8594;</span> **C**: No <span>&#8594;</span> **D**: No <span>&#8594;</span> Establish ABDA

As per the strategy box, we move ABDA HQ into Kendari.


### Japan Card 2: *Central Force* (\#59) as EC
<figure>
    <a href="{{ "/assets/eots/cards/japanese cards59.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/japanese cards59.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Japan card 2, Central Force</figcaption>
</figure>

My first offensive went smoothly, so I'll try to keep up the momentum and continue to push into the Dutch East Indies.

**Activations and Movements**: Combined Fleet HQ for 8 activations

* [19th Army] moves from Tarakan (2616) to invade Balikpapan (2517) (Use one ASP)
* 4th Air Division in Tarakan (2616) supports the invasion of Balikpapan
* CVL Zuiho from Soerabaja (2220) moves to Balikpapan (2517) for invasion support
* CA Nachi and 2nd Marine Brigade move from Miri (2415) to Makassar (2620)
* 5th Air Division in Kuala Lumpur (1913) is active but doesn't move

As per card event, I took Medan (1813) with paratroopers, destroying the 2nd Dutch Regiment.

**Reactions**: Erasmus follows the same pattern as with Japan Card 1. Intel roll: 6 - 2DRM = 4. The attack is intercepted. Erasmus again can't mount an effective defense force. Basically, the Reaction matrix will only send a reaction force, if it can match the attack strength of the offensive forces; this is not the case, Erasmus could only muster a total reaction force of 14 attack (DD US Asia, CL Dutch, [CA US Asia], and Dutch Air Unit).

**Battle Resolution**: There's one battle to resolve:

* **Balikpapan** (A): Since I have naval and air units, I get a +4DRM, which makes this battle an auto-win for me. 5th Dutch Regiment is destroyed. Japan controls Balikpapan.

**Post Battle Movement**: The Allies have no active unit eligible for PBM, so Japan conducts PBM next.

* CA Nachi moves to Davao (2915)
* CVL Zuiho moves back to Soerabaja (2220)
* 4th Air Division moves to Makassar (2620); this puts ABDA HQ out of supplies

<figure>
    <a href="{{ "/assets/eots/T1_postbattle_2.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/T1_postbattle_2.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Situation after battle resolution</figcaption>
</figure>


### Allies Card 2: *'Vinegar' Joe Stilwell* (\#7) as EC
<figure>
    <a href="{{ "/assets/eots/cards/allied cards07.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/allied cards07.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Allies card 1, Operation Culverin</figcaption>
</figure>

Erasmus uses the *Axis of Determination*:

**A**: Yes <span>&#8594;</span>
**B**: No <span>&#8594;</span>
**C**: No <span>&#8594;</span>
**D**: Yes <span>&#8594;</span>
**E**: No <span>&#8594;</span> CBI Defense

Erasmus wants to increase defenses in the Ceylon-Burma-India region. *Card Selection* to [page X][3] tells us which card to use:


As per the strategy box, we move ABDA HQ into Kendari.



[1]: https://www.gmtgames.com/p-645-empire-of-the-sun-3rd-printing.aspx
[2]: https://s3-us-west-2.amazonaws.com/gmtwebsiteassets/nneots/EOTSRULES2015v3.pdf
[3]: https://www.youtube.com/watch?v=JpmWYELBzCU&list=PLDp4C70KgUqaoV4Uw1ApJ96usebj9dtTC
[4]: https://www.youtube.com/watch?v=CfqEUiHlCVI[5]:
[5]: https://boardgamegeek.com/boardgame/11825/empire-sun
[6]: http://www.vassalengine.org/index.php
