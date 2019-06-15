# Using Transforming Commands for Visualizations:
----
## Explore data structure requirements

* Visualizations require search results in specific formats or data structures. 
* Write queries to generate results in the correct format for the visualization that you are building.

### Data and formatting requirements
----

Depending on the visualization that you are creating, you can use specific search commands to generate results in the correct format.
Visualizations require a search using transforming commands `stats`, `chart`, `timechart`, or `geostats` to render.

Charts visualize one or more data series, or related data points. Depending on the chart type or complexity, the number and ordering of data series can vary.

Single value and gauge visualizations represent a single numerical value.

Maps combine a query and other data components, including data with coordinates or place information, lookup definitions, and geographical markup files.

| Chart name | Optimized for single series? | Optimized for multiple series? | Notes                                                |
| ----       | -----                        | ----                           | ----                                                 |
| Pie        | Yes                          | No                             | Pie charts can only render a single series.          |
| Bar        | Yes                          | Yes                            |                                                      |
| Column     | Yes                          | Yes                            |                                                      |
| Line       | Yes                          | Yes                            | Typically, line charts are used for multiple series. |
| Area       | No                           | Yes                            | Use an area chart to render multiple series.         |
| Scatter    | No                           | Yes                            | Scatter charts work best with two data series.       |
| Bubble     | No                           | Yes                            | Bubble charts work best with three data series.      |

### Using the statistics table
----

When creating a visualization, you can check the Statistics table after running a search to make sure that result fields are generated correctly. 
The number and order of Statistics table columns show you the data structure that a search generated. 

#### Visualization Types:
----
* Events list

* Table visualizations

* Charts
 *    Pie chart
 *    Column and bar charts
 *    Line and area charts
 *    Scatter chart
 *    Bubble chart

* Single value

* Gauges

* Maps
    
## Explore visualization types
-----

## Create and format charts and timecharts
-----
[ ] Create each of these:

[ ] Events list
* An admin uses an events list to give users access to recent notable system events. To generate the events list, the admin runs the following search.

`error OR failed OR severe OR ( sourcetype=access_* ( 404 OR 500 OR 503 ) )`

[ ] Table visualizations
* `index = _internal | stats count by action, host`
* `index = _internal | stats count by action, host | table host count`

[ ] Charts: Charts that aren't defaulted to what you want, change them in the Viz Picker
 *    Pie chart:
     * `... | stats count by Code`
     * `index = _internal | chart avg(bytes) over source`
 *    Column and bar charts:
     * `...| chart avg(bytes) over source `
     * **bar chart**: `index=_internal "group=pipeline" | stats sum(cpu_seconds) as totalCPUSeconds by processor | sort 10 totalCPUSeconds desc`
     * **stacked column chart**: `...| timechart count by Code | fields _time L B N`
 *    Line and area charts:
     * **single series: `...| chart avg(bytes) over source`
     * **multiple series: `...| chart avg(bytes) over source`
     * **line chart**: `index=_internal | timechart count by sourcetype`
     * **Area Chart**: `index=_internal source=*metrics.log group=search_concurrency "system total" NOT user=* | timechart max(active_hist_searches) as "Historical Searches" max(active_realtime_searches) as "Real-time Searches" `
     * **Stacked Area Chart**: `sourcetype=access_* status=200 action=purchase categoryID!=NULL | timechart count(categeoryId) BY categoryID`
 *    **Scatter chart**: `source="earthquake.csv" | table Region Magnitude Depth` and choose scatter
 *    **Bubble chart**: `source="earthquake.csv" | table Region Magnitude Depth` and choose bubble

[ ] Single value:
* `index=_internal source="*splunkd.log" log_level="error" | timechart count`
* `index = _internal source = "*splunkd.log" log_level = "error" | stats count`

[ ] Gauges
* `index=_internal source="*splunkd.log" log_level="error" | stats count as errors`
* Select the Guage type you want:
    * Radial
    * Filler
    * Marker
    
[ ] Maps
* `index=main mag>3 | geostats latfield=latitude longfield=longitude count` and click a cluster to execute a search
* Chloropleth example
