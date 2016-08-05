---
layout: post
title: MTA Traffic
---

Here's a brief exploration into the realm of traffic on the MTA. We all know there's traffic on the 4,5,6 but just how do those numbers stack up to the rest of the city? More specifically, I investigated a hypothetical scenario for street outreach to wealthy donors. Yes, perhaps this is a misguided cause to start with. After all, how many wealthy donors will you really successfully solicit on the street or at subway stations? Nonetheless, this initial analysis could prove fruitful for determining preliminary target areas and more marketing research on how to best approach potential donors could then be conducted. 

In answering this question, (where is there a high volume of wealthy potential donors) I merged and cleaned several datasets in order to make an interactive visualization. This included downloading income data from the 2014 American Community Survey and joining that with census tract Tiger files. This produced the first layer of the map which depicts the percentage of households earning over $100K/year for a given census tract. From there, I then produced a second layer by aggregating and cleaning MTA turnstile data. Here's an initial view produced using CartoDB's torque map:

<iframe width="100%" height="520" frameborder="0" src="https://matthewbmitchell.carto.com/viz/935eff94-497a-11e6-a6da-0e05a8b3e3d7/embed_map" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe>

Much more to come including more updates and details on the MTA turnstile data. Stop by regularly for more ramblings on data science!
