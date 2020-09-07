---
layout: page
permalink: /gis-projects/darien-gap
---
<h1>Bridging the Darien Gap</h1>
The purpose of this project was to use **cost-distance analysis** to determine the most efficient road
through the Darien Gap, which is a dense and dangerous rainforest separating Panama and Columbia. The
Darien Gap is the only break in the Pan-American Highway, which stretches from northern Alaska to 
the southern tip of Argentina.

The factors evaluated were slope, bodies of water, and forested areas. Ideally, a road would be built
on flat ground without the need for bridges or significant deforestation to clear a path. Therefore,
the parameters provided to the cost-distance tool in ArcMap reflected these demands. This was configured
by gathering elevation data from the USGS Earth Explorer and forest cover data from the Japanese
Aerospace Exploration Agency's ALOS-2 project and performing raster analysis to aggregate the data.

The result is two
possible routes, which connect Yaviza, Panama to either Apartado, Columbia or Chigorodo, Columbia.

**Summary**
* Analyzes forests, bodies of water, and terrain slope to find a viable route between Panama and Columbia
* Tools used: Cost-distance, raster analysis and reclassification

**Map 1: General map of routes**
This first map shows the general path of the proposed routes, and the relevant highway systems that exist
in both Panama and Columbia for context.

<img src="/assets/img/gis-projects/darien-gap-0.jpg">

**Map 2: Routes with respect to forested areas**
This map puts into context two of the variables considered: forested areas and bodies of water. We can see
that the cost-distance algorithm efficiently routed the paths between forested areas and in such a way to 
minimize any bridge-building that would need to be done.

<img src="/assets/img/gis-projects/darien-gap-1.jpg">

**Map 3: Routes with respect to terrain slope**
Finally, this map puts the third variable into context, which is slope. This shows that a good balance was struck
between all three variables, since we are also able to efficiently route the proposed roads in areas of low slope.
One example is in the lower right corner, where we pass between two mountains before forking to either city in Columbia.

<img src="/assets/img/gis-projects/darien-gap-2.jpg">