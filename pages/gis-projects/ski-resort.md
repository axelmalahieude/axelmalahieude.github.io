---
layout: page
permalink: /gis-projects/ca-ski-resort
---
<link rel="stylesheet" href="/assets/css/style.css">
<h1 id="top">Planning a New Ski Resort in California</h1>
For my final project in Advanced GIS, I chose to use the tools and methods we had learned throughout the course to plan a new ski resort in California. This is a two-step process; first, I'll go through a site selection process to find a mountain suitable for a ski resort. Then, using cost-distance analysis, the goal will be to plan out a few pistes and ski lifts. Finally, the results will be presented in an interactive web map.

I found this topic interesting because as someone who has skiied extensively throughout California, I was curious as to how much potential there is for growth in the ski industry. I was also intrigued by all the different factors that go into making good ski conditions.

**GIS Methods Used**
* Interpolation
* Site selection
* Cost-distance analysis
* Web mapping (using the <code>qgis2threejs</code> plugin)
* Raster reclassification, raster analysis, focal statistics

**Table of Contents**

These links will take you to the corresponding section, if you wish to skip around.
<ul>
    <li><a href="#site-selection">Stage 1: Selecting a Mountain</a></li>
        <ul>
            <li><a href="#elevation">Elevation</a></li>
            <li><a href="#slope">Slope</a></li>
            <li><a href="#aspect">Aspect</a></li>
            <li><a href="#temperature">Temperature</a></li>
            <li><a href="#snowfall">Snowfall</a></li>
            <li><a href="#maps1">Maps</a></li>
        </ul>
    <li><a href="#slope-mapping">Stage 2: Planning Ski Pistes</a></li>
</ul>

<h2 id="site-selection">Stage 1: Selecting a Mountain</h2><a href="#top">Back to top</a>

The first part of the ski resort planning process is to select a mountain. Naively, we might be tempted to just select a mountain in the Sierra Nevada and be done. However, when researching important factors in ski journals, the 6 following criteria must be considered:
* Elevation
* Slope
* Temperature
* Snowfall
* Aspect (angle of the slope relative to the sun)
* Accessibility (proximity to roads)

To make matters more complicated, elevation and slope not only have to be within specific bounds, but there should also be plenty of variety. For instance, to be able to support any skiier type, from beginner to expert, we should have both gentle slopes and more aggresive slopes. Likewise, it is good to have a large range of elevations so that we can ensure a high vertical distance between the base camp and the summit. The other three factors are less complicated but nevertheless important. Temperature is straightforward, and should be considered both for snow health and skiier comfort. Snowfall is also obvious, but with modern technology it is possible to overcome poor snowfall levels with snowmaking. This will be considered when evaluating snowfall throughout California. Finally, aspect is important in order balance out solar radiation. If we angle the ski pistes away from the sun, we can avoid any unnecessary melting and preserve higher-quality snow.

As for roads, there is no one-size-fits-all. To keep things simple, we will only consider regions within 5 miles of an existing highway. This will minimize any road-building costs, and avoids complex analysis of whether a road can feasibly be built given the extreme conditions as that is not the purpose of this project.

To perform this site selection, all 5 remaining factors will be evaluated equally, with a bonus point awarded for having good snowmaking conditions. Therefore, we can create an index for each criteria, scaled from 0 to 5 according to suitability, and simply sum them all up to create a final score out of 25 for how well that specific region could support a ski resort. The following sections will go in-depth on the analysis of each factor.

<h4>Data Collection</h4>
Data was collected as follows:
* Elevation: a digital elevation model (DEM) was sourced from the <a href="https://earthexplorer.usgs.gov/">USGS Earth Explorer</a> at a 90-meter resolution. Since we want to analyze all of California, 60 individual DEMs were downloaded and mosaicked using ArcMap. This large, newly-created DEM was then used to derive slope and aspect.
* Temperature: high-quality rasters representing average worldwide temperatures at a 1-kilometer resolution were obtained from <a href="https://www.worldclim.org/data/worldclim21.html">WorldClim</a>. This is the result of <a href="https://rmets.onlinelibrary.wiley.com/doi/abs/10.1002/joc.5086">research published in the International Journal of Climatology</a>, using complex interpolation techniques and satellite data. This data is far more accurate than anything I could have produced myself with interpolation, which is why this dataset was chosen.
* Snowfall: Unfortunately, snowfall data is not readily available as a raster. However, NOAA provides <a href="https://www.ncdc.noaa.gov/cdo-web/">weather data collected by stations throughout the United States</a> as point shapefiles. This data includes year-to-date snowfall levels, and can therefore be interpolated to suit our needs.

<h4 id="elevation">Elevation</h4>
As stated above, we would like to ensure a good vertical distance between base camp and the summit. This way, we know we'll have skiiable pistes from the summit to the base camp. The actual elevations are more or less irrelevant, as long as the region has good snow. To do this, we can look at each data point, and find the difference between the highest and lowest surrounding elevation. This can be done using focal statistics. Based on a list of North American ski resort statistics,<sup><a href="#fn1" id="ref1">1</a></sup> a good-sized ski resort will usually be at least 1000 acres, so the neighborhood for each point was set to be a circle of radius 2 kilometers. The results were then scored according to the following scale:
<table>
    <tr>
        <th>Vertical drop (feet, distance from highest to lowest nearby point)</th>
        <th>Score</th>
    </tr>
    <tr>
        <td>>3000</td>
        <td>5</td>
    </tr>
    <tr>
        <td>2000-3000</td>
        <td>4</td>
    </tr>
    <tr>
        <td>1000-2000</td>
        <td>3</td>
    </tr>
    <tr>
        <td>&#60;1000</td>
        <td>0</td>
    </tr>
</table>

<h4 id="slope">Slope</h4>
After computing slope from our DEM, the rest of this analysis was very similar to what we did for elevation. We would like to have plenty of variety in available piste slopes, to support all levels of skiiers. Therefore, I used focal statistics again for the same neighborhoods, but this time computed the standard deviation to allow us to look at the distribution rather than the raw values. Finally, I scored the results statistically. That is, the mean was scored at 3, and each standard deviation above and below the mean was increased and decreased respectively by 1 to favor places with high standard deviations of slopes.

<h4 id="aspect">Aspect</h4>
Analyzing the aspect was a much simpler process. In general, north-facing slopes are ideal. This is because in the northern hemisphere, the sun will shine more directly during the winter months on south-facing slopes, causing the snow to go through more significant thermal cycles. In other words, more sun means worse snow quality. This also has impacts on avalanche probability.<sup><a href="#fn2" id="ref2">2</a></sup> As such, the following straightfoward scale was used to reclassify the aspect, calculated from our mosaicked DEM:
<table>
    <tr>
        <th>Aspect</th>
        <th>Score</th>
    </tr>
    <tr>
        <td>North</td>
        <td>5</td>
    </tr>
    <tr>
        <td>Northeast, northwest</td>
        <td>4</td>
    </tr>
    <tr>
        <td>East, west</td>
        <td>3</td>
    </tr>
    <tr>
        <td>Southeast, southwest</td>
        <td>2</td>
    </tr>
    <tr>
        <td>South</td>
        <td>1</td>
    </tr>
</table>

<h4 id="temperature">Temperature</h4>
This was another straightforward reclassification step. Since we only care about the temperature during the ski season, we took advantage of WorldClim's offering of monthly averages, and took the average temperature during December, January, February, and March. With help from various articles,<sup><a href="#fn2" id="ref2">2</a></sup> the following scale was developed according to skiier comfort and the impact of temperature on snow conditions:
<table>
    <tr>
        <th>Temperature (Fahrenheit)</th>
        <th>Score</th>
    </tr>
    <tr>
        <td>15-25</td>
        <td>5</td>
    </tr>
    <tr>
        <td>25-32</td>
        <td>4</td>
    </tr>
    <tr>
        <td>32-40</td>
        <td>3</td>
    </tr>
    <tr>
        <td>40-45</td>
        <td>1</td>
    </tr>
    <tr>
        <td>45+</td>
        <td>0</td>
    </tr>
</table>

<h4 id="snowfall">Snowfall</h4>
Finally, the last criteria is how much snow a region has. As stated in the data collection section, the only available weather data that included snowfall was as point data from NOAA; therefore, the first step was to use inverse distance weighting interpolation to obtain estimated yearly snowfall throughout California. Very quickly, I realized that this process was less than ideal. Unfortunately, few NOAA weather stations are located in remote places where it snows frequently, which is understandable since fewer people live in those places. This means that our estimated snowfall values were lower than expected. Again, this is expected; it's difficult to interpolate a value higher than any of our data points. To get around this, I used the list of North American ski resort statistics to find typical yearly snowfall.<sup><a href="#fn1" id="ref1">1</a></sup> I compared the actual snowfall to the interpolated snowfall for various regions and found that the difference was actually very consistent. For instance, according to my interpolated snowfall values, Big Bear had roughly 40 inches of snowfall, and resorts in the Lake Tahoe area had around 150 inches of snowfall. In reality, these resorts have around 1.5x to 2x this snowfall. Therefore, I could still use my interpolated data, but I just needed to be more generous with my reclassification:
<table>
    <tr>
        <th>Yearly snowfall (inches)</th>
        <th>Score</th>
    </tr>
    <tr>
        <td>90-220</td>
        <td>5</td>
    </tr>
    <tr>
        <td>56-90</td>
        <td>4</td>
    </tr>
    <tr>
        <td>28-56</td>
        <td>3</td>
    </tr>
    <tr>
        <td>&#60;28</td>
        <td>0</td>
    </tr>
</table>

However, many ski resorts are able to lengthen their ski seasons and artificially increase snowfall with snowmaking.<sup><a href="#fn4" id="ref4">4</a></sup> Under the right conditions, "snow cannons" can spray highly pressured water into the air, which falls to the ground as snow. The ability to make snow, however, is highly dependent on temperature and humidity (which is why many resorts make snow only during the night).<sup><a href="#fn5" id="ref5">5</a></sup> A good metric of temperature and humidity is wet bulb temperature, so it is often used as a benchmark for determining snowmaking potential; snowmaking is generally only possible when the wet bulb temperature is below 27.5&#176;F.<sup><a href="#fn5" id="ref5">5</a></sup>

Using the <a href="https://journals.ametsoc.org/bams/article/98/9/1897/70218/Two-Simple-and-Accurate-Approximations-for-Wet">Stull Formula</a>, wet bulb temperature can be found computationally from relative humidity and temperature. Using vapor pressure data from WorldClim (the same source as the temperature data), average relative humidity between December and March was calculated throughout California. This was then used alongside temperature data as input to the Stull Formula, which results in our desired wet bulb temperature data. By filtering for wet bulb temperatures below 27.5&#176;F, I was able to find all regions in California where snowmaking is theoretically possible.

To incorporate this into our overall snowfall analysis, snowmaking was treated as a "bonus". That is, if a region has enough natural snowfall to support some level of skiing (i.e., scores 3, 4, or 5 in the table above), then their snowfall score was increased by one (e.g. a 4 becomes a 5). However, this was capped at 5 to avoid having scores of 6 out of 5.

<h4 id="maps1">Maps</h4>
The final step was to sum up all of these indices. After filtering out all places farther than 5 miles from an existing road, the following map was created:
<img src="/assets/img/gis-projects/final-ca.jpg"> 
Clearly, the best places for ski resorts are in the Sierra Nevada. Zooming in to this region gives us a better idea for where we should make our new ski resort:
<img src="/assets/img/gis-projects/final-sn.jpg">
<!-- One interesting thing I noted while going through this site selection process is how much area we lose by requiring that our ski resort be within 5 miles of an existing road. In hindsight, it makes sense, because there isn't a need for a road since the region is too mountainous to drive through, but it does mean that a significant amount of skiiable area is inaccessible.
<img src="/assets/img/gis-projects/final-sn-all.jpg"> -->
The area I will choose for this new ski resort is shown in this following map. After deliberation about which of the blue areas are to pick, I found this one to be the most promising. It is right off the border of a major highway, which means little to no road will need to be built, aside from a parking lot. It is also placed very close to large cities in the Tahoe area, including some in Nevada. The other promising locations I could have chosen is the area just southeast of Bear Valley and the area west of Independence. However, despite the fact that these areas are very close to a road, the road leading to these areas is incredibly windy and it would take visitors significantly longer to drive to the resort.
<img src="/assets/img/gis-projects/final-site2.jpg">

<h2 id="slope-mapping">Stage 2: Planning Ski Pistes</h2><a href="#top">Back to top</a>

<hr>
<sup id="fn1">1. <a href="https://en.wikipedia.org/wiki/Comparison_of_North_American_ski_resorts">https://en.wikipedia.org/wiki/Comparison_of_North_American_ski_resorts</a>&nbsp;&nbsp;<a href="ref1">&#8593;</a></sup><br>
<sup id="fn2">2. <a href="https://avalanche.org/avalanche-encyclopedia/aspect/">https://avalanche.org/avalanche-encyclopedia/aspect/</a>&nbsp;&nbsp;<a href="ref2">&#8593;</a></sup><br>
<sup id="fn3">3. <a href="https://www.snow-forecast.com/roundups/what-makes-the-perfect-skiing-conditions">https://www.snow-forecast.com/roundups/what-makes-the-perfect-skiing-conditions</a>&nbsp;&nbsp;<a href="ref3">&#8593;</a></sup><br>
<sup id="fn4">4. <a href="https://www.economist.com/business/2017/02/09/snow-making-companies-in-a-warming-world">https://www.economist.com/business/2017/02/09/snow-making-companies-in-a-warming-world</a>&nbsp;&nbsp;<a href="ref4">&#8593;</a></sup><br>
<sup id="fn5">5. <a href="https://www.technoalpin.com/en/faq.html">https://www.technoalpin.com/en/faq.html</a>&nbsp;&nbsp;<a href="ref5">&#8593;</a></sup>