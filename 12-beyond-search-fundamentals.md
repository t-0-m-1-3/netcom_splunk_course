## Search fundamentals review
----
### Ahh Home...
[![Foo](https://www.learnsplunk.com/uploads/2/7/1/9/2719363/7109108_orig.png)](https://www.learnsplunk.com/uploads/2/7/1/9/2719363/7109108_orig.png) 

### The Process Model for search
----
[![Foo](https://dev.splunk.com/web_assets/developers/devguide/Filter.jpg)](https://dev.splunk.com/web_assets/developers/devguide/Filter.jpg) 


### The Breakdown of the langeage elements
[![Foo](https://image.slidesharecdn.com/powerofsplv105-160714211141/95/power-of-splunk-search-processing-language-spl-8-638.jpg?cb=1468530881)](https://image.slidesharecdn.com/powerofsplv105-160714211141/95/power-of-splunk-search-processing-language-spl-8-638.jpg?cb=1468530881) 

#### Well at least they have a great sense of humor about it
----
[![Foo](https://slideplayer.com/slide/1633086/7/images/22/Splunk+Search+Processing+Language.jpg)](https://slideplayer.com/slide/1633086/7/images/22/Splunk+Search+Processing+Language.jpg) 


## Case sensitivity
----
* case() 
* term()


### Using Subsearches to find loosely related events
----
* **Subsearch** is a search within a primary, or outer, search. When a search contains a subsearch, the subsearch is run first.

* Subsearches must be enclosed in square brackets in the primary search.

`sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip] | stats count, dc(productId), values(productId) by clientip`

* The subsearch portion of the search is enclosed in square brackets.

`[search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip]`

The first command in a subsearch must be a generating command, such as `search`, `eventcount`, `inputlookup`, and `tstats`. For a list of generating commands, see Command types in the Search Reference.

One exception is the `foreach` command, which accepts a subsearch that does begin with a generating command.  

### How subsearches work

A subsearch looks for a single piece of information that is then added as a criteria, or argument, to the primary search. You use a subsearch because the single piece of information that you are looking for is dynamic. The single piece of information might change every time you run the subsearch. 

#### Lab
`You want to return all of the events from the host that was the most active in the last hour. The host that was the most active might be different from hour to hour. You need to identify the most active host before you can return the events`
 
Break this search down into two parts.

* The most active host in the last hour. This is the subsearch.
* The events from that host. This is the primary search.


You could run two searches to obtain the list of events. The following search identifies the most active host in the last hour.

`sourcetype=syslog earliest=-1h | top limit=1 host | fields host`

This search returns only one host value. Assume that the result is the host named crashy. To return all of the events from the host crashy, you need to run a second search.

`sourcetype=syslog host=crashy`

**The drawback to running two searches is that you cannot setup reports and dashboard panels to run automatically.** You must run the first search to identify the piece of information that you need, and then run the second search with that piece of information.

* You can combine these two searches into one search that includes a subsearch.

`sourcetype=syslog [search sourcetype=syslog earliest=-1h | top limit=1 host | fields + host]` 
 
#### When to use subsearches
-----
Subsearches are mainly used for two purposes:
* Parameterize one search, using the output of another search. The example described above
* Run a separate search and add the output to the first search using the append command. 

#### Subsearch Caveats
----
* Prevent a search from being too expensive
    * Default time limit for a subsearch is **60 seconds**; if it's still running it is finalized.
    * Default event limit is **1,000** and other events are truncated. 
    * Fields returned from the subsearch must be searchable. `search` operator needs to be included. 
  
### Nested Subsearches
----
* You can use more than one subsearch in a search.
    * The inner most subsearch is run first, followed by the next inner subsearch, working out to the outermost subsearch and then the primary search. 

```
index=* OR index=_* 
[search index=* | stats count by component 
| search [search index=* | stats count by user 
| search [search index=* | stats by ipaddress]]]
```
 
For example, you have the following search.

`index=* OR index=_* | [search index=* | stats count by customerID] | [search index=* | stats by productName]`

The order in which the search is processes is:

1. `search index=* | stats by customerID`
2. `search index=* | stats count by productName`
3. (An implied search command) `index=* OR index=_*` (the results of the subsearches)

### Lab Time: Subsearch examples
----
* Without a subsearch, find what the most frequent shopper purchased: 2 searches
    * `sourcetype=access_* status=200 action=purchase | top limit=1 clientip`
    * `sourcetype=access_* status=200 action=purchase clientip=87.194.216.51 | stats count, dc(productId), values(productId) by clientip`

[![Foo](https://www.loggly.com/wp-content/uploads/2019/03/LG-Blog-Body-SplunkCloudvsLoggly-Size-Q119-1-700x393.png)](https://www.loggly.com/wp-content/uploads/2019/03/LG-Blog-Body-SplunkCloudvsLoggly-Size-Q119-1-700x393.png) 
    
* Using the subsearch:
    * **To find what this shopper has purchased, you run a search on the same data. You provide the result of the most frequent shopper search as one of the criteria for the purchases search.  The most frequent shopper search becomes the subsearch**
    * `sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip`
    * `sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip] | stats count, dc(productId), values(productId) by clientip`
    * **Rename the columns for easier people processesing**: `sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip] | stats count AS "Total Purchased", dc(productId) AS "Total Products", values(productId) AS "Product IDs" by clientip | rename clientip AS "VIP Customer"`  
    
[![Foo](https://www.loggly.com/wp-content/uploads/2019/03/LG-Blog-Body-SplunkCloudvsLoggly-Size-Q119-1-700x393.png)](https://www.loggly.com/wp-content/uploads/2019/03/LG-Blog-Body-SplunkCloudvsLoggly-Size-Q119-1-700x393.png) 
     
##  Using the job inspector to view search performance
------
* Can be found on the `Search and Reporting App` under the `Job` tab
* Can also be accessed with the SearchID**(SID)** and the **Search Head Endpoint** `http://search.head.endpoint:8000/manager/search/job_inspector?sid=3.1415926535897932384626433832795`
Here's an example of the execution costs for a dedup search, run over All time:
`* | dedup punct`

[![Foo](https://docs.splunk.com/images/1/19/SearchInspectorExecCostsDedupEx.png)](https://docs.splunk.com/images/1/19/SearchInspectorExecCostsDedupEx.png) 
