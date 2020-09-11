---
layout: page
permalink: /gis-projects/california-wine
---
<link rel="stylesheet" href="/assets/css/style.css">
<h1>Finding the Best Winegrowing Regions in California</h1>
California is known for its wine countries, from Napa Valley all the way to Southern California.
For my final project in Intermediate GIS, I decided to perform a suitability analysis to see which
places in California are best suited for growing wine.

**See this project in video format!**
<div class="image-container">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/nLeyDbTNjc8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
<br>

To do this, I researched which factors are the most important when deciding on the location for your
vineyard. Though it varies based on the variety of grapes, temperature and soil drainage were consistently
referred to as being the most important. Temperature data was obtained from WorldClim at a resolution of
1 square kilometer, and soil drainage data was found in the USDA's SSURGO database.

Significant processing had to be done to the data. For temperature, one needs to look at the growing degree
days for each specific region, which is a metric computed by summing the average number of degrees Fahrenheit above 
50 at each location across the entire growing season, which is from April through October. The result is a number
which puts you on the following chart, and determines which grape varieties you could conceivably grow. Given that
higher amounts of growing degree days limit the grape varieties you can grow to table grapes, region II was determined
to be the most desirable.

This process was done using the raster calculator in QGIS.

<table>
    <tr>
        <th>Growing Degree Days</th>
        <th>Region</th>
        <th>Grape Varieties</th>
    </tr>
    <tr>
        <td>2000-2500</td>
        <td>I</td>
        <td>Chardonnay, Pinot Noir, Gewurztraminer, Reisling</td>
    </tr>
    <tr>
        <td>2500-3000</td>
        <td>II</td>
        <td>Cabernet Sauvignon, Merlot, Sauvignon Blanc</td>
    </tr>
    <tr>
        <td>3000-3500</td>
        <td>III</td>
        <td>Zinfandel, Barbera, Gamay</td>
    </tr>
    <tr>
        <td>3500-4000</td>
        <td>IV</td>
        <td>Malvasia, Thompson Seedless</td>
    </tr>
    <tr>
        <td>4000-4500</td>
        <td>V</td>
        <td>Thompson Seedless, other table grapes for eating</td>
    </tr>
</table>

Data from USDA's SSURGO database required a series of table joins to get into the correct format. The USDA
classifies soils in 7 different levels, ranging from 1 ("very poorly drained") to 7 ("excessively drained"). For wine,
a level of 5 ("well drained") is optimal.

A simple "suitability index" was created, ranking ideal conditions (well drained, region II) with a score of 5 and 
decreasing linearly as we get away from these conditions. When summing these two indices up, we get a score of 10 to rank each location based on how well it can support grapevines. This involved vector overlay analysis, as the temperature raster data was converted to vector before combining these two data sets.


**Summary**
* Classified temperature and soil drainage data on a scale from 1 to 5, based on how suited they are for grapevines
* Aggregated this data into a single metric indicating vineyard suitability for regions in California
* Tools used: raster operations, table joins, and overlay analysis


**Map 1: Growing Degree Days**

Recall that we chose region II as the ideal location for vineyards.
<img src="/assets/img/gis-projects/california-wine-0.jpg">

**Map 2: Soil Drainage Levels**

Recall that we chose "well drained" as the ideal drainage level for vineyards.
<img src="/assets/img/gis-projects/california-wine-1.jpg">

**Map 3: Overall Winegrowing Suitability**

<img src="/assets/img/gis-projects/california-wine-2.jpg">

**Maps 4-6: Comparing our results to current winery locations**

To conclude, I thought it would be interesting to evaluate how well California's current wineries have chosen their locations. In the following maps, we can see that most wineries are located in ideal conditions. However, a significant number of wineries in Southern California are in good but not ideal locations. This is interesting to note, and somewhat expected since temperatures are far more extreme during summer (which is the peak growing season) in Southern California than in Northern California. Furthermore, Southern California is more desertic, which means that soil drainage is typically
higher than in Northern California, as shown in map 2.
<img src="/assets/img/gis-projects/california-wine-3.jpg">
<img src="/assets/img/gis-projects/california-wine-4.jpg">
<img src="/assets/img/gis-projects/california-wine-5.jpg">
