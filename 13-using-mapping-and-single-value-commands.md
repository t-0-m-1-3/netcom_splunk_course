## Using Mapping and and Single Value Commands
----
[![Foo](https://dev.splunk.com/web_assets/developers/devguide/Filter.jpg)](https://dev.splunk.com/web_assets/developers/devguide/Filter.jpg) 

##  The `iplocation` command
* Extracts location information from IP addresses by using 3rd-party databases. This command supports IPv4 and IPv6. 
* Events can have empty fields returned, if items from the `allfields` argument are empty. 
  
1. Just add location
 `sourcetype=access_* | iplocation clientip`

2. Search for client errors and return the first 20 results

`sourcetype=access_* status>=400 | head 20 | iplocation clientip | table clientip, status, City, Country`

3. Add a prefix to the fields added by the iplocation command

`sourcetype = access_* | iplocation prefix=iploc_ allfields=true clientip | fields iploc_*`

[![Foo](https://docs.splunk.com/images/thumb/6/69/7.1.0_iplocationPrefix-compressor.png/800px-7.1.0_iplocationPrefix-compressor.png)](https://docs.splunk.com/images/thumb/6/69/7.1.0_iplocationPrefix-compressor.png/800px-7.1.0_iplocationPrefix-compressor.png) 

##  The `geostats` command
* Use the geostats command to generate statistics to display geographic data and summarize the data on maps.
* The command generates statistics which are clustered into geographical bins to be rendered on a world maps.
* Can be grouped using `BY`
* Many limits can be set in the `limits.conf` file. default return limit is **50,000**.

#### Lab:
1. Default settings and get a count:
    1. `... | geostats count`
2. Specify the lat/long fields, calculate the average of a field:
    1. `... | geostats latfield=eventlat longfield=eventlong avg(rating) by gender`
3. Count each product sold by vendor and display the information:
    1. ` sourcetype=vendor_sales | stats count by Code VendorID | lookup prices_lookup Code OUTPUTNEW product_name | table product_name VendorID | lookup vendors_lookup VendorID | geostats latfield=VendorLatitude longfield=VendorLongitude count by product_name`
    2. Click the Visualization tab and plot the results on a map. 
[![Foo](https://docs.splunk.com/images/thumb/6/69/7.1.0_iplocationPrefix-compressor.png/800px-7.1.0_iplocationPrefix-compressor.png)](https://docs.splunk.com/images/thumb/6/69/7.1.0_iplocationPrefix-compressor.png/800px-7.1.0_iplocationPrefix-compressor.png) 
    
##  The `geom` command
* The geom command adds a field, named geom, to each result. 
* This field contains geographic data structures for polygon geometry in JSON
### Usage
#### Specifying a lookup
* To use your own lookup file, you can define the lookup in Splunk Web or edit the `transforms.conf` file. 
Testing lookup files

You can use the inputlookup command to verify that the geometric features on the map are correct. The syntax is | inputlookup <your_lookup>.

For example, to verify that the geometric features in built-in geo_us_states lookup appear correctly on the choropleth map:

* Run the following search:
    * `| inputlookup geo_us_states`
    * On the Visualizations tab, change to a Choropleth Map.
* 
* To show how the output appears with the `allFeatures` argument, the following search creates a simple set of fields and values.
    * `| stats count | eval featureId="California" | eval count=10000 | geom geo_us_states allFeatures=true` 
    * On the Visualizations tab, change to a Choropleth Map.

#### Lab:
1. Built-in geospatial lookup:
    1. `...|geom geo_us_states`
2. Use a field that contains the featureID:
    1. `... | geom geo_us_states featureIdField="state"` 
3.  Built in countries lookup
    1. `... | lookup geo_countries latitude AS lat, longitude as long | stats count BY featureIdField AS country | geom geo_countries featureIdField="country"`
4. Specifiy a bounding box shape:
    1. `... | geom geo_us_states featureIdField="state" gen=0.1 min_x=-130.5 min_y=37.6 max_x=-130.1 max_7=37.7`

##  The `addtotals` command
* computes the arithmetic sum of all numeric fields for each search result. The results appear in the Statistics tab.
* The  `addtotals` command is a **distributable streaming command**(applies a transformation to each event returned by a search.), except when is used to calculate column totals. When used to calculate column totals, the addtotals command is a transforming command.

### Lab:
1. Calculate the sum of the numeric fields of each event:
        * `source="addtotalsData.csv" | chart sum(sales) BY products quarter`
    * Add a column to generate totals for each row:
        * `source="addtotalsData.csv" | chart sum(sales) BY products quarter | addtotals`
    * Stats to calculate totals:
        * `source="addtotalsData.csv" | stats sum(sales) BY products`
2. Specify a name for the field that contains sums:
    1. `... | addtotals fieldname=sum`
3. Wildcards to specifiy the names of the field:
    1. `... | addtotals fieldname=TotalAmount amount* *size*`
4. Sum for a specific field:
    1. `source="addtotalsData.csv" | stats sum(quota) by quarter | addtotals row=f col=t labelfield=quarter sum(quota)`
5. Calculate totals and add custom labels:
    1. `source="addtotalsData.csv" | chart sum(sales) by products quarter| addtotals col=t labelfield=products label="Quarterly Totals" fieldname="Product Totals"`

