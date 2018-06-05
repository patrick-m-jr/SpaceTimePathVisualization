# SpaceTimePathVisualization
Introduction to a Space-Time Path Visualization.
## Basics

Inspired by Mei Po Kwan's Space-Time Path visualizations.

<img src= "images/arcgisAppStudio1.png" width = "500">

## Updating Three Sections of the QT Code: Map Background, URL for MapServer, and Viewpoint (center, scale, and spatialreference) parameters. These are indicated by the red arrows. 

<img src= "images/qmlCodeSample.png" width = "500">

### 1. Basemap options: Update the basemap accordingly. A <a href = "https://developers.arcgis.com/qt/latest/qml/api-reference/qml-esri-arcgisruntime-basemap.html" target ="_blank"> list </a> of options can be found here. 

```
BasemapTopographic
BasemapDarkGrayCanvasVector
etc.
```

### 2. Updating "ArcGISMapImageLayer" Object

This is a simple update. It requires a "Map Service URL" that is accessible (without secure access) and shared publicly. I attempted to call a few different map services, see below for types and considerations (skip to c for the layer that worked). By default, this app accesses the layers in the map service regardless of number of layers. 

  a. This first option errored out when I ran the application. My assumption is that it is because it is calling a Map Service I created a hosted on a secure platform. I tried to 'Add Item' to my ArcGIS Online and embed security credentials but it was still erroring out. (<a href="http://communityhub.esriuk.com/technicalsupport/2014/7/22/how-to-use-arcgis-server-services-in-arcgis-online.html" target="_blank">see this link</a>)
```
//url: "https://ec2-54-68-22-196.us-west-2.compute.amazonaws.com:6443/arcgis/rest/services/Murphy/graffiti_CostMap_StPaulMN_2015/MapServer"


```

  b. The Map Server called here is a result of publishing a Feature Layer on ArcGIS Online as a Tile Layer. However, there was an issue with visibility as the application ran successfully and acknowledged the layer in the provided Legend but did not show. 

```
url: "https://services3.arcgis.com/J1Locv0GPOt6yBRR/arcgis/rest/services/BTS_Reportings_Tile_Layer/MapServer"
```
  c. I ended up configuring the application with a Map Service hosted on ESRI sampleserver6. 
```
url: "http://sampleserver6.arcgisonline.com/arcgis/rest/services/911CallsHotspot/MapServer"
```
<img src= "images/mapServerOptions.png" width = "500">


### 3. Updating "initialViewpoint: ViewpointCenter" Coordinate and Target Scale parameters

I updated this section of code so that the app opens centered on the location and subsequently the data. The format of the coordinates and targetScale can be condensed in scientific format where '11e6' == '11000000'

```
                    initialViewpoint: ViewpointCenter {
                        center: Point {
                            //x moves left/right. 
                            x: 7610000//6e6 //-11e6
                            //y moves up/down
                            y: 680000 //6e6
                            spatialReference: SpatialReference {wkid: 102726}
                        }
                        targetScale: 250000
                    }
```
### 4. Updating "initialViewpoint: ViewpointCenter": Spatial Reference parameters

Technically, this could be considered a substep of #3, however, I dubbed it is own step because it's important to match coordinate systems. This map in particular used "NAD_1983_StatePlane_Oregon_North_FIPS_3601_Feet." Make sure that the Spatial Referene on your Map Service matches your map object.

```
                    initialViewpoint: ViewpointCenter {
                        center: Point {
                            //x moves left/right. 
                            x: 7610000//6e6 //-11e6
                            //y moves up/down
                            y: 680000 //6e6
                            
        ***Don't forget to update the spatial reference to the map service being called. ***
                            spatialReference: SpatialReference {wkid: 102726}
                        }
                        targetScale: 250000
                    }
```
# Save your edits and re-run the app in AppStudio.
In my example, I have adjusted the data that is displayed, the scale, and viewpoint center. The app now shows 911 calls in the Portalnd, OR area. 

<img src= "images/911HotSpotsApp.png" width = "350">



## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc
