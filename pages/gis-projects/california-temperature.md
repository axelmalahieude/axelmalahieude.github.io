---
layout: page
permalink: /gis-projects/california-temperature
---
<h1>Evaluating Temperatures in California Using Interpolation</h1>
In this project, I used **interpolation** to find out how various regions of California
compare in terms of extreme temperatures. To get a good idea for how temperature differs,
I looked at the high temperatures in both January and August, to see which places are the
coldest and which are the warmest. This might be useful for someone considering where to 
live in California.

Data was collected from NOAA as point data from each of the weather stations in California.
Next, IDW and kriging interpolation were used to create rasters, which were then classified
to create informative maps. Finally, important cities throughout the state were highlighted
to provide some context for the maps.

**Summary**
* Interpolated data from NOAA to evaluate where in California it is hottest and coldest throughout the year
* Tools used: interpolation (both inverse distance weighting and kriging), raster analysis

A list of the coldest cities in California in January, using the interpolated data:
<table>
    <tr>
        <th>City</th>
        <th>Estimated High Temperature</th>
    </tr>
    <tr>
        <td>Sunnyside-Tahoe City</td>
        <td>40.02</td>
    </tr>
    <tr>
        <td>Lee Vining</td>
        <td>40.16</td>
    </tr>
    <tr>
        <td>Chilcoot-Vinton</td>
        <td>	40.56</td>
    </tr>
    <tr>
        <td>Mammoth Lakes</td>
        <td>40.56</td>
    </tr>
    <tr>
        <td>Bridgeport</td>
        <td>40.77</td>
    </tr>
    <tr>
        <td>Cedarville</td>
        <td>40.92</td>
    </tr>
    <tr>
        <td>Chester</td>
        <td>40.93</td>
    </tr>
    <tr>
        <td>Susanville</td>
        <td>41.00</td>
    </tr>
    <tr>
        <td>Mono City</td>
        <td>41.03</td>
    </tr>
    <tr>
        <td>Lake City</td>
        <td>41.05</td>
    </tr>
</table>

A list of the hottest cities in California in August, using the interpolated data:
<table>
    <tr>
        <th>City</th>
        <th>Estimated High Temperature</th>
    </tr>
    <tr>
        <td>Furnace Creek</td>
        <td>114.26</td>
    </tr>
    <tr>
        <td>Mecca</td>
        <td>107.68</td>
    </tr>
    <tr>
        <td>Baker</td>
        <td>107.53</td>
    </tr>
    <tr>
        <td>Blythe</td>
        <td>107.41</td>
    </tr>
    <tr>
        <td>Palo Verde</td>
        <td>106.77</td>
    </tr>
    <tr>
        <td>Needles</td>
        <td>106.74</td>
    </tr>
    <tr>
        <td>Oasis</td>
        <td>106.32</td>
    </tr>
    <tr>
        <td>Bluewater</td>
        <td>106.29</td>
    </tr>
    <tr>
        <td>Imperial</td>
        <td>106.23</td>
    </tr>
    <tr>
        <td>Indio</td>
        <td>106.20</td>
    </tr>
</table>

**Map 1: High temperatures in January, found using inverse distance weighting interpolation**

The first map was created using inverse distance.
<img src="/assets/img/gis-projects/california-temperature-0.jpg">

**Map 2: High temperatures in August, found using kriging interpolation**

This map was made using kriging, so that I could learn how to use a different interpolation technique
and compare it against inverse distance weighting.
<img src="/assets/img/gis-projects/california-temperature-2.jpg">
