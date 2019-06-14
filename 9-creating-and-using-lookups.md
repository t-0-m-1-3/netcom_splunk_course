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

**Calculate the sales of each product** 
```index=web sourcetype=access* action=purchase
| lookup product_lookup productId OUTPUT price product_name
| stats sum(price) as sales by product_name
```
### Creating an Automatic Lookup
* Under `Settings>Lookups>Automatic Lookups`
* Click `New Automatic Lookup`
* Select the destination app
* Enter the lookup Name
* Select the lookup table definition
* Select host, source, sourcetype to apply the lookup to and specify the name.
* Define the lookup input fields ( these exist in your events that you're relating to your lookup table )
* Define the lookup output fields

The Automatic lookups are available in your search 





     a.       Describe lookups
     
     b.       Create a lookup file and create a lookup definition
     
     c.        Configure an automatic lookup
