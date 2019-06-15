## Filtering and Formatting
----
### The `eval` command
* calculates an expression and puts the resulting value into a search results field.
If the field name that you specify does not match a field in the output, a new field is added to the search results.
 If the field name that you specify matches a field name that already exists in the search results, the results of the eval expression overwrite the values in that field. 

The `eval` command evaluates mathematical, string, and boolean expressions.

You can chain multiple eval expressions in one search using a comma to separate subsequent expressions.

* Concatenate values from two fields

`... | eval full_name = first_name." ".last_name`

* Separate multiple eval operations with a comma

`... | eval full_name = first_name." ".last_name, low_name = lower(full_name)`

* Display timechart of the avg of cpu_seconds by processor, rounded to 2 decimals

`... | timechart eval(round(avg(cpu_seconds),2)) by processor`

* Separate events into categories, count and display minimum and maximum values
`source=all_month.csv | eval Description=case(depth<=70, "Shallow", depth>70 AND depth<=300, "Mid", depth>300, "Deep") | stats count min(mag) max(mag) by Description`

* Find IP addresses and categorize by network using eval functions cidrmatch and if
`sourcetype=access_* | eval network=if(cidrmatch("182.236.164.11/16", clientip), "local", "other")`

###  Using the `search` and `where` commands to filter results
-----
#### `search`
* Retrieve events from indexes or filter the results of a previous search command in the pipeline. You can retrieve events from your indexes, using keywords, quoted phrases, wildcards, and field-value expressions. The `search` command is implied at the beginning of any search. 
* Boolean Comparison Operators
**Old Search**
`(code=10 OR code=29) host!="localhost" xqp>5`
**New Search**
`code IN(10, 29) host!="localhost" xqp>5`

3. Using wildcards
**Old Search**
`host=webserver* (status=4* OR status=5*)`
**New Search**
`host=webserver* status IN(4*, 5*)`

4. Using the IN operator

`sourcetype=access_combined_wcookie action IN (addtocart, purchase)`

#### `where`
* Uses eval-expressions to filter search results. 
* These eval-expressions must be Boolean expressions, where the expression returns either true or false. 
   
* Use the where command to match IP addresses or a subnet

`host="CheckPoint" | where like(src, "10.9.165.%") OR cidrmatch("10.9.165.0/25", dst)`

###  The filnull command
-----
* Replaces null values with a specified value. 

* Example 1: For the current search results, fill all empty fields with NULL.

`... | fillnull value=NULL`
* Example 2: For the current search results, fill all empty field values of "foo" and "bar" with NULL.

`... | fillnull value=NULL foo bar`
* Example 3: For the current search results, fill all empty fields with zero.

`... | fillnull`
* Example 4: Build a time series chart of web events by host and fill all empty fields with NULL.

`sourcetype="web" | timechart count by host | fillnull value=NULL`
