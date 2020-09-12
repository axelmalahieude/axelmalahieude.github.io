---
layout: page
permalink: /gis-projects/ca-ski-resort
---
<link rel="stylesheet" href="/assets/css/style.css">
<h1 id="top">Planning a New Ski Resort in California</h1>
For my final project in Advanced GIS, I chose to use the tools and methods we had learned throughout the course to plan a new ski resort in California. This is a two-step process; first, I'll go through a site selection process to find a mountain suitable for a ski resort. Then, using cost-distance analysis, the goal will be to plan out a few slopes and ski lifts. To better visualize these results, the final product will be presented in an interactive web map.

I found this topic interesting because as someone who has skied extensively throughout California, I was curious as to how much potential there is for growth in the ski industry. I was also intrigued by all the different factors that go into making good ski conditions.

Check out this project in video format!

<div class="image-container">
<iframe width="560" height="315" src="https://www.youtube.com/embed/cgSS6tm9Ip8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
<br>
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
    <li><a href="#slope-mapping">Stage 2: Planning the Ski Area</a></li>
        <ul>
            <li><a href="#lifts">Chair Lifts</a></li>
            <li><a href="#slopes">Ski Slopes</a></li>
            <li><a href="#ski-map">Ski Area Map</a></li>
            <li><a href="#ski-webmap">Webmap</a></li>
        </ul>
    <li><a href="#conclusion">Conclusion</a></li>
</ul>

Note that all maps on this page can be clicked to view a full-size PDF copy.

<h2 id="site-selection">Stage 1: Selecting a Mountain</h2><a href="#top">Back to top</a>

The first part of the ski resort planning process is to select a mountain. Naively, we might be tempted to just select a mountain in the Sierra Nevada and be done. However, when researching important factors in ski journals, the 6 following criteria must be considered:
* Elevation
* Slope
* Temperature
* Snowfall
* Aspect (angle of the slope relative to the sun)
* Accessibility (proximity to roads)

To make matters more complicated, elevation and slope not only have to be within specific bounds, but there should also be plenty of variety. For instance, to be able to support any skier type, from beginner to expert, we should have both gentle slopes and more aggressive slopes. Likewise, it is good to have a large range of elevations so that we can ensure a high vertical distance between the base camp and the summit. The other three factors are less complicated but nevertheless important. Temperature is straightforward, and should be considered both for snow health and skier comfort. Snowfall is also obvious, but with modern technology it is possible to overcome poor snowfall levels with snowmaking. This will be considered when evaluating snowfall throughout California. Finally, aspect is important in order balance out solar radiation. If we angle the ski slopes away from the sun, we can avoid any unnecessary melting and preserve higher-quality snow.

As for roads, there is no one-size-fits-all. To keep things simple, we will only consider regions within 5 miles of an existing highway. This will minimize any road building costs, and avoids complex analysis of whether a road can feasibly be built given the extreme conditions as that is not the purpose of this project.

To perform this site selection, all 5 remaining factors will be evaluated equally, with a bonus point awarded for having good snowmaking conditions. To do this, we can create an index for each criteria, scaled from 0 to 5 according to suitability, and simply sum them all up to create a final score out of 25 for how well that specific region could support a ski resort. The following sections will go in-depth on the analysis of each factor.

<h4>Data Collection</h4>
Data was collected as follows:
* Elevation: a digital elevation model (DEM) was sourced from the <a href="https://earthexplorer.usgs.gov/">USGS Earth Explorer</a> at a 90-meter resolution. Since we want to analyze all of California, 60 individual DEMs were downloaded combined into one. Using the elevation data contained in the DEM, slope and aspect were then obtained.
* Temperature: high-quality rasters representing average worldwide temperatures at a 1-kilometer resolution were obtained from <a href="https://www.worldclim.org/data/worldclim21.html">WorldClim</a>. This is the result of <a href="https://rmets.onlinelibrary.wiley.com/doi/abs/10.1002/joc.5086">research published in the International Journal of Climatology</a>, using complex interpolation techniques and satellite data. This data is far more accurate than anything I could have produced myself with interpolation, which is why this dataset was chosen.
* Snowfall: Unfortunately, snowfall data is not readily available as a raster. However, NOAA provides <a href="https://www.ncdc.noaa.gov/cdo-web/">weather data collected by stations throughout the United States</a> as point shapefiles. This data includes year-to-date snowfall levels, and can therefore be interpolated to suit our needs.

<h4 id="elevation">Elevation</h4>
As stated above, we would like to ensure a good vertical distance between base camp and the summit. This way, we know we'll have skiable slopes from the summit to the base camp. The actual elevations are more or less irrelevant, as long as the region has good snow. To do this, we can look at each data point, and find the difference between the highest and lowest surrounding elevation. This can be done using focal statistics. Based on a list of North American ski resort statistics,<sup><a href="#fn1" id="ref1">1</a></sup> a good-sized ski resort will usually be at least 1000 acres, so the neighborhood for each point was set to be a circle of radius 2 kilometers. The results were then scored according to the following scale, also based on trends in existing ski resorts:
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
After computing slope from our DEM, the rest of this analysis was very similar to what we did for elevation. We would like to have plenty of variety in available ski trails, to support all levels of skiers. Therefore, I used focal statistics again for the same neighborhoods, but this time computed the standard deviation to allow us to look at the distribution rather than the raw values. Finally, I scored the results statistically. That is, the mean was scored at 3, and each standard deviation above and below the mean was increased and decreased respectively by 1 to favor places with high standard deviations of slopes.

<h4 id="aspect">Aspect</h4>
Analyzing the aspect was a much simpler process. In general, north-facing slopes are ideal. This is because in the northern hemisphere, the sun will shine more directly during the winter months on south-facing slopes, causing the snow to go through more significant thermal cycles. In other words, more sun means worse snow quality. This also has impacts on avalanche probability.<sup><a href="#fn2" id="ref2">2</a></sup> As such, the following straightforward scale was used to reclassify the aspect, calculated from our mosaicked DEM:
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
This was another straightforward reclassification step. Since we only care about the temperature during the ski season, we took advantage of WorldClim's offering of monthly averages, and took the average temperature during December, January, February, and March. With help from various articles,<sup><a href="#fn3" id="ref3">3</a></sup> the following scale was developed according to skier comfort and the impact of temperature on snow conditions (note that no areas in California had an average temperature below 15&deg;):
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
The final step was to sum up all of these indices. The focus is placed on locations that scored 17 or over since otherwise the area will score too low in too many of the 5 mentioned criteria to be worth having a ski resort at. After filtering out all places farther than 5 miles from an existing road, the following map was created:
<div class="tooltip" title="Click to see a full-size version of this map" onclick="window.open('/assets/pdf/california_areas.pdf', '_blank')">
    <img src="/assets/img/gis-projects/final-ca.jpg">
</div> 
Clearly, the best places for ski resorts are in the Sierra Nevada. Zooming in to this region gives us a better idea for where we should make our new ski resort:
<div class="tooltip" title="Click to see a full-size version of this map" onclick="window.open('/assets/pdf/sierra_nevada_areas.pdf', '_blank')">
    <img src="/assets/img/gis-projects/final-sn.jpg">
</div> 
<!-- One interesting thing I noted while going through this site selection process is how much area we lose by requiring that our ski resort be within 5 miles of an existing road. In hindsight, it makes sense, because there isn't a need for a road since the region is too mountainous to drive through, but it does mean that a significant amount of skiiable area is inaccessible.
<img src="/assets/img/gis-projects/final-sn-all.jpg"> -->
The area I will choose for this new ski resort is shown in this following map. After deliberation about which of the top-scoring areas to pick, I found this one to be the most promising. It is located south of South Lake Tahoe and northeast of Kirkwood, on the north-facing slope of a mountain whose peak is roughly 3,000 feet above where the base camp would be. It is right off the border of a major highway, which means little to no road will need to be built, aside from a parking lot. It is also placed very close to large cities in the Tahoe area, including some in Nevada. The other promising locations I could have chosen are the area just southeast of Bear Valley and the area west of Independence. However, despite the fact that these areas are very close to a road, the road leading to these areas is incredibly windy and it would take visitors significantly longer to drive to the resort.
<div class="tooltip" title="Click to see a full-size version of this map" onclick="window.open('/assets/pdf/site_proposal.pdf', '_blank')">
    <img src="/assets/img/gis-projects/final-site.jpg">
</div> 
<div class="tooltip" title="Click to see a full-size version of this map" onclick="window.open('/assets/pdf/site_proposal_terrain.pdf', '_blank')">
    <img src="/assets/img/gis-projects/final-site-terrain.jpg">
</div> 

<h2 id="slope-mapping">Stage 2: Planning the Ski Area</h2><a href="#top">Back to top</a>

Now that we have selected our mountain, it's time to map out some ski slopes. Fortunately, because we chose a mountain that scored so highly on our 25-point scale, there are a few assumptions we can make. Firstly, we don't have to worry about snow quality or aspect along the slope, since that's already guaranteed to be in good conditions. Secondly, since we specifically chose a location with a good variety of slopes and high vertical drop, we know that there must exist downhill trails from a summit to one or more lower-elevation areas. Therefore, our slope mapping process comes down to selecting a few flat areas for chair lifts and ensuring that whatever trails we pick never go uphill.

<h4 id="lifts">Chair Lifts</h4>
Any ski resort needs chair lifts to transport skiers back to the top of runs. For this infrastructure, we need a small flat area for both the departure and arrival locations of the chair lifts; using the slope data derived from our digital elevation model, several flat areas were identified. These locations will be also used as the start and end positions for our ski slopes, to ensure skiers do not end up stranded at the bottom of their run.

<h4 id="slopes">Ski Slopes</h4>
The difficult part of picking a ski slope is ensuring it goes downhill for the entirety of the run. Otherwise, significant costs will need to be taken to fill or dig out any uphill areas, as these would present obstacles to skiers. A helpful tool for this is cost-distance analysis. In short, we define weights, or costs, of traversing from one point to another point, and the cost-distance algorithm uses these weights to find the "cheapest" path between two points. In our case, these weights are differences in elevation. By making positive differences in elevation (uphill) "cost" more than negative differences in elevation (downhill), we ensure that the cheapest path will be one that maximizes downhill sections. This process was done several times, each time with different chair lifts as our origin and destinations, in order to generate all of our ski slopes.

<p><strong>Technical Details</strong></p>
After some trial and error, I found that simply using the DEM as the cost raster to normal cost-distance analysis did not guarantee downhill trails; instead, the trails were just straight lines between points. However, elevation differences are natively supported by ArcMap's cost-distance tool by taking advantage of <a href="https://desktop.arcgis.com/en/arcmap/10.3/tools/spatial-analyst-toolbox/how-the-horizonal-and-vertical-factors-affect-path-distance.htm">vertical factors and path-distance analysis</a>. This requires specifying a depressionless DEM and makes an actual cost raster optional, since the vertical factors themselves are treated as costs. My use of this tool is a somewhat unconventional use of path-distance, because the entire premise of path-distance is that the <a href="https://pro.arcgis.com/en/pro-app/tool-reference/spatial-analyst/understanding-path-distance-analysis.htm#GUID-839042F2-B1AB-4D4F-A171-7DCE89F4A762">distance traveled changes when an incline is present</a>, but it works for ensuring a downhill slope in this case since the methodology is similar (skiing uphill would mean skiing a longer distance to get to the bottom of the trail). I started by making a depressionless DEM by eliminating sinks with the hydrology tools. Though a cost raster is optional, I decided to use an inverted slope raster as my costs in order to favor steeper downhill slopes over gentler downhill slopes (inverting the slope raster gives lower values to steeper slopes, making them "cheaper").

<p><strong>Checking the Results</strong></p>
Now that I had generated all of these paths down the mountain, I wanted to verify that they were in fact downhill. To do this, I generated a stack profile for each slope. This means that for each slope, I plotted the distance along the slope against the elevation at that point. The expectation is that as our distance increases, elevation only decreases and never increases. 
<div class="image-container">
    <img height="75%" width="75%" src="/assets/img/gis-projects/final-stack-profile.png">
</div>
<br>
This graph shows the elevation profile of Run 7, which goes from Chair Lift A down to Chair Lift D (see map below), and confirms that it is indeed a downhill trail, which is exactly what we wanted. By processing this data in Excel, I found that the average slope along this trail is 41°. Since ski slopes are ranked in terms of difficulty by their steepest and most difficult section, this puts Run 7 in the "black diamond" category. While other factors go into trail difficulty, such as obstacles, bumps, and any narrow sections, these can only be identified by being on-site. For the purposes of this project, the steepness of trails will be sufficient to determine trail difficulty.
<table>
    <tr>
        <th>Slope incline (degrees)</th>
        <th>Difficulty rating</th>
    </tr>
    <tr>
        <td>6-25</td>
        <td>Green circle (beginner)</td>
    </tr>
    <tr>
        <td>25-40</td>
        <td>Blue square (intermediate)</td>
    </tr>
    <tr>
        <td>&gt;40</td>
        <td>Black diamond (advanced)</td>
    </tr>
</table>
This analysis was repeated for the 7 other runs created using cost-distance analysis to verify the outcomes, resulting in 8 total ski slopes.

<h4 id="ski-map">Ski Area Map</h4>
The results of this analysis are shown below, with slopes highlighted according to difficulty and chair lifts depicted in red.

<div class="tooltip" title="Click to see a full-size version of this map" onclick="window.open('/assets/pdf/ski_area_map.pdf', '_blank')">
    <img src="/assets/img/gis-projects/final-ski-area.jpg">
</div>

<h4 id="ski-webmap">Ski Resort Webmap</h4>
To get a better visual understanding of this newly created ski area I created, I turned the results into a webmap. You can scroll in the blue area to zoom in and out, right click and drag to pan, and left click and drag to rotate. You can also click on all of the ski slopes and chair lifts and see more information, such as average slope, elevation, and distance. All units are in feet.

<a href="/webmap-final" target="_blank">Click here</a> to see a full-size version of this webmap. Note that it may some time to load the webmap, depending on your internet connection.

<iframe src="/webmap-final" width="100%" height="500px"></iframe>


<h4 id="conclusion">Conclusion</h4><a href="#top">Back to top</a>

In conclusion, I was able to find a place for a new ski resort in California and then design a ski area for the chosen location. Overall, the results are satisfying; though the site selection results initially surprised me by how few ideal ski areas there are, and made me wonder whether the selection criteria was too harsh, this actually helped in quickly finding a suitable place to build a resort. I was especially pleased with being able to look at the focal characteristics of areas with respect to elevation and slopes; it seems that this helped in being able to determine whether an area would be able to have downhill ski trails, since in the second portion of this project I had no problems with finding adequate slopes.

It was also helpful that there existed a variant of cost-distance analysis that is able to take into account the elevation of the path. However, if I were to repeat this project, I would try to be more careful with the types of ski trails that are mapped. In this project, I added a cost raster with the goal of obtaining the steepest trails possible. However, this is to the detriment of less-advanced skiers because if the geography of the selected mountain had been slightly different perhaps only black diamond-level trails would have been produced.

Nevertheless, I learned a lot about ski resorts throughout this project, and was happy to have been able to create a trail map for a potential ski resort.

<a href="#top">Back to top</a>

<hr>
<sup id="fn1">1. <a href="https://en.wikipedia.org/wiki/Comparison_of_North_American_ski_resorts">https://en.wikipedia.org/wiki/Comparison_of_North_American_ski_resorts</a>&nbsp;&nbsp;<a href="#ref1">&#8593;</a></sup><br>
<sup id="fn2">2. <a href="https://avalanche.org/avalanche-encyclopedia/aspect/">https://avalanche.org/avalanche-encyclopedia/aspect/</a>&nbsp;&nbsp;<a href="#ref2">&#8593;</a></sup><br>
<sup id="fn3">3. <a href="https://www.snow-forecast.com/roundups/what-makes-the-perfect-skiing-conditions">https://www.snow-forecast.com/roundups/what-makes-the-perfect-skiing-conditions</a>&nbsp;&nbsp;<a href="#ref3">&#8593;</a></sup><br>
<sup id="fn4">4. <a href="https://www.economist.com/business/2017/02/09/snow-making-companies-in-a-warming-world">https://www.economist.com/business/2017/02/09/snow-making-companies-in-a-warming-world</a>&nbsp;&nbsp;<a href="#ref4">&#8593;</a></sup><br>
<sup id="fn5">5. <a href="https://www.technoalpin.com/en/faq.html">https://www.technoalpin.com/en/faq.html</a>&nbsp;&nbsp;<a href="#ref5">&#8593;</a></sup>