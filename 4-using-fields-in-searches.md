# Understand fields
* Fields are searchable key/value pairs
* Can be searched with their names
* `AND` is implied between search terms `action=add_to_cart status=200`
* `area_code=404`
* `action=purchase status=503`
* `soruce=/var/log/messages* NOT host=mail2`
* `sourcetype=acess_combined`

[![Foo](https://cdn-images-1.medium.com/max/1600/1*HrW_Ar1M9L3NNhjQJUi4dA.png)](https://cdn-images-1.medium.com/max/1600/1*HrW_Ar1M9L3NNhjQJUi4dA.png) 

#### *Field Discovery* happens automatically, Splunk discovers key/value pairs based on sourcetypes and pairs found in the data
* Prios, some fields are already stored: `host`, `source`, `sourcetype`, and `index` are **Meta** fields, while internal fields like `_time` and `_raw` are also stored
* **Discovery** directly finds the fields related *at* search time.
* Some fields might not appear in the results
* **Data-Specific** fields come from characteristics of the data ( i.e. **action=purchase**, **add_to_cart**, **etc...** )
* These can also come from inside the data too ( i.e. erorr codes for HTTP )
     
# Use the fields sidebar
* The sidebar contain:
    * **Selected Fields** configurable fields for each event
        * Selected Fields and the values listed under the events for that sourcetype ( by default )
        * You can choose these to be anything you want or make
        * Are in the sidebar and below each event where a value exists
    * **Interesting Fields** pop up in at least 20% 
        * Can modify a field and make it a selected field:
        * Click the field and click `yes` for the selected toggle
    * **All Fields** 
        * Can also be made a **Selected Field**
# The Field Window
* Select a field and see a detailed report:
    * You can narrow results based on the field itself under the **Reports** section
    * you can click a value to add it to the search

# Use fields in searches
* Efficient ways to highlight searches and refine results
* Field names are **CaSe SeNsItIvE** ; field values are **NOT** `host=whatever` will return `HOST=whatever` won't
* Splunk is IP field aware, it understands subnetting `clients_network="192.168.52.0/24"` or`clients_network="10.10.5.*"` 
* Use wildcards, but remember they are evaluted last; 
* Use reltaional operators too `src_port>1000 OR src_port<4000` or `host!=lab`

## A RegEx Primer:
[![Foo](https://imgs.xkcd.com/comics/tar.png)](https://imgs.xkcd.com/comics/tar.png) 

* Say we have a simple string with some integers; `ip=1.2.3.4`:
    * `ip=(<?P<subnet>1.2.3).4` is not a useful regex, but will illustrate: pulling (1.2.3) into a field
        * `ip=(?P<subnet>\d+\.\d+.\d+)\.d+`:
            * `ip=` in the raw string `ip=`
            * `()` bound the capture buffer * `?P<subnet>` inside of the parenthesis says, make this field from these results
            * `\d` matches any single digit
            * `+` is one or more items before the operator
            * `\.` matches the dots in the ip string
            * `\d+\.\d+` matches the other parts of the ip address
            * `\.\d+` finishes the last part of the IP
            * `[1,2,4-9]` is a character set, that will match anything between 1-2, 4-9
            * There are a bunch of ways to make this type of RegEx
            * RegEx manual is based on PERL scripts
     
# `!` vs `NOT` 
  ----
Both will exclude events from you results BUT:
  * `!` would return everything where the field you called exists but the value does not
  * `NOT` will give you everything, except where it's 200
      * `status!=200` vs `NOT status=200`
  *  They can yield the same results if the field always is present in your data
  * `sattus fields for HTTP events are always in `www` logs`
# Fast vs. Smart vs. Verbose:
  * **Fast** emphasizes spped over completeness
  * **Smart** balances speed and completeness
  * **Verbose** allows acces to underlying events using reporting and statiscal commands along with totals

# Best Practices:
  * Time is the most effiecient filter
  * Specifiy one or more index values at the beginning of your search
  * Include as many search terms as possible to limit your results to the smallest domain
  * Make the terms your search as specific as possible `"failed password ssh"`
  * Search for something is faster than search for **NOT** something
  * Filter as early as possible ( dedup )
  * Use Wildcards at the end of the string; they are evaluated last
      * using `OR` might be faster as well

# Working with Indexes
  * It's better to specify an index in your search `index=web_log,ssh_log`
  * it's also possible to specify multple ones, or to use wildcards
  * Using a wildcard is much faster than using no index
  * **Index** is always a viewable field in the search results

* `sourcetype= | eval req_time=req_time/1000 | stats avg(req_time_seconds)`

### Queries
1. Create a new field that contains the result of a calculation

Create a new field called velocity in each event. Calculate the velocity by dividing the values in the distance field by the values in the time field.
`... | eval velocity=distance/time`

2. Use the if function to analyze field values

Create a field called error in each event. Using the if function, set the value in the error field to OK if the status value is 200. Otherwise set the error field value to Problem.

`... | eval error = if(status == 200, "OK", "Problem")`

3. Convert values to lowercase

Create a new field in each event called lowuser. Using the lower function, populate the field with the lowercase version of the values in the username field.

`... | eval lowuser = lower(username)`
4. Use the value of one field as the name for a new field

In this example, use each value of the field counter to make a new field name. Assign to the new field the value of the Value field. See Field names under the Usage section.

`index=perfmon sourcetype=Perfmon* counter=* Value=* | eval {counter} = Value`
5. Set sum_of_areas to be the sum of the areas of two circles

`... | eval sum_of_areas = pi() * pow(radius_a, 2) + pi() * pow(radius_b, 2)`
6. Set status to some simple http error codes

``... | eval error_msg = case(error == 404, "Not found", error == 500, "Internal Server Error", error == 200, "OK")`
