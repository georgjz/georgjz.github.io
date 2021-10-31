---
layout:     post
title:      "The Wasp: Designing a Front Panel Computer with VHDL, Part 5"
date:       2019-03-01
excerpt:    "The Wasp learns to read data from RAM with a simple control signal generator"
tags:       [electronics, VHDL, hardware design, wasp]
feature:    /assets/wasp/04_systemsketchaddrac.jpg
published:  true
comments:   true
---
### Prelude

<figure>
    <a href="{{ "/assets/wasp/05_examine_timingdiagram.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/wasp/05_examine_timingdiagram.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>The <i>Wasp</i> so far</figcaption>
</figure>

<figure>
    <a href="{{ "/assets/wasp/05_examine_timingdiagram_marked.png" | uri_escape | absolute_url }}">
        <img src="{{ "/assets/wasp/05_examine_timingdiagram_marked.png" | uri_escape | absolute_url }}">
    </a>
    <figcaption>The <i>Wasp</i> so far</figcaption>
</figure>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border-color:#999;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;border-top-width:1px;border-bottom-width:1px;border-color:#999;color:#444;background-color:#F7FDFA;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;border-top-width:1px;border-bottom-width:1px;border-color:#999;color:#fff;background-color:#26ADE4;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-b3vi{background-color:#f7fdfa;text-align:center}
.tg .tg-7btt{font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-jmht{background-color:#D2E4FC;font-weight:bold;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-j0tj{background-color:#D2E4FC;text-align:center;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
.tg .tg-svo0{background-color:#D2E4FC;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-5hgy{background-color:#D2E4FC;text-align:center}
</style>
<table class="tg">
  <tr>
    <th class="tg-7btt" colspan="13">State Transition Table</th>
  </tr>
  <tr>
    <td class="tg-jmht" colspan="3">Current State</td>
    <td class="tg-jmht">Input</td>
    <td class="tg-jmht" colspan="3">Next State</td>
    <td class="tg-j0tj" colspan="6"><span style="font-weight:bold">Output</span></td>
  </tr>
  <tr>
    <td class="tg-7btt">A</td>
    <td class="tg-7btt">B</td>
    <td class="tg-7btt">C</td>
    <td class="tg-7btt">0</td>
    <td class="tg-7btt">A+</td>
    <td class="tg-7btt">B+</td>
    <td class="tg-7btt">C+</td>
    <td class="tg-0lax"></td>
    <td class="tg-0lax"></td>
    <td class="tg-0lax"></td>
    <td class="tg-0lax"></td>
    <td class="tg-0lax"></td>
    <td class="tg-0lax"></td>
  </tr>
  <tr>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-5hgy" colspan="6" rowspan="2">111110</td>
  </tr>
  <tr>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-b3vi" colspan="6" rowspan="2">011110</td>
  </tr>
  <tr>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
  </tr>
  <tr>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">1</td>
    <td class="tg-5hgy" colspan="6" rowspan="2">111110</td>
  </tr>
  <tr>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-b3vi" colspan="6" rowspan="2">101110</td>
  </tr>
  <tr>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">0</td>
  </tr>
  <tr>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-5hgy" colspan="6" rowspan="2">100100</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-b3vi" colspan="6" rowspan="2">100101</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
  </tr>
  <tr>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-5hgy" colspan="6" rowspan="2">100100</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
  </tr>
  <tr>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">1</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-svo0">0</td>
    <td class="tg-b3vi" colspan="6" rowspan="2">111110</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">1</td>
  </tr>
</table>
the real mumu:

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border-color:#aabcfe;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#aabcfe;color:#669;background-color:#e8edff;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#aabcfe;color:#039;background-color:#b9c9fe;}
.tg .tg-baqh{text-align:center;vertical-align:top}
.tg .tg-7k3a{background-color:#D2E4FC;font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-j0tj{background-color:#D2E4FC;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-baqh" colspan="5">Truth Table for A</th>
  </tr>
  <tr>
    <td class="tg-7k3a">I</td>
    <td class="tg-7k3a">A</td>
    <td class="tg-7k3a">B</td>
    <td class="tg-7k3a">C</td>
    <td class="tg-7k3a">Next A</td>
  </tr>
  <tr>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
  </tr>
  <tr>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">0</td>
  </tr>
  <tr>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
  </tr>
  <tr>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">0</td>
  </tr>
  <tr>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
  </tr>
  <tr>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">0</td>
  </tr>
  <tr>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">1</td>
  </tr>
  <tr>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">1</td>
  </tr>
  <tr>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">1</td>
  </tr>
  <tr>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
  </tr>
  <tr>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">0</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">0</td>
    <td class="tg-baqh">0</td>
  </tr>
  <tr>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
    <td class="tg-j0tj">1</td>
  </tr>
</table>
Text mumu:

[1]: {{ site.baseurl }}{% post_url 2019-02-14-wasp-02 %}
[2]: https://en.wikipedia.org/wiki/Black_box
