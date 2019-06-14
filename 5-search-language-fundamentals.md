## The Splunk Seach Language:
----
Has 5 Main Components:
1. Search Terms -> What we're searching for
    * words, "phrases in quotes", Booleans, etc. 
2. Commands -> What we want to happen to the results
    * Calculate a number, evaluate and format results, create a chart for reports. 
3. Functions -> How we want to display the data
    * Transform values, take an average, calculate a sum
4. Arguments - > What we pass into the function
    * Calculate min/max/avg values for fields, convert values and metrics, etc.
5. Clauses -> modifiers like AS
    * group using a *BY* clause or give a field another name with *AS*

##  Examine the search pipeline
Autocomplete and autocoloring will help you type through the commands on the search bar
*booleans and command modifiers* are orange; *Commands* are blue; *Command Arguments* are green; *functions* are purple; 
Parenthesis will highlight their matches, 

`ctrl + \` will cause each pipe to move to a new line
User preference options to break lines, theme, or how much is displayed. 

### Limits 
*Left to right* flow of search commands are piped in
Results are read from disk, into memory. 
The time range and well crafted terms will shrink the returned resulting set. 
Once data is removed it won't be in the resulting set.

## Review basic search commands and general search practices
  Specify indexes in searches
  Autoformat or use `shift+enter` when typing searches for readability   
     
## Use the following commands to perform searches:
         
#### `fields`
* `fields` command will be used to include / or exclude fields from the returned results
* can make a search faster, but not necessarily; excluding will not
* Piped the results into the `fields` command 
* `index=web sourcetype=access_combined | fields status`
* You can include items from the list with a plus `+` sign` index=web sourcetype=access_combined | fields status`
* You can remove items from the list with a minus `-` sign` index=web sourcetype=access_combined | fields -  status`
* Field extraction is one of the most costly parts of search
* Inclusion happens before exclusion, and will typically improve performance. 

Display network failures during the previous week:
`index=security sourcetype=linux_secure (fail* OR invalid)`       

Display network failures during the previous week, with only user, app, and src_ip
`index=security sourcetype=linux_secure (fail* OR invalid) | fields user, app, src_ip`       

#### `tables` 
* Specified fields are kept in results. Retains the data in a tabluated format. 
* Using this query display `index=web sourcetype=access_combined | table clientip, action, productId, status` for the last 4 hours
* `index=web sourcetype=access* status=200 product_name=* | table JSESSIONID, product_name, price`

#### `rename` 
* Take the field name returned and use the `AS` clause to represent it under a different name
```index=web sourcetype=access_combined 
 | table clientit, action, productId, status
 | rename productId as ProductID, 
 action as "Customer Action",
 status as "HTTP Status"
 | table "Customer Action", "HTTP Status"
```

* `index=web sourcetype=access* status=200 product_name=* | table JSESSIONID, product_name, price | rename JSESSIONID as "User Session" product_name as "Game"`
* you will need to use the *renamed field names* in the subsequent commands you type; the old field name is no longer present in the results. 
```index=web sourcetype=access_combined 
 | table clientit, action, productId, status
 | rename productId as ProductID, 
 action as "Customer Action",
 status as "HTTP Status"
 | table action, status
```
* Wrap the name of the field in quotes `""`
* Reorder coloumns by reordering where you type them

#### `dedup`
* Removed duplicate values from a report
* `index=security sourcetype=history* Address_description="San Francisco" | dedup | table Username First_Name Last_Name`
* You can  `dedup` using a single field`index=security sourcetype=history* Address_description="San Francisco" | dedup First_Name | table Username First_Name Last_Name`
* Or multiple fields `index=security sourcetype=history* Address_description="San Francisco" | dedup First_Name Last_Name | table Username First_Name Last_Name`
        
#### `sort`
* Display results in Ascending or Descending order
* `sourcetype=vendor_sales | table Vendor product_name`
* `sourcetype=vendor_sales | table Vendor product_name | sort Vendor product_name`
* String data is sorted Alphnumerically
* Intergers are sorted numverically
* Effects all field names `sourcetype=vendor_sales | table Vendor product_name | sort - sale_price Vendor`
* Effects only field names `sourcetype=vendor_sales | table Vendor product_name | sort -sale_price Vendor`
* Will sort the first 20 results in descending order or sale_price displayed `sourcetype=vendor_sales | table Vendor product_name | sort 20  -sale_price Vendor`
