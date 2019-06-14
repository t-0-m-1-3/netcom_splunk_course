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
    
[![Foo](https://dev.splunk.com/web_assets/developers/devguide/SearchBroken.jpg)](https://dev.splunk.com/web_assets/developers/devguide/SearchBroken.jpg) 

##  Examine the search pipeline
Autocomplete and autocoloring will help you type through the commands on the search bar
*booleans and command modifiers* are orange; *Commands* are blue; *Command Arguments* are green; *functions* are purple; 
Parenthesis will highlight their matches, 

`ctrl + \` will cause each pipe to move to a new line
User preference options to break lines, theme, or how much is displayed. 

### Limits 
* *Left to right* flow of search commands are piped in
* Results are read from disk, into memory. 
* The time range and well crafted terms will shrink the returned resulting set. 
* Once data is removed it won't be in the resulting set.

## Review basic search commands and general search practices
* Specify indexes in searches
* Autoformat or use `shift+enter` when typing searches for readability   
     
## Use the following commands to perform searches:
`error sourcetype=access_combined | top uri`
`error sourcetype=access_combined | top uri | search count>1`

#### `fields`
* `fields` command will be used to include / or exclude fields from the returned results
* can make a search faster, but not necessarily; excluding will not
* Piped the results into the `fields` command 
* `index=web sourcetype=access_combined | fields status`
* You can include items from the list with a plus `+` sign` index=web sourcetype=access_combined | fields status`
* You can remove items from the list with a minus `-` sign` index=web sourcetype=access_combined | fields -  status`
* Field extraction is one of the most costly parts of search
* Inclusion happens before exclusion, and will typically improve performance. 

#### Displaying internal fields in Splunk Web

Other than the _raw and _time fields, internal fields do not display in Splunk Web, even if you explicitly specify the fields in the search. For example, the following search does not show the _bkt field in the results.

`index=_internal | head 5 | fields + _bkt | table _bkt`

To display an internal field in the results, the field must be copied or renamed to a field name that does not include the leading underscore character. For example:

`index=_internal | head 5 | fields + _bkt | eval bkt=_bkt | table bkt`
Internal fields and the outputcsv command

When the outputcsv command is used in the search, there are additional internal fields that are automatically added to the CSV file. The most common internal fields that are added are:

   `_raw`
   `_time`
   `_indextime`

To exclude internal fields from the output, specify each field that you want to exclude. For example:

`... | fields - _raw _indextime _sourcetype _serial | outputcsv MyTestCsvFile`


Display network failures during the previous week:
`index=security sourcetype=linux_secure (fail* OR invalid)`       

Display network failures during the previous week, with only user, app, and src_ip
`index=security sourcetype=linux_secure (fail* OR invalid) | fields user, app, src_ip`       

#### `tables` 
* Create a table for fields foo, bar, then all fields that start with 'baz'.
`... | table foo bar baz*`

* Specified fields are kept in results. Retains the data in a tabluated format. 
* Using this query display `index=web sourcetype=access_combined | table clientip, action, productId, status` for the last 4 hours
* `index=web sourcetype=access* status=200 product_name=* | table JSESSIONID, product_name, price`
* `sourcetype=access_* | dedup clientip | eval network=if(cidrmatch("192.0.0.0/16", clientip), "local", "other") | table clientip, network`

#### `rename` 
#### Rename with a phrase

`... | rename SESSIONID AS "The session ID"`

#### Rename multiple, similarly named fields
    * Use wildcards to rename multiple fields.
    * Wrap the name of the field in quotes `""`
    * Reorder coloumns by reordering where you type them

`... | rename *ip AS *IPaddress`
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

#### `dedup`
* Removed duplicate values from a report
* `index=security sourcetype=history* Address_description="San Francisco" | dedup | table Username First_Name Last_Name`
* You can  `dedup` using a single field`index=security sourcetype=history* Address_description="San Francisco" | dedup First_Name | table Username First_Name Last_Name`
* Or multiple fields `index=security sourcetype=history* Address_description="San Francisco" | dedup First_Name Last_Name | table Username First_Name Last_Name`

* Remove duplicates of results with the same 'source' value and sort the events by the '_size' field in descending order.

`... | dedup source sortby -_size`

* For events that have the same 'source' value, keep the first 3 that occur and remove all subsequent events.

`... | dedup 3 source`

* For events that have the same 'source' AND 'host' values, keep the first 3 that occur and remove all subsequent events.

`... | dedup 3 source host`

#### `sort`
* Display results in Ascending or Descending order
* `sourcetype=vendor_sales | table Vendor product_name`
* `sourcetype=vendor_sales | table Vendor product_name | sort Vendor product_name`
* Automatically determine what it is sorting. 
    * If the field takes on numeric values, the collating sequence is numeric. 
    * If the field takes on IP address values, the collating sequence is for IPs. 
    * Otherwise, the collating sequence is in lexicographical order:
      * Alphabetic strings are sorted lexicographically.
      * Punctuation strings are sorted lexicographically.
      * Numeric data is sorted as you would expect for numbers and the sort order is specified as ascending or descending.
      * Alphanumeric strings are sorted based on the data type of the first character. If the string starts with a number, the string is sorted numerically based on that number alone. Otherwise, strings are sorted lexicographically.
      * Strings that are a combination of alphanumeric and punctuation characters are sorted the same way as alphanumeric strings.

* Effects all field names `sourcetype=vendor_sales | table Vendor product_name | sort - sale_price Vendor`
* Effects only field names `sourcetype=vendor_sales | table Vendor product_name | sort -sale_price Vendor`
* Will sort the first 20 results in descending order or sale_price displayed `sourcetype=vendor_sales | table Vendor product_name | sort 20  -sale_price Vendor`

Return the most recent event:

`... | sort 1 -_time`

* Use the sort field options to specify field types
`... | sort num(ip), -str(url)`

* Sort first 100 results in descending order of the "size" field and then by the "source" value in ascending order. 

`... | sort 100 -num(size), +str(source)`

* Sort results by the "_time" field in ascending order and then by the "host" value in descending order.

`... | sort _time, -host`
