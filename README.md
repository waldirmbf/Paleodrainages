# **Paleodrainages reconstruction in ArcGIS**
Step-by-step protocol to generate paleodrainages reconstructions in ArcGIS v. 10.4.1.

-----------------------

##  **1) Downloading bathymetric files**
Download bathymetric data from
 https://www.gebco.net/data_and_products/gridded_bathymetry_data/

 The area of interest has to be selected and dragged down while holding Ctrl.  Tick GeoTIFF format (30sec is the best resolution),  add file to basket, register an account and wait for the file to be ready to downloaded.

 ##  **2) Working in ArcGis v. 10.4.1**

 *2.1) Open ArcGis and import bathymetric file.*

 *2.2) In data view mode, select the subste of the area of interest creating a rectangle. Convert the area within the rectangle into a shape file, as follows:*

 ````
 > Drawing > Convert Graphics to Feature > Select the Geotiff file as the source.

````

 4) Extract by Mask -> mask the bathymetric file into the rectangle you just created. After this, we will only be working on this reduced area.

 5) Contour -> This is the step on which you select the desired sea level in base contour. -125m is the average of sea level retraction on the last glacial maximum

 6) Use information tool to get the ID of the main line (in our case 90)


 7) Double click on the object, open atributte table and then select by attribute ID=90

 8) Now the line is selected. Create a shapefile of this line. DOuble click on the contour file, data > export data > selected features.

 9) Now, the hardest part. We need to transform the conrount line into a polygon. Use Editor > Start editing > Edit vertices >  Continue feature tool. Make a polygon with the contour.

 10) Feature to polygon to transform the contour lines into a polygon.

 11) Extract by mask the initial bathymateric file with the polygon you have just created.

 12) Use fill. This tool is done to flood the area (you wont see anything) to identify the steepness and fepth of every cell.
 13) Use flow direction on the filled extracted.
 14) Flow accumulationto idenify river orders (optional)
 15)Con (inputs flow accumulation and flow direction) (option) Not sure why is needed
 16) Stream order
 17) Stream to feature
 18) After creating the streams, select their order on the query builder. Right click, go to the Query builder and make the expression grid_core <> 1 AND grid_core <> 2 grid_core <> 3
 19)Basin
 20) Play with the basin symbology and the streams (if you want to identify stream order as well).

 Useful material: ANDREA THOMAz Palaedr_steps_AT.pdf in Dropbox and this video: https://www.youtube.com/watch?v=XHdRDs5NSDo&list=LLB9MUAQwDkSR-Ba4tRxywog&index=102&t=0s (also on dropbox)
