## Creating and Using a Lookup
* Sometimes static ( or fairly consistent data ) appears in a search, but isn't in the index
* Lookups pull this data from standalone files at search time. 
* They allow you to add more fields to your events:
  * Descriptions for HTTP status codes
  * Prices on products
  * User information, IPs, workstationIDs, and other tags
* After configuring a lookup, you can use the fields in searches
* Lookup fields also appear in the fields sidebar
* Lookup field values are `case sensitive` by default.
* Time based lookups can be created if the lookup values are timestamps

### Creating a Lookup
1. Upload teh required file for the lookup into Splunk
2. Define the lookup type
3. Configure the lookup to run ( automatically? )

### Adding a New Lookup Table File
* Inside of `Settings>Lookups>Lookup Table Files` is a wizard:
  * Click `New Lookup Table File`
  * Select the destination
  * Enter the name of the lookup and save

### The `inputlookup` Command
* Is used to load results from a specified static lookup for inspection and review

### Creating a Lookup Definition
* Inside of `Settings>Lookups>Lookup Definitions`:
  * Click `New Lookup Definition`
  * Select the destination app
  * Name the lookup definition
  * Select the lookup type ( file or external resource )
  * Select the file and save

### Applying Advanced Options
* `Min/Max` the number of matches for each input lookup value
* Default the value to output ( when fewer than the min # appear )
* Case sensitivity can be turned off
* Batch Query Index: improves performance for larger files
* Match Types: supplies the format for non-exact matching
* Filter lookups: filters results before returning

### The `lookup` Command
* If a lookup is not configured to run automatically, the `lookup` command needs to be used inside of searches. 

### Lookup Lab
----
1. Upload the prices.csv lookup

**Calculate the sales of each product** 
```index=web sourcetype=access* action=purchase
| lookup product_lookup productId OUTPUT price product_name
| stats sum(price) as sales by product_name
```
[![Foo](https://www.tutorialspoint.com/splunk/images/lookup_7.jpg)](https://www.tutorialspoint.com/splunk/images/lookup_7.jpg)

### Creating an Automatic Lookup
-----
**Steps**
1. In Splunk Web, select Settings > Lookups.
2.  Under Actions for Automatic Lookups, click Add new.
3.  Select the Destination app.
4.  Give your automatic lookup a unique Name.
6.  Select the Lookup table that you want to use in your fields lookup.
7.  In the Apply to menu, select a host, source, or source type value to apply the lookup and give it a name in the named field.
     *  The first field is the field in the lookup table that you want to match. The second field is a field from your events that matches the lookup table field. 
8.  Under Lookup output fields provide one or more pairs of output fields.
     *  The first field is the corresponding field that you want to output to events. The second field is the name that the output field should have in your events. 

9.  You can select the checkbox for Overwrite field values to overwrite the field values when the lookup runs.  Click Save.
The Automatic lookup view appears, and the lookup that you have defined is listed.
