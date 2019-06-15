## Using Basic Transformain Commands
* Order results into a data table, that splunk uses for statistical purposes
* Needed to transform results into visuals

[![Foo](https://www.tutorialspoint.com/splunk/images/schedule_alert_5.jpg)](https://www.tutorialspoint.com/splunk/images/schedule_alert_5.jpg)

##  The `top` command 
* Finds the most common values in a result set; Defaults to Top 10
* Automatically includes the count and percent columns 
* `index=sales sourcetype=vendor_sales | top Vendor`
* Can have limits added in to truncate results, or a zero to return them all
* Other clauses, like `countfield`, `percent field`, `showcount`, `showperc` , `showother`, `showstrg`
* You can remove fields by passing in False i.e.`showperc=False`
* *BY* clauses can be used to show results over a set of fields
 `index=web sourcetype=access_combined action=purchase status=200 | top limit=20 categoryId`
 
* Using the top command with a single field:
`index=security sourcetype=linux_secure (fail* OR invalid) | top limit=5 src_ip`

* Using the top command with multiple fields:
`index=network sourcetype=cisco_wsa_squid | top cs_username x_webcat_code_full limit=3`

* Using top with a single field and a by clause:
* *Top 3 web categories browsed by user*
`index=network sourcetype=cisco_wsa_squid | top x_webcat_code_full  by cs_username limit=3`
* *Top 3 users for each caetgory*
`index=network sourcetype=cisco_wsa_squid | top cs_username by x_webcat_code_full limit=3`
* *Display the Top 3 Web and User Categories, rename the count field, show count but not percent*
`index=network sourcetype=cisco_wsa_squid | top cs_username x_webcat_code_full limit=3 countfield="Total Viewed" showperc=f`

* Return the 20 most common values for a field

`sourcetype=access_* | top limit=20 referer`

This screen image shows the results of the search. There are three columns in the results: referer, count, and percent.
Example 2: Return top values for one field organized by another field

This search returns the top "action" values for each "referer_domain".

`sourcetype=access_* | top action by referer_domain`

Because a limit is not specified, this returns all the combinations of values for "action" and "referer_domain" as well as the counts and percentages:


* Returns the top product purchased for each category
    * **After you configure the field lookup, you can run this search using the time range, All time.**
`sourcetype=access_* status=200 action=purchase | top 1 productName by categoryId showperc=f countfield=total`
   *  This search returns the top product purchased for each category. Do not show the percent field. Rename the count field to "total".
   
## The `rare` command
* Shows the least common values of a field set. 
* `index=sales sourcetype=vendor_sales | rare product_name showperc=f limit=1` What is selling the worst, right now
* `index=sales sourcetype=vendor_sales | rare Vendor` will return the least amount of product sold
* *BY* clauses can be used to show results over a set of fields
 
* Return the least common values in the "url" field. Limits the number of values returned to 5.

`... | rare url limit=5`

* Return the least common values organized by host

`... | rare user by host`

## The `stats` command
[![Foo](https://www.tutorialspoint.com/splunk/images/transforming_3.jpg)](https://www.tutorialspoint.com/splunk/images/transforming_3.jpg)

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
* Return the average transfer rate for each host

`sourcetype=access* | stats avg(kbps) BY host`
* Search the access logs, and return the total number of hits from the top 100 values of "referer_domain"

`sourcetype=access_combined | top limit=100 referer_domain | stats sum(count) AS total`

* Calculate the average time for each hour for similar fields using wildcard characters
    * Return the average, for each hour, of any unique field that ends with the string "lay". For example, delay, xdelay, relay, etc.

`... | stats avg(*lay) BY date_hour`

* Remove duplicates in the result set and return the total count for the unique results

`... | stats dc(host)   `

`sourcetype=access_* | chart count BY status, host`

* Compare the difference between using the stats and chart commands

`sourcetype=access_* | stats count BY status, host`

* Use eval expressions to count the different types of requests against each Web server

`sourcetype=access_* | stats count(eval(method="GET")) AS GET, count(eval(method="POST")) AS POST BY host`

This example uses eval expressions to specify the different field values for the stats command to count.
    The first clause uses the count() function to count the Web access events that contain the method field value GET. Then, using the AS keyword, the field that represents these results is renamed GET.
    The second clause does the same for POST events.
    The counts of both types of events are then separated by the web server, using the BY clause with the host field.

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

* The following example returns the count of events where the status field has the value "404"

`...count(eval(status="404")) AS count_status BY sourcetype`


* The following example separates search results into 10 bins and returns the count of raw events for each bin.

`... | bin size bins=10 | stats count(_raw) BY size`


* The following example generates a sparkline chart to count the events that use the _raw field.

`... sparkline(count)`


* The following example generates a sparkline chart to count the events that have the user field.

`... sparkline(count(user))`


* The following example uses the timechart command to count the events where the action field contains the value purchase.

`sourcetype=access_* | timechart count(eval(action="purchase")) BY productName usenull=f useother=f`

* Count the number of different page requests for each Web server
`sourcetype=access_* | chart count(eval(method="GET")) AS GET, count(eval(method="POST")) AS POST BY host`

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

* Removes duplicate results with the same "host" value and return the total count of the remaining results.

`... | stats dc(host)`


* Generates sparklines for the distinct count of devices and renames the field, "numdevices".

`...sparkline(dc(device)) AS numdevices`


* Counts the distinct sources for each sourcetype, and buckets the count for each five minute spans.

`...sparkline(dc(source),5m) BY sourcetype`

* Count the number of different customers who purchased something from the Buttercup Games online store yesterday. The search organizes the count by the type of product 

`sourcetype=access_* action=purchase | stats dc(clientip) BY categoryId`

### The `sum` function
* returns the sum of numerical values
* `index=sales sourcetype=vendor_sales | stats sum(price) as "Gross Sales" by product_name`
* `index=sales sourcetype=vendor_sales | stats count as "Units Sold" sum(price) as "Gross Sales by product_name"`
* `sum(eval(date_hour * date_minute))`


### The `avg`/`min`/`max` function
* will only work on numerical data
* `index=sales sourcetype=vendor_sales | stats avg(sale_price)as "AveragePrice"`
* Returns the average (mean) "size" for each distinct "host".
`... | stats avg(size) BY host`

* Returns the average "thruput" of each "host" for each 5 minute time span.

`... | bin _time span=5m | stats avg(thruput) BY _time host`


* Charts the ratio of the average (mean) "size" to the maximum "delay" for each distinct "host" and "user" pair.

`... | chart eval(avg(size)/max(delay)) AS ratio BY host user`


* Display a timechart of the average of cpu_seconds by processor, rounded to 2 decimal points.

`... | timechart eval(round(avg(cpu_seconds),2)) BY processor`

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
