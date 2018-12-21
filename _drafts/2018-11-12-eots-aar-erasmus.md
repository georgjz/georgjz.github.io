---
layout:     post
title:      "An Empire of The Sun After Action Report"
date:       2018-11-12
excerpt:    "I play against the new bot for solitaire play in Empire of The Sun"
tags:       [Wargaming, EoTS, Empire of The Sun, aar, after action report]
feature:    /assets/snesaa/02/saa02_featurecard.gif
published:  true
comments:   true
---
# DRAFT

### Introduction
One of my favorite games is [Empire of The Sun][1] by Mark Herman. This game simulates the Pacific Theater of World War II on an operational level. The Japanese must force the US into negotiating a peace treaty by lowering US political will, while the Allies must push back the Japanese and prevent them from taking precious resources that will fuel the Japanese war machine.

The game ships with a bot called *Erasmus* that is meant to simulate an opponent and as a teaching tool for the game. I decided to take on the [draft of the new *Card Driven Solitaire System*][2] and its [companion cards][3] one fine evening.

I play Japan against the Allied Erasmus. Here you'll find a fast & loose turn 2 I played against it. The idea was to get a quick overview how *Erasmus* works. So please forgive me if I skip any rules or details.

That being said, let's get into it!

### Setup
For simplicity, I play the 1942 Campaign.

<figure>
    <a href="{{ "/assets/eots/mevserasmus/T1_setup.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/mevserasmus/T1_setup.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>The setup for the 1942 Campaign</figcaption>
</figure>

As per scenario rules, the Allies get free Naval Emergency Movements to open the game. I use the "default" Allied moves:

* US DD Asia to Batavia (2018)
* US [CA Asia] to Biak (3319)
* Dutch CL to Soerabaja (2220)
* Australian CA Kent to Gili-Gili (4024)
* British CA Exeter to Dacca (1905)

Unit names in brackets denote reduced units. After the NEM, I draw the opening hands. I opt for the historical variant to ensure an aggressive Japanese opening. The Allies opt to take the *Arcadia Conference* card (\#4) since there really isn't a reason not to.

Here's the Japanese opening hand:

{% capture jp_opening_hand %}
    {{ "/assets/eots/cards/japanese cards47.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards12.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards03.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards59.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards51.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards64.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards45.gif" | uri_escape | absolute_url }}
{% endcapture %}
{% include gallery images=jp_opening_hand caption="Japanese opening hand" cols=4 %}

I get four offensive cards, one political card, and two reaction cards.

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

I commence the festivities with a standard offensive: My goal is to take out the Allied air units in the Philippines, Malaya, and Dutch East Indies. This will prevent the Allies player from activating his HQs in Manila and Singapore (for a while at least). I activate the South Sea HQ in truk (4017) for 7 + 2 = 9 activations.

**Activations and Movements**: South Sea HQ for 9 activations

* The 3rd and 4th Air Division move from Nagoya (3407) to Aparri (2911). They'll attack Manila (2813); Manila will be a battle hex.
* The 5th Air Division moves from Clark Field (2812) to Kuala Lumpur (1913). They attack Singapore (2015), so it will be a battle hex.
* CVs Shokaku and Soryu move to hex 2314 and also attack Singapore.
* CVL Ryujo, CA Nachi, and [16th Army] invade Soerabaja (2220), so another battle hex. (Use one ASP)
* BB Hiei moves to Baik (3319) to battle the US Asia fleet in the fourth battle hex.

<figure>
    <a href="{{ "/assets/eots/T1_prebattle_1.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/T1_prebattle_1.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Situation prior to battle, declared battle hexes</figcaption>
</figure>

**Reactions**: Now *Erasmus* comes into play for the first time. *Erasmus* will tell you how your opponent acts and reacts according to a given set of decision matrices.

Open the [link I provided][3] earlier to follow the decision matrix on page 12:

**A**: No <span>&#8594;</span> **B**: Yes <span>&#8594;</span> **C**: Yes <span>&#8594;</span> **D**: No or **E**: No or **F**: No <span>&#8594;</span> **H**: No and **I**: No <span>&#8594;</span> Execute PBM if applicable

There are no Allied reaction moves.

**Battle Resolution**: There are four battle hexes to resolve:

* **Manila** (A): Japan 40 vs. Allies 4. Japan roll: 4 for 20 hits. Allies roll: 6 for 4 hits. Japan destroys the FEAF air unit (11 hits lost), Allies don't have enough hits to reduce any Japanese unit (4 hits lost).
* **Singapore** (B): Japan 46 vs. Allies 6. Japan roll: 8 for 46 hits. Allies roll: 6 for 9 hits. Japan reduces and destroys MA air unit for 18 hits (28 hits lost), Allies again don't have enough hits (9 hits lost).
* **Soerabaja** (C): Japan 18 vs. Allies 3. Japan roll: 3 for 9 hits. Allies roll: 8 for 9 hits. Dutch CA unit reduced, Japan wins Naval-Air battle so invasion proceeds successfully.
* **Biak** (D): Japan 17 vs. Allies 2. Japan roll: 9 for 17 hits and critical hit. Allies roll: 7 for 2 hits. CA US Asia destroyed (11 hits lost), Allied hits have no effect (2 hits lost).

[Dutch CA] unit conducts Naval Emergency Movement to Tjilatjap (2019).

**Post Battle Movement**: The Allies have no active unit eligible for PBM, so Japan conducts PBM next.

* The 4th Air Division moves from Aparri (2911) to Jitra (1912); this will neutralize the Malaya HQ.
* CVs Shokaku and Soryu move to Davao (2915).
* CA Nachi moves from Soerabaja (2220) to Miri (2415).
* BB Hiei moves back to Tokyo (3706)
* All other naval and air units stay where they are; CVL Ryujo prevents the units in Tjilatjap (2019) from being activated.

### Allies Card 1: *Arcadia Conference* (\#) as OC
<figure>
    <a href="{{ "/assets/eots/cards/allied cards34.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/allied cards34.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Allies card 1, Operation Culverin</figcaption>
</figure>

The Allies opt to play this as an OC card to remove SW Pac HQ. It's placed on the turn marker for return in turn 3.

<u>My Take</u>: An interesting move. Instead of playing *Arcadia Conference* (\#4) into Australia or the DEI, he opts to pull SW Pac HQ early. So he may (or may not) bring it back this turn if he wishes. And he made the smart move to reinforce Manila before pulling out. But it will also prevent him from activating the units trapped in Tjilatjap (2019) for (at least) one more card play. I'll make use of that.

### Japan Card 2: *Naval Battle of Guadalcanal* (\#20) as EC
<figure>
    <a href="{{ "/assets/eots/cards/japanese cards20.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/japanese cards20.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Japan card 2, Naval Battle of Guadalcanal</figcaption>
</figure>

My first offensive went smoothly, so I'll try to keep up the momentum and continue to push into the Dutch East Indies.

**Activations and Movements**: Combined Fleet HQ for 8 activations

* 27th Army (reduced) moves from Hakodate (3704) to invade Amchitka (4700), 1 ASP used.
* CA Nachi and 17th Army (reduced) move from Hong Kong (2709) to invade Balikpapan (2517), 1 ASP used; Balikpapan will be a battle hex.
* 3rd Air Division moves from Aparri (2911) to Davao (2915); will participate in the battle hex in Balikpapan.
* CVL Zuiho from Davao (2915), and 19th Army from Vogelkop (3219) invade Tarakan (2616), 1 ASP used; Tarakan will be a battle hex.
* BB Kongo moves from Cam Ranh (2311) to Tjilatjap (2019); Tjilatjap will be a battle hex.
* 25th Army (reduced) moves from Kota Bharu (2112) to Bangka (2017), 1 ASP used.

<figure>
    <a href="{{ "/assets/eots/T1_prebattle_2.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/T1_prebattle_2.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Situation prior to battle, declared battle hexes</figcaption>
</figure>

The idea of invading Amchitka is twofold: I want to force him to at least divert some resources to retaking Amchitka. And I want to be able to threaten Midway with a 2OC card. Amchitka is in range of Combined Fleet HQ, exactly 10 hexes away from Midway, and there is no way for the Allies to block supplies to Amchitka.

The three other invasions are pretty self-explanatory I think; I take three more resource hexes and make use of the fact that my card is only a 2OC card, so he can't react to any of the battle hexes in any way.

Tjilatjap was a bit concerning with a total attack strength of 14; so the Allies would be able to hit both battleships and carriers. But thanks to the card event, the total attack strength will drop to 11, minimizing my risks.

Let's see how these battles turn out.

**Reactions**: The Allies opt for an intel roll: 9. Back luck, so no reactions and I get a surprise attack.

**Battle Resolution**: There are three battle hexes to resolve:

* **Balikpapan** (A) and **Tarakan** (B): Since I only have naval and air units in both battle hexes, I get a +4DRM, which makes both battles an auto-win for me.
* **Tjilatjap** (C): Japan 13 vs. Allies 11. Japan roll +3DRM for surprise attack: 9 for 13 hits and a critical hit. Japan uses the critical hit to destroy the Dutch air unit and the remaining 4 hits to reduce US Asia (DD). Allies attack strength is now 6. Allies roll: 1 for 6 hits, no effect (6 hits lost).

The invasions of Bangka (2017) and Amchitka (4700) are unopposed and succeed.

**Post Battle Movement**: The Allies have no active unit eligible for PBM, so Japan conducts PBM next.

* CA Nachi moves to Miri (2415)
* CVL Zuiho moves back to Davao (3016)
* BB Kongo moves back to Cam Ranh (2311)
* 3rd Air Division moves to Balikpapan (2517)

<figure>
    <a href="{{ "/assets/eots/T1_postbattle_2.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/T1_postbattle_2.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Situation after battle resolution</figcaption>
</figure>

I took out a total of three naval/air steps, and two ground steps while taking no losses. Further, three more resource hexes. All in all, not a bad start. But I have already used four out of seven ASPs, so it's probably time to swing East for the Solomons and New Guinea.

The battle of Tjilatjap delivered me a lucky punch, I took out his air unit effectively trapping the small fleet there. So any further amphibious assaults won't be in danger of being pushed back by enemy naval units.

I by now assume he's going to play one of his HQs into Australia. He should be able to guess by now that I'm going to swing East. Without an HQ, he won't be able to react and at least annoy me until reinforcements arrive.



[1]: https://www.gmtgames.com/p-645-empire-of-the-sun-3rd-printing.aspx
[2]: https://s3-us-west-2.amazonaws.com/gmtwebsiteassets/nneots/EOTSRULES2015v3.pdf
[3]: https://www.youtube.com/watch?v=JpmWYELBzCU&list=PLDp4C70KgUqaoV4Uw1ApJ96usebj9dtTC
[4]: https://www.youtube.com/watch?v=CfqEUiHlCVI[5]:
[5]: https://boardgamegeek.com/boardgame/11825/empire-sun
[6]: http://www.vassalengine.org/index.php
