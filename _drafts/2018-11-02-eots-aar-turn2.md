---
layout:     post
title:      "An Empire of The Sun After Action Report, Part 1"
date:       2018-11-02
excerpt:    "The Imperial Japanese Forces start their conquest of the West and South Pacific"
tags:       [Wargaming, EoTS, Empire of The Sun, aar, after action report]
feature:    /assets/snesaa/02/saa02_featurecard.gif
published:  true
comments:   true
---
# DRAFT

### Introduction
Besides machine language programming, I enjoy strategy games, especially hex and counter wargames. One of my favorites is [Empire of The Sun][1] by Mark Herman. This game simulates the Pacific Theater of World War II on an operational level. The Japanese must force the US into negotiating a peace treaty by lowering US political will, while the Allies must push back the Japanese and prevent them from taking precious resources that will fuel the Japanese war machine.

I will assume you, the reader, are somewhat familiar with the [rules][2]. *Empire of The Sun* is a CDG or *Card Driven Game*. If you're interested in details, here is a [good video][3] by the designer himself explaining the basics. There is another good [series of videos][4] by John Steidl that'll explain the basic rules and concepts in great detail (the good kind of binge watching!).

That being said, let's get into it!

### Setup
I found my opponent on [Board Game Geek][5]. We agreed to use [Vassal][6] to start a PBEM (play by e-mail) game. Empire of The Sun plays very well as PBEM game, since planing a turn's strategy can be time-consuming. It also eliminates the need to somehow store the game between sessions (a problem every wargamer faces). We will play the 1942 Campaign scenario:

[1942 start up]

As per scenario rules, the Allies get free Naval Emergency Movements to open the game. My opponent moves the US Asia Fleet (one CA, one DD unit) into Tjilatjap (2019), where the Dutch light cruiser is already waiting. A bit of an unorthodox move. But one that will force me to improvise a bit. A lucky card draw helps me greatly to reduce the threat this fleet poses, as we will see shortly.

[opening fleet]

After the NEM, we draw our opening hands. I opt not to take the historical variant (which would give me three fixed cards and four random), so I draw seven random cards. The Allies opt to take the *Arcadia Conference* card (\#4) since there really isn't a reason not to. He draws four additional cards to complete his hand.

Here's the Japanese opening hand:

{% capture jp_opening_hand %}
    {{ "/assets/eots/cards/japanese cards44.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards28.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards20.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards23.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards15.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards57.gif" | uri_escape | absolute_url }}
    {{ "/assets/eots/cards/japanese cards69.gif" | uri_escape | absolute_url }}
{% endcapture %}
{% include gallery images=jp_opening_hand caption="Japanese opening hand" cols=4 %}

I get four offensive cards, two political cards, and one reaction card. Not a bad hand at all. The smaller movement allowance of *Naval Battle of Guadalcanal* (\#20) is something to look for since naval units will only be able to move up to 10 hexes. Drawing both *Tokyo Express* cards (\#28 and \#44) in the same turn might be a bit of a bummer since it will waste the Tokyo Express markers. This might hurt me later in the game. But the two additional ASP will come in very handy. *Operation RE* (\#23) is not a super strong card, but might be useful for a small offensive and/or moving around units for a later offensive phase. The *War In Europe* cards (\#57) are most effective early in the game for the Japanese. Might be a candidate for event play if all of my offensives go smoothly. Now, *Mahatma Gandhi* (\#15) is an interesting card. Some (Allies) players absolutely loathe this card when played as an event, since it will cost them a ground replacement step and move India closer to surrender. On the other hand, playing the event as Japan pretty much means that they are willing to commit resources to the conquest of the Burma-India area; else it's a wasted 3 OC card in my opinion (although this game is a lot about simply *threatening* your opponent's positions). Lastly, *JN25 Code Change* (\#69) might not be really useful this early, since the Allies don't really have the units to run a proper offensive yet. But I have an idea for utilizing this 1 OC card this turn.

So, I believe this to be quite a decent opening hand that will let me wreak some serious havoc on my opponent in this turn.

### Japan Card 1: *Tokyo Express* (\#44) as EC
<figure>
    <a href="{{ "/assets/eots/cards/japanese cards44.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/japanese cards44.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Japan card 1, Tokyo Express</figcaption>
</figure>

I commence the festivities with a standard offensive: My goal is to take out the Allied air units in the Philippines, Malaya, and Dutch East Indies. This will prevent the Allies player from activating his HQs in Manila and Singapore (for a while at least). I activate the Combined Fleet HQ in Kure (3407) for 3 + 4 = 7 activations.

**Activations and Movements**: Combined Fleet HQ for 7 activations

* The 3rd and 4th Air Division move from Nagoya (3407) to Aparri (2911). They'll attack Manila (2813); Manila will be a battle hex.
* The 16th Army (reduced), Nachi (CA), and Ryujo (CVL) move from Davao (2915) to invade Soerabaja (2220).
* The 19th Army (reduced) moves from (2913) to invade Vogelkop (3219).
* The 22nd Air Fleet moves from Saigon (2212) to Kota Bharu (2112) to attack Singapore (2015); Singapore will be a battle hex.

I used both additional ASP the card event grants me.

[board situation prior to battle]

**Reactions**: The Allies opt for an intel roll: 6. So the intelligence status changes to *Intercept*. He activates SW Pac HQ and moves SL Corps from (2912) into Manila (2813).

**Battle Resolution**: There are two battle hexes to resolve:

* **Manila** (A): Japan 40 vs. Allies 4. Japan roll: 9 for 40 hits and critical hit. Allies roll: 7 for 4 hits. Japan destroys the FEAF air unit (critical hit, 40 hits lost), Allies don't have enough hits to reduce any Japanese unit (4 hits lost).
* **Singapore** (B): Japan 20 vs. Allies 9. Japan roll: 8 for 20 hits. Allies roll: 6 for 9 hits. Japan reduces and destroys MA air unit for 18 hits (2 hits lost), Allies again don't have enough hits (9 hits lost).

The invasions of Soerabaja (2220) and Vogelkop (3219) are unopposed and succeed.

**Post Battle Movement**: The Allies have no active unit eligible for PBM, so Japan conducts PBM next.

* The 4th Air Division moves from Aparri (2911) to Jitra (1912); this will neutralize the Malaya HQ.
* Nachi (CA) moves from Soerabaja (2220) to Hong Kong (2709).
* All other naval and air units stay where they are; Ryujo (CVL) prevents the units in Tjilatjap (2019) from being activated.

My first offensive was quite effective. I took out two Allied air units, took two resource hexes, and suffered no losses myself. The dice so far have favored me. Let's see how the Allies open the game.

### Allies Card 1: *Operation Culverin* (\#34) as OC
<figure>
    <a href="{{ "/assets/eots/cards/allied cards34.gif" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/eots/cards/allied cards34.gif" | uri_escape | absolute_url }}">
    </a>
    <figcaption>Allies card 1, Operation Culverin</figcaption>
</figure>

The Allies opt to play this as an OC card to remove SW Pac HQ. It's placed on the turn marker for return in turn 3.

*My Take*: An interesting move. Instead of playing *Arcadia Conference* (\#4) into Australia or the DEI, he opts to pull SW Pac HQ early. So he may (or may not) bring it take this turn if he wishes. And he made the smart move to reinforce Manila before pulling out. But it will also prevent him from activating the units trapped in Tjilatjap (2019) for (at least) one more card play. I'll make use of that.

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
* Nachi (CA) and 17th Army (reduced) move from Hong Kong (2709) to invade Balikpapan (2517), 1 ASP used; Balikpapan will be a battle hex.
* 3rd Air Division moves from Aparri (2911) to Davao (2915); will participate in the battle hex in Balikpapan.
* Zuiho (CVL) from Davao (2915), and 19th Army from Vogelkop (3219) invade Tarakan (2616), 1 ASP used; Tarakan will be a battle hex.
* Kongo (BB) moves from Cam Ranh (2311) to Tjilatjap (2019); Tjilatjap will be a battle hex.
* 25th Army (reduced) moves from Kota Bharu (2112) to Bangka (2017), 1 ASP used.

The idea of invading Amchitka is twofold: I want to force him to at least divert some resources to retaking Amchitka. And I want to be able to threaten Midway with a 2-card. Amchitka is in range of Combined Fleet HQ, exactly 10 hexes away from Midway, and there is no way for the Allies to block supplies to Amchitka.

The three other invasions are pretty self-explanatory I think; I take three more resource hexes and make use of the fact that my card is only a 2-card, so he can't react to any of the battle hexes in any way.

Tjilatjap was a bit concerning with a total attack strength of 14; so the Allies would be able to hit both battleships and carriers. But thanks to the card event, the total attack strength will drop to 11, minimizing my risks.

Let's see how these battles turn out.

**Reactions**: The Allies opt for an intel roll: 9. Back luck, so no reactions and I get a surprise attack.

**Battle Resolution**: There are three battle hexes to resolve:

* **Balikpapan** (A) and **Tarakan** (B): Since I only have naval and air units in both battle hexes, I get a +4DRM, which makes both battles an auto win for me.
* **Tjilatjap** (C): Japan 13 vs. Allies 11. Japan roll +3DRM for surprise attack: 9 for 13 hits and a critical hit. Japan uses the critical hit to destroy the Dutch air unit and remaining 4 hits to reduce US Asia (DD). Allies attack strength is now 6. Allies roll: 1 for 6 hits, no effect (6 hits lost).

The invasions of Bangka (2017) and Amchitka (4700) are unopposed and succeed.

**Post Battle Movement**: The Allies have no active unit eligible for PBM, so Japan conducts PBM next.

* Nachi (CA) moves to Miri (2415)
* Zuiho (CVL) moves back to Davao (3016)
* Kongo (BB) moves back to Cam Ranh (2311)
* 3rd Air Division moves to Balikpapan (2517)

I took out a total of three naval/air steps, and two ground steps while taking no losses. Further, three more resource hexes. All in all not a bad start, but I have already used four out of seven ASPs, so it's probably time to swing West for the Solomons and New Guinea. 
