# Example Map in QGIS Using Data from Go.Data

![image](https://user-images.githubusercontent.com/19505814/122239678-21581d00-ce8f-11eb-8b3e-5a9bfdcd7649.png)

Below we will outline instructions to create a simple choropleth map of cases. We will walk through exporting data manually from Go.Data, reformatting in Excel, and cleaning, aggregating and mapping case data in QGIS, an open source GIS application.

This is not meant to be a complete tutorial on how to make maps or use QGIS - this guide assumes you have some experience with QGIS and/or mapping. The guide uses a polygon GIS dataset (shapefile) with a unique identifier field that matches the unique identifier (Location_ID) of the case data exported from Go.Data. For sources of GIS data to make your own map please see our section [? add the link where the different location data sources are mentioned].

## Step 0. Requirements:
- If you have not used QGIS before, we recommend you spend time learning the basics of QGIS using the training materials available on the QGIS.org website [here](https://qgis.org/en/site/forusers/trainingmaterial/index.html). 
- To install QGIS on your computer, you can find the installation files for QGIS [here](https://qgis.org/en/site/forusers/download.html). 
- This guide was developed using QGIS version 3.4.7 “Madeira”. If you have a different version installed, the graphics and method may be different. You do not need to install this version - just a note that you may have some interface differences.

## Step 1. Extract case data
This step is covered in the [Data Extraction](https://worldhealthorganization.github.io/godata/data-extraction/) document. Please note that you should include the location ID field in the export. This field will be used to join the case data to the shapefile. (I should check and see if it is naturally coming out or if you have to specify it).
