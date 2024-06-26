# Geospatial Data Handling



> [!NOTE]
> This repository holds the completed result of my project but the source code.
> I cannot post the source code publicly here to avoid plagiarism, so I put it in another private repository. Although, I can show it upon request.



1. You need to create (generate) longitude,latitude pairs (ie. spatial coordinates) for 12 locations on the USC campus, in addition to where your home/apartment/dorm room is.
The 12, other than your home, would have to be spread out - use your judgment, try to span the campus area, choose locations inside or outside, nearby - we don't want to cover a 'huge' region, or at the other extreme, have the points be 'too close'.
**The 12 'campus' points do need to be as follows: 3 libraries, 3 restaurants/coffee places, 3 waterworks (eg. fountains), 3 department buidings :) Have fun, walking around and sampling these locations!

2. Now that you have 13 coordinates and their label strings (ie. text descriptions such as "CS Dept", "BME"), you are going to create a KML file (.kml format, which is XML) out of them using a text editor. Specifically, each location will be a 'placemark' in your .kml file (with a label, and coords). Here is more detail. The .kml file with the 13 placemarks is going to be your starter file, for doing visualizations and queries. Here is a .kml skeleton to get you started (just download, rename and edit it to put in your coords and labels). NOTE - keep your labels to be 15 characters or less (including spaces). Here is the same .kml skeleton in .txt format, if you'd like to RMB save it instead and rename the file extension from .txt to .xml. NOTE too that in .kml, you specify (long,lat), instead of the expected (lat,long) [after all, longtitude is what corresponds to 'x', and latitude, to 'y']! Also, place your 12 coords (not your home one) in four KML 'folders' - 'libraries', 'eateries', 'waterworks' and 'department buildings'.
You are going to use Google Earth to visualize the data in your KML file (see #3 below). FYI, as a quick check, you can also visualize it using this page - simply copy and paste your KML data into the textbox on the left, and click 'Show it on the map' to have it be displayed on a map on the right :) Or you can do a quick check at http://kmlviewer.nsspot.net/ instead.

3. Download Google Earth on your laptop, install it, bring it up. Load your .kml file into it - that should show you your sampled locations, on Google Earth's globe :) Take a snapshot (screengrab) of this, for submitting.
![](/step3.png)

4. Install Oracle 11g+Oracle Spatial, or Postgres+PostGIS on your laptop, and browse the docs for the spatial functions and search for basic tutorials (eg. this is a good one).
Windows users: please install Postgres v.13.3 from here, then install PostGIS v.3.1.2 after starting Postgres' Stack Builder UI. Mac users can do this instead; more front-end tools are listed at https://postgresapp.com/documentation/gui-tools.html - you can use any of them, instead of DataGrip that's listed in the previous instructions page.
After you install Postgres+PostGIS, here is how you can create a DB, create a table, insert data, query it.
OR, you don't need to install your own copy of Postgres/PostGIS at all :) DO feel free to do the queries this way!

5. You will use the spatial db software to execute the following two spatial queries that you'll write:
• compute the convex hull for your 13 points [a convex hull for a set of 2D points is the smallest convex polygon that contains the point set]. If you use Oracle, see this page; if you decide to use Postgres, read this and this instead. Use the query's result polygon's coords, to create a polygon in your .kml file (edit the .kml file, add relevant XML to specify the KML polygon's coords). Load this into Google Earth, visually verify that all your points are on/inside the convex hull, then take a screenshot. Note that even your data points happen to have a concave perimeter and/or happen to be self-intersecting, the convex hull, by definition, would be a tight, enclosing boundary (hull) that is a simple convex polygon. The convex hull is a very useful object - eg. see this discussion.. Note: be sure to specify your polygon's coords as '...-118,34 -118,34.1...' for example, and not '...-118, 34 -118, 34.1...' [in other words, do not separate long,lat with a space after the comma, ie it can't be long, lat].
• compute the four nearest neighbors of your home/apt/dormroom location [look up the spatial function of your DB, to do this]. Use the query's results, to create four line segments in your .kml file: line(home,neighbor1), line(home,neighbor2), line(home,neighbor3), line(home,neighbor4). Verify this looks correct, using Google Earth, take a snapshot.
Note - it *is* OK to hardcode points, in the above queries! Or, you can create and use a table to store your 13 points in it, then write queries against the table.
![](/step5-nearest-neighbours.png)
![](/step5-convex-hull.png)

6. Use OpenLayers (a JavaScript API) to visualize your location data. The idea is to store your 13 sampled points, via your web browser, in a browser cache area in your local machine, where the data would persist [even after you close the browser]; then you'd read back the stored values, and visualize them, using the OpenLayers API. To store and load points, you'll use 'HTML5 localStorage', which is a key-value based standard WWW API.

Now you're ready to store, retrieve, and plot your points!
Add your sampled locations to OL.html
![](/step6-markers.png)

7. Using Tommy Trojan as the center, compute (don't/can't use GPS!) a set (sequence) of lat-long (ie. spatial) co-ordinates that lie along a pretty Spirograph™ curve :)
If you are on campus, you can go visit TT to get his (long,lat); otherwise, please long-press (for just under a second) on him in https://www.google.com/maps, that will make his coords pop up in a little box.
Create a new KML file with Spirograph curve points [see below], convert the KML to an ESRI 'shapefile', visualize the shapefile data using ArcGIS Online.
To convert your .kml into a shapefile, use this online converter: https://mygeodata.cloud/converter/kml-to-shp - the result will be a .zip [which is what we call 'shapefile'], which will contain within it, shape data (.shp), a relational table (.dbf), and other optional files (.shx, .prj, .cpg). Here is a page on shapefiles, and this talks about the various components (.shp, .dbf etc) of a shapefile.
Once you have your shapefile, you can upload it to ArcGIS' online map creator to view your Spirograph curve-shaped points. To do so, log on to ArcGIS [after creating a free 'public' account], at https://www.arcgis.com/, then use the 'Map' tab - https://www.arcgis.com/home/webmap/viewer.html?useExisting=1. Do 'Add -> Add Layer from File', and upload your shapefile .zip, you should see your data overlaid on a map. Here is a screenshot of roughly what to expect.
What if your shapefile is too large, and you get a 'Dataset too large' error while using ArcGIS? In that case, you can import your shapefile into https://maps.equatorstudios.com/ instead, and create a screenshot (this site will handle large shapefiles).
For the Spirograph curve point creation, use the following parametric equations (with R=36, r=9, a=30):
x(t) = (R+r)*cos((r/R)*t) - a*cos((1+r/R)*t)
y(t) = (R+r)*sin((r/R)*t) - a*sin((1+r/R)*t)
Using the above equations, loop through t from 0.00 to n*Pi (eg. 2*Pi; note that 'n' might need to be more than 2, for the curve to close on itself; and, t is in radians, not degrees), in steps of 0.01. That will give you the sequence of (x,y) points that make up the Spiro curve, which would/should look like the curve in the screengrab below, when R=36, r=9, a=30:

Note - your figure MUST have 4 loops - so please do not change R, r or a!
In order to center the Spirograph at a given location [TT or other], you need to ADD each (x,y) curve point to the (lat,long) of the centering location - that will give you valid Spiro-based spatial coords for use in your .kml file [note that it means, in actuality, you're adding your location's (long,lat) to the computed curve's (y,x)]. You can use any coding language you want, to generate (and visualize) the curve's coords: JavaScript, C/C++, Java, Python, SQL, MATLAB, Scala, Haskell, Ruby, R.. You can also use Excel, SAS, SPSS, JMP etc., for computing [and plotting, if you want to check the results visually] the Spirograph curve points. This can help you a lot :)
Payoff - when you do this in KML, what you'll see is the Spirograph curve superposed on the land imagery - pretty!
![](step7-spirograph.png)
