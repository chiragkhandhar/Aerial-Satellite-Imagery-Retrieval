# Aerial-Satellite-Imagery-Retrieval

1. Chirag Khandhar  
2. Akshay Kulkarni
3. Megha Tatti

## The Bing Map Tile System

* We are using **Bing Map Tile System** that provides a pre render World Map at multiple levels of details.
* We provide a **lat-lon bounding box** and **Level of detail** to cut each map into tiles for quick retrieval.
* To make the map seamless, and to ensure that aerial images from different sources line up properly, we have to use a single projection for the entire world.
* We chose to use the **Mercator Projection** which looks like this:
<p align="center">
  <img width="150" align="center" src="https://docs.microsoft.com/en-us/bingmaps/articles/media/150afcdc-99eb-4296-9948-19c0a65727a3.jpg">
</p>


## Ground Resolution and Map Scale
* In addition to the projection, the ground resolution or map scale must be specified in order to render a map. 
  * Lowest Level (Level 1) =  map size 512 x 512 px
  * Level 2 = map size = 1024 x 1024 px and so on.
  * Thus, the map width and height grow by a factor of 2.
* In general, we calculate the width and height of the map in pixels as follows:
  * ```Map width = Map Height = 256 x 2^level pixels```
* The **Ground Resolution** indicates the distance on the ground that’s represented by a single pixel in the map.
* It varies depending on the level of detail and the latitude at which it’s measured.
* Thus, by using Earth Radius = 6378137 m the Ground Resolution in m/px can be calculated as follows:
  *  ```Ground Resolution = cos(latitude * pi/180) * earth circumference / map width```
  * ``` Ground Resolution = (cos(latitude * pi/180) * 2 * pi * 6378137 meters) / (256 * 2^level pixels)```
* The **Map Scale** indicates the ratio between map distance and ground distance, when measured in the same units.
* It varies depending on the level of detail and the latitude at which it’s measured.
* It can be calculated from the ground resolution as follows, given the screen resolution in dots per inch, typically 96 dpi:
  * ``` Map Scale = 1 : ground resolution * screen dpi / 0.0254 meters/inch ```

  * ``` Map Scale = 1 : [cos(latitude * pi/180) * 2 * pi * 6378137 * screen dpi] / (256 * 2^level * 0.0254)```

## Pixel Coordinates
* Now after calculating the above quantities, we now convert the Geographic Coordinates into Pixel Coordinates.
* We consider the following conventions:
  * Pixel at upper left corner = (0, 0)
  * Pixel at lower right corner = (width - 1, height - 1)
* Given latitude and longitude in degrees, and the level of detail, the pixel XY coordinates can be calculated as follows:
  * ``` sinLatitude = sin(latitude * pi/180)```
  * ``` pixelX = ((longitude + 180) / 360) * 256 * 2 level```
  * ```pixelY = (0.5 – log((1 + sinLatitude) / (1 – sinLatitude)) / (4 * pi)) * 256 * 2^level```
  
## Tile Coordinates and Quadkeys
* To optimize the performance of map retrieval and display, the rendered map is cut into tiles of 256 x 256 pixels each.
* Each tile is given XY coordinates from upper left to lower right corner.
* Thus, given a pair of pixel XY coordinate we can easily determine the tile XY coordinates of the tile containing that pixel.
  * ```tileX =  floor( pixelX / 256)```
  * ```tileY =  floor( pixelY / 256)```
* The quad key is a integer value with base 4 that is accepted by the Bing map.
* Parameters for Quadkey:
  * **Tile Position**: A tuple of tile coordinates x and y.
  * **Level**: The level of detail of the map ranging from 1 to 23 that was used to calculate the pixel position.

## How to Run

* This script was written in Python 3.8.
* Install the following packages
  * `numpy`
  * `cv2`
  * `requests`
* We've used Anaconda IDE (Spyder).
* Run the `main.py` by pressing run button or if you are using Command Prompt use `python main.py`
* Enter the required data.
  * ```p1_latitude = 49.945895```
  * ```p1_longitude = 7.846655```
  * ```p2_latitude = 49.952333```
  * ```p2_longitude = 7.820331```
  * ```level = 16```
  * ```filename = "Test.jpg"```
