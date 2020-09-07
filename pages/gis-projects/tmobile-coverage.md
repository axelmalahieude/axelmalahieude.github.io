---
layout: page
permalink: /gis-projects/tmobile-coverage
---
<h1>Evaluating T-Mobile's Cellular Coverage in Los Angeles</h1>
In this project, I used **viewshed analysis** to find out how effective T-Mobile's cellular service is in Los Angeles County. The purpose of this is to get a benchmark to then evaluate how to best improve T-Mobile's coverage. Potential methods of improving a cellular provider's coverage are by increasing the height of each cell tower, increasing the range of each cell tower, or introducing new cell towers in the region. Each of these three methods will then be compared to determine which results in the best improvement in T-Mobile's network. 

Viewshed analysis is used for this task because cell towers need a direct line of sight to the subscriber's phone in order to propagate their signal. Importantly, mountainous regions cause significant problems to this line of sight, which is where viewshed analysis comes in. 

T-Mobile cell tower location data was obtained from the company's website, and this included each tower's height. These towers are our "observer" points during the viewshed analysis process. It was assumed that each tower has a base range of 25 kilometers, and that each subscriber uses their cell phone at a height of 1.5 meters.

**Summary**
* Evaluated T-Mobile's cell coverage in LA county
* Determine which proposed method of network improvement (increasing range, tower height, or adding more towers) results in the most increase in covered area
* Tools used: viewshed analysis, advanced raster analysis, raster mosaicking


**Map 1: Raw cellular coverage**

Interestingly, the raw coverage of LA county is not as great as I would have imagined. 5,000 square kilometers are covered, which corresponds to only 41.1% of LA county's total area. However, on the map we see that most uncovered areas are in the mountain ranges, where population density is very low, so this is understandable. However, this leaves definite room for improvement in cell coverage, as the next few maps will try to evaluate.
<img src="/assets/img/gis-projects/viewshed-0.jpg">

**Map 2: Cellular coverage if increasing tower height by 10 meters**

This method of improving coverage results in marginal improvements. The newly covered area is 5,128 square kilometers, which is just 41.7%, 0.6% more than with the towers' original height. Perhaps the next methods will provide more improvement.
<img src="/assets/img/gis-projects/viewshed-1.jpg">

**Map 3: Cellular coverage if increasing tower range by 5 kilometers**

This method provides some improvement over the option of increasing tower height. The coverage area of LA county with this option is now 5,365 square kilometers, which represents 43.6% of LA county.
<img src="/assets/img/gis-projects/viewshed-2.jpg">

**Map 4: Cellular coverage if adding 3 new towers**

This method is by far the most efficient. Introducing 3 new towers in places where there is little to no cell coverage results in the most drastic improvement in covered area, coming out to almost 6,200 square kilometers, or just over 50% of LA county's total area. This is almost 9% more than the raw coverage, which is a significant amount, and now includes the city of Santa Clarita, which has a population of over 200,000 and can therefore represent a significant market for T-Mobile.
<img src="/assets/img/gis-projects/viewshed-3.jpg">