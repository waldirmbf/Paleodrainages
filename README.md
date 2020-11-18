# **Paleodrainages reconstruction in ArcGIS**
Step-by-step protocol to generate paleodrainages reconstructions in ArcGIS v. 10.4.1.

-----------------------

##  **1) Downloading bathymetric files**
Download bathymetric data from
 https://www.gebco.net/data_and_products/gridded_bathymetry_data/

 The area of interest has to be selected and dragged down while holding Ctrl. Check GeoTIFF format (30sec is the best resolution), add file to your basket, register an account and wait for the file to be ready to downloaded.

 ##  **2) Working in ArcGis v. 10.4.1**

### 2.1) Processing files

 __2.1.1)__ Open ArcGis and import bathymetric file.

 __2.1.2)__ In data view mode, select the subset of the area of interest creating a rectangle. Convert the area within the rectangle into a shape file, as follows:

 ````
 > Drawing > Convert Graphics to Feature > Select the Geotiff file as the source.

````
__2.1.3) Extract by masking:__ We need to extract the area of interest (delimited by the rectangle) from our original bathymetric file. As follows:*

````
> ArcToolbox > Spatial Analyst Tools > Extraction > Extraction by Mask
````
> (or use Crtl +'F') and look for 'Extraction by mask'. The 'input raster' will be the bathymetric file, while 'feature mask data' will be the retangle feature created on __step 2.1.2.__

__2.1.4) Contour:__ From now on, most of the work is done on the masked feature. On this step, a contour on the masked feature will be created to delimit for how further from the continent is the delimitation for the last glacial maximum. In this case, the average reduction on sea level was 125m. As follows:

````
> (use Crtl +'F') and look for 'Contour'. Select the one for (Spatial Analyst).  
````
>The 'input raster' will be the masked feature, an the 'Contour interval' value is set as '125'.

__2.1.5) ID for the contour line:__ Use information tool (i) to get the ID of the line delimiting the continental shelf at -125m. Once in possession of the line ID, right-click on the contour object, open its attribute table, go to select by attribute and select the object ID as equals the ID of the line identified. This highlight the line as a selected attribute within the contour object.

__2.1.6) Creating a shape file the contour line:__ Now that the contour line is selected, create a shape file containing this line. As follows:
````
> Right-click the 'Contour' object. Data > Export data > Selected features
````
__2.1.7) Create a polygon area from the contour line:__ Now, the hardest part,  transforming the contour line into a polygon. For that, select the contour line and then as follows:
````
 Editor > Start editing > Edit vertices >  Continue Feature tool.
````
>Continue the contour line following the rectangle limits created on
__step 2.2__. Once done, click Editor > Stop editing.

 __2.1.8) Transform contour line into polygon:__ As follows:
 ````
 > (use Crtl +'F') and look for 'Feature to polygon'. Select the one for (Data management).
 ````
>Select the contour line as input raster.

__2.1.9) Extract by masking:__ Now, we defined polygon containining the contour line and the continental area to be analysed. We need to extract by masking the polygon just created within the initial bathymetric file. This follows the same approach as used on __step 2.1.3__.

> (use Crtl +'F') and look for 'Extract by mask'. Input raster wil be the original bathymetric file, while the feature to be masked will be the polygon created in __step 2.1.8.__

### 2.2) Hydrology tools

__2.2.1) Fill:__ This tool is used to flood the area  to identify the steepness and depth of every cell.

> (use Crtl +'F') and look for 'Fill'. Input raster will be the file created in __step 2.1.9.__ P.S.: Not much will happen in the screen during this step, however a new file will be created.

__2.2.2) Flow direction:__ According to the the steepness and depth of every cell, this tool will indicate the likeky flow direction.

> (use Crtl +'F') and look for 'Flow direction'. Input raster wil be the file created in __step 2.2.1.__

 __2.2.3) Flow accumulation:__ This tool is optional, but it can be used to identify the river orders.

> (use Crtl +'F') and look for 'Flow accumulation'. Input raster wil be the file created in __step 2.2.2.__

__2.2.4) Con:__ Another optional tool to filter out only cells present in both flow accumlation and flow direction rasters.

__2.2.5) Stream order:__ Identify the stream order for each linear network according to the flow direction.

> (use Crtl +'F') and look for 'Stream order'. Input raster wil be the file created in __step 2.2.4.__ Also input flow direction raster.

__2.2.6) Stream to feature:__ Converts linear networks into features. This is the step where all the potential streams in the interst area will show up.

> (use Crtl +'F') and look for 'Stream to feature'. Input raster wil be the file created in __step 2.2.5.__ Also input flow direction raster.

 __2.2.7) Filtering streams:__ Given the larger number of foodable areas, the stream to feature tool ends up with streams in all potential orders. To filter those out, double click in the stream feature and then:
 ````
  > Query > Query builder > Select grid_code > Get unique values   
 ````
>  Build the following the expression to filter out the first three river orders:
grid_code <> 1 AND grid_code <> 2  AND grid_code <> 3


__2.2.8) Basins__:  Also optional, but its used to delimiate river basins based on the flow direction raster.

>  Play with the basin and streams symbology and the streams to highlight the rivers or paleodrainages of interest.
