## Using Basic Transformain Commands
* Order results into a data table, that splunk uses for statistical purposes
* Needed to transform results into visuals

##  The `top` command 
* Finds the most common values in a result set; Defaults to Top 10
* Automatically includes the count and percent columns 
* `index=sales sourcetype=vendor_sales | top Vendor`
* Can have limits added in to truncate results, or a zero to return them all
* Other clauses, like `countfield`, `percent field`, `showcount`, `showperc` , `showother`, `showstrg`
* You can remove fields by passing in False i.e.`showperc=False`
* *BY* clauses can be used to show results over a set of fields
 `index=web sourcetype=access_combined action=purchase status=200 | top limit=20 categoryId`
Using the top command with a single field:
`index=security sourcetype=linux_secure (fail* OR invalid) | top limit=5 src_ip`
Using the top command with multiple fields:
`index=network sourcetype=cisco_wsa_squid | top cs_username x_webcat_code_full limit=3`
Using top with a single field and a by clause:
*Top 3 web categories browsed by user*
`index=network sourcetype=cisco_wsa_squid | top x_webcat_code_full  by cs_username limit=3`
*Top 3 users for each caetgory*
`index=network sourcetype=cisco_wsa_squid | top cs_username by x_webcat_code_full limit=3`
*Display the Top 3 Web and User Categories, rename the count field, show count but not percent*
`index=network sourcetype=cisco_wsa_squid | top cs_username x_webcat_code_full limit=3 countfield="Total Viewed" showperc=f`

## The `rare` command
* Shows the least common values of a field set. 
* `index=sales sourcetype=vendor_sales | rare product_name showperc=f limit=1` What is selling the worst, right now
* `index=sales sourcetype=vendor_sales | rare Vendor` will return the least amount of product sold
* *BY* clauses can be used to show results over a set of fields

## The `stats` command
* Helps in calculating statistics about search results
* Most Common Stats Functions are:
    * `count`: returns the number of events matching the results
    * `distinct count`: returns the count of the unique values
    * `sum`: returns the sum of numerical values
    * `average`: returns the average of numverical values
    * `min`: returns the minimum values of the results 
    * `max`: return the maximum value of the results
    * `list`: will list all the values of th field
    * `values`: will list the unique values of the field
    
### The `count` function
* The count functions is made to return the number of matching events. 
* `index=sales sourcetype=vendor_sales | stats count`
* To be a little more explicit: `index=sales sourcetype=vendor_sales | stats count as "Total Sells by Vendors" by product_name`
    * You can add any number of fields to split the count by
* To pass a field into a function, it will only perform the function on events matching that field value: 
  `index=web sourcetype=access_combined | stats count(action) as ActionEvents`
  Or maybe compare some actions against all events:
  `index=web sourcetype=access_combined | stats count(action) as ActionEvents, count as "Total Events"`
*count the invalid or failed login attempts in the last 60 minutes*
`index=security sourcetype=linux_secure (invalid OR failed) | stats count`
`index=security sourcetype=linux_secure (invalid OR failed) | stats count as "Potential Issues"`

### The `dc` function
* Will return the count of unique values in the search results.
* To see how many distinct game titles are for sale for vendors:
    * `index=sales sourcetype=vendor_sales | stats distinct_count(product_name) as "Number of Games for sale by vendors" by sale_price`
* `distinct_count` and `dc` are interchangable
    * `index=sales sourcetype=vendor_sales | stats dc(product_name) as "Number of Games for sale by vendors" by sale_price`
*Unique websites visited by employees in the last 4 hours*
`index=network sourcetype=cisco_wsa_squid | stats dc(s_hostname) as "Websites Visited:"`
*How much bandwidth have users consume at each website visit during the past week*
`index=network sourcetype=cisco_wsa_squid | status(sc_bytes) as Bandwidth by s_hostname`
`index=network sourcetype=cisco_wsa_squid | status(sc_bytes) as Bandwidth by s_hostname | sort -Bandwidth`

### The `sum` function
* returns the sum of numerical values
* `index=sales sourcetype=vendor_sales | stats sum(price) as "Gross Sales" by product_name`
* `index=sales sourcetype=vendor_sales | stats count as "Units Sold" sum(price) as "Gross Sales by product_name"`

### The `avg`/`min`/`max` function
* will only work on numerical data
* `index=sales sourcetype=vendor_sales | stats avg(sale_price)as "AveragePrice"`

### The `list` function
* Will list all the fields for the data
* List out all of the devices for the assets given out across all employees:
 `index=bcgassets sourcetype=asset_list | stats list(Asset) as "company assets" by Employee`
*which websites did each employee visit in the last 60 minutes*
`index=network sourcetype=cisco_wsa_squid | stats list(s_hostname) as "Websites Visited:" by cs_username`

### The `values` function
* We can list out the distinct values of fields by piping the field into the function and crossing it by a field.
* `index=network sourcetype=cisco_wsa_squid | stats values(s_hostname) by cs_username`
*Display the IP address for the names of users who have failed access attempts in the last 60 mintues*
`index=security sourcetype=linux_secure fail* | stats values(user) as "User Names", count(user) as Attempts by src_ip`

##### The Tables that are returned from a `stats` command can be formatted using the brush icon
