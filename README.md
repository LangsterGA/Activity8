# Example Map in QGIS Using Data from Go.Data

![image](https://user-images.githubusercontent.com/19505814/122239678-21581d00-ce8f-11eb-8b3e-5a9bfdcd7649.png)

Below we will outline how to create a simple choropleth map of cases. We will walk through exporting data manually from Go.Data, reformatting in Excel, and cleaning, aggregating and mapping case data in QGIS, an open source GIS application.

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

## Step 3. Import data into QGIS
In QGIS, add your Go.Data .csv export by clicking on the “Add Delimited Text” button to get the dialog below. Your settings should look like the settings in the graphic below as far as file format and geometry definition. Click add and then close. You will see points in the map view.

![image](https://user-images.githubusercontent.com/19505814/122250195-7566ff80-ce97-11eb-8b96-bca5e1eed015.png)

## Step 4. Create virtual layer
Next, create a Virtual Layer – this will eventually be joined to our polygon shapefile. We do this step-in order to query and aggregate our data prior to joining to the GIS dataset. To learn more about Virtual layers in QGIS, click [here](https://docs.qgis.org/3.16/en/docs/user_manual/managing_data_source/create_layers.html?highlight=virtual#creating-virtual-layers). Next, import the cases that you just loaded into QGIS, into this virtual layer, and then Add to embed it into the layer as shown below. 

![image](https://user-images.githubusercontent.com/19505814/122294900-8c711600-cec6-11eb-88e1-8fd1cc62d113.png)

For this demonstration, we will filter out cases with specific classification types and aggregate by administrative unit area since our GIS dataset is the administrative unit boundaries. The method used here is just one example of performing this workflow – you may know of others and we encourage you to share them to our [Community of Practice](https://community-godata.who.int/login). 

## Step 5. Filter data
Next, add a SQL statement/filter in the Query section of the dialog to get only confirmed and probable case records, and a count of those records by unique “Location_ID”. See syntax in graphic. 

![image](https://user-images.githubusercontent.com/19505814/122295207-ef62ad00-cec6-11eb-94de-8e856275a929.png)

*Note that the unique geographic identifier field is “Location_ID”, the case classification field is “Classification” and “Cases_2021_06_06_clean” is the name of the csv file that was embedded. 

## Step 6. Join to shapefile
In order to make a choropleth map we need to join our virtual layer to an existing polygon layer. Right-click on your polygon GIS layer and go to Properties and select Joins as highlighted in the image below. Use the green + button to add your join. Choose the virtual layer that was just created and the “Location_ID” field as the Join field. For the Target field, choose the matching unique identifier field in the GIS dataset. In this example, it is the “FIPS field”. Then click, OK, Apply and then close the dialog. 

![image](https://user-images.githubusercontent.com/19505814/122295775-a0694780-cec7-11eb-9a18-a8cbbe9b29bc.png)

If you look in the attributes for the shapefile you will see the new column with the virtual layer count. This is the field that will be used to symbolize the choropleth map. These values are the confirmed and probable cases aggregated by Location_ID.

![image](https://user-images.githubusercontent.com/19505814/122296031-eb835a80-cec7-11eb-96fd-989c510fc97e.png)

## Step 7. Create choropleth map
To symbolize the polygon GIS layer, right-click on it in the layers panel to get to Properties and select the “Symbology” item. Follow the graphic below to set up parameters. 

![image](https://user-images.githubusercontent.com/19505814/122296311-41580280-cec8-11eb-8603-20f08b0a454b.png)

Choose “Graduated” for the symbol type. For the column – choose the virtual layer count field that was created in a previous step. Choose the mode – in this example I am using “Natural Breaks”, and then click on the button “Classify” to get the classes to show up in the dialog. You can also adjust the number of classes (right side of dialog). Because this dataset has a low case count, I am only showing 3 classes. When you have completed the parameters click Apply.
