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

## Step 2. Reformat extracted file before import to QGIS 
Prior to mapping, you will want to reformat the case export file from Go.Data. This is necessary since there are columns with characters in the header and dashes are used in the file name. In Excel, or a similar application, change the column named “Addresses Location ID (1)” to “Location_ID”. This column is used for aggregating and joining, so it has to be formatted properly for QGIS. For the csv file that was exported, in the filename you will replace the dashes with underscores, or just remove them. Note, in the example below the filename represents data that was exported from Go.Data on June 6, 2021.

FROM: “Cases - 2021-06-06.csv”
TO: “Cases_2021_06_06.csv”

*Note: the dashes were converted to underscores and the spaces removed.

## Step 3. Filter and aggregate in QGIS
For this demonstration, we will filter out cases with specific classification types and aggregate by administrative unit area since our GIS dataset is the administrative unit boundaries. The methods used here are just one example of performing this workflow – you may know of others and we encourage you to share them to our [Community of Practice](https://community-godata.who.int/login). 

In QGIS, add your Go.Data .csv export by clicking on the “Add Delimited Text” button to get the dialog below. Your settings should look like what is set in the graphic below as far as file format and geometry definition. Click add and then close. You will see points in the map view.

Our case data has multiple case classifications. For this demonstration we want those classified as “confirmed” or “probable” only. We also want to aggregate them by unique identifier, which in this case is the “Location_ID” field. This field value matches our unique identifier in our GIS dataset. 

*Note: If your Go.Data instance was not set up using the unique identifier in your GIS dataset, you can use your admin area name field for the filter, aggregation and join. However, the records must be unique values so be sure there are no discrepancies (ie. different spelling between datasets, same name different location, etc.) prior to these processes, or your results will be incorrect.

