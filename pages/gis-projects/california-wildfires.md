---
layout: page
permalink: /gis-projects/california-wildfires-august-2020
---
<link rel="stylesheet" href="/assets/css/style.css">
<h1>3D Mapping California Wildfires in August 2020</h1>
In August 2020, there were several large fires burning throughout the San Francisco Bay Area. As part of a 3D web mapping project, I decided to use the latest available data to create a 3D visualization of the situation. This web map presents the extent of the wildfires in the southeastern Bay Area as of August 27th, 2020. Air quality, major cities, affected buildings, and major roads are also shown. All of this data is mapped atop a digital elevation model to show relief.

To perform this analysis, I first had to gather fire data from the <a href="https://data-nifc.opendata.arcgis.com/">National Interagency Fire Center</a>, which includes very up-to-date GIS data relating to wildfires. Air quality was obtained from the <a href="https://www.epa.gov/outdoor-air-quality-data/download-daily-data">EPA</a>, and finally, roads and buildings were obtained from OpenStreetMap and extracted as needed.

The remainder of this project was quite simple. I clipped and extracted all the necessary data for the region I was working on, reclassified the air quality levels on a scale from "good" to "poor" based on PM2.5 levels, and configured the webmap using the `qgis2threejs` plugin for QGIS.

**Summary**
* Web map of fires in the San Fransisco Bay Area in August 2020
* Uses the qgis2threejs plugin to generate the web map
* Includes 3-dimensional features and labels for context

**Web map**

<a href="/webmap" target="_blank">Click here to interact with a full-page version of this map!</a>

You can scroll in the blue space to zoom in and out, and pan or click around to interact.
<iframe src="/webmap" width="100%" height="500px"></iframe>

<br>
<br>

**Static map showing the survey area**

<img src="/assets/img/gis-projects/california-wildfires.jpg">