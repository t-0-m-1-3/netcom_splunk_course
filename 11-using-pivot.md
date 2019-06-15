## Pivots 
----
[![Foo](https://docs.splunk.com/images/thumb/f/f0/6.0_pivot_filter_element_ex.png/700px-6.0_pivot_filter_element_ex.png)](https://docs.splunk.com/images/thumb/f/f0/6.0_pivot_filter_element_ex.png/700px-6.0_pivot_filter_element_ex.png) 

* The Pivot tool lets you report on a specific data set without the Splunk Search Processing Language 
* First, identify a dataset that you want to report on
* Then use a drag-and-drop interface to design and generate pivots that present different aspects of that data 

### Opening Pivots
* Pivots automatically open with a count of events for the selected object. 
* You can use the pencil icon to open the time picker to select a time range: the default option is `All Time`
* The pivot will run immediately when the new range is selected. 

#### How does Pivot work? 
------
* It uses data models to define the broad category of event data that you're working with, and then uses hierarchically arranged collections of data model datasets to further subdivide the original dataset and define the fields that you want Pivot to return 

* Data models and their datasets are designed by the knowledge managers in your organization
* They do a lot of hard work for you to enable you to quickly focus on a specific subset of event data.
    * For example, you can have a data model that tracks email server information, with datasets representing emails sent and emails received. If you want to focus on patterns in your sent email, select the "Email Activity" data model and choose the "Emails Sent" dataset. 

### Creating a pivot:
----
##### There are two ways to navigate to the Pivots view:
1. Through the Datasets page
2. Through the Data Model listing page, via Settings

**Steps**

| From                     | What to do                                                 |
| ---                      | ----                                                       |
| **DataSets pages**       | 1.Search and Reporting App, open Datasets tab              |
|                          | 2. Identify the model you want to pivot                    |
|                          | 3. Actions column>**Explore>Visualize with Pivot**         |
|                          | 4. **Save As**                                             |
| **Settings>Data Models** | 1. **Settings>Data Models**                                |
|                          | 2. Locate Model and in **Actions** column, click **Pivot** |
|                          | 3. Click a dataset and create the pivot                    |
|                          | 4. **Save As**                                             |

### Selecting a Dataset
* From search and reporting app, select the `Dataset` tab
* This will display a list of available lookup table files and data models
* Prebuilt lookups and data models make it easier to interact with your data. 
* Click `Explore> Visulize with Pivot`

#### Quick Aside About datasets
----
A dataset is determined by the type of dataset you choose and the way the dataset has been defined by your data model administrator. 
There are four dataset types:
1. **Event datasets** represent a set of events. Root event datasets are defined by constraints.
2.  **Transaction datasets** represent transactions--groups of events that are related in some way, such as events related to a firewall intrusion incident, or the online reservation of a hotel room by a single customer.
3.  **Search datasets** represent the results of an arbitrary search. Search datasets are typically defined by searches that use transforming or streaming commands to return results in table format, and they contain the results of those searches.
4.  **Child datasets** can be added to any dataset. They represent a subset of the dataset encompassed by their parent dataset. You may want to base a pivot on a child dataset because it represents a specific chunk of data--exactly the chunk you need to work with for a particular report.
    
    
#### Dataset constraints and fields
----
**Constraints** are simple searches that define the dataset that a dataset represents. Are used by root event datasets and all child datasets to define the dataset that they represent. All child datasets inherit constraints from their parent datasets.

* For example, the sample data model "Splunk's Internal Server Logs" includes a child event dataset named "Search Load - Users." It contains events that track the number of concurrent searches being run by users. The inherited constraints for this dataset boil down to the following search:

`index=_internal source=*metrics_log*`

* Search returns metrics log events from the _internal index. The child dataset then has this additional constraint:

`group=search_concurrency user=*`
 
* This further narrows down the set of events represented by the dataset to metrics log events from the _internal index that have a group field value of concurrency and a user field with any value

### Design Pivot Tables with The Editor
----
* When you first enter the Pivot Editor after selecting a dataset, you'll be in the Pivot Editor's pivot table mode by default. 
* The pivot table will initially display one row that presents the dataset's total result count over all time.
* What this initial result count represents depends on the type of dataset you've selected. 

| Type of DataSet                    | Initial Results count represents                                         |
| ---                                | ---                                                                      |
| event dataset ( or child dataset ) | total number of events selected by the dataset constraints over all time |
| transaction dataset                | total number of transactions identified by the dataset oer all time      |
| search dataset                     | total number of table rows returned by the base search over all time     |

##### Go to the "Splunk's Internal Server Logs" data model and click the "Search Load - Users" dataset, you'll see a pivot table that presents the total result count in the "Search Load - Users" dataset over all time.

### Understanding pivot table elements
----
* Editor uses elements to define a pivot table. 
* There are four basic pivot element categories: 
    * Filters 
    * Split rows 
    * Split columns 
    * Column values 

When you first open the Pivot Editor for a specific dataset, only two elements will be defined:
1. A Filter element (set to All time)
2. A Column Values element (set to the Count_of_<dataset_name> field)

This gives you the total count of results returned by the dataset over all time.
You can add multiple elements from each pivot element category to define your pivot table 

#### To add a pivot element
Click the + icon. This opens up the element dialog, where you choose a field and then define how the element it

#### To inspect or edit an element
Click the "pencil" icon on the element. This opens the element dialog. 

#### To reorder pivot elements within a pivot element category
Drag and drop an element within its pivot element category to reorder it

#### To transfer pivot elements between pivot element categories
Drag and drop them.

| Pivot Element    | How does it work?                        | How is it used in data visualization                       |
| **filter**       | used to cut down the result count        | all of the charts and data viz                             |
| **split row**    | splits out the pivot results by row      | column, area, line, scatter, pie, and single value viz     |
| **split column** | breaks out values by column              | column, bar, line, area, scatter, pie and single value viz |
| **column value** | usually a number and aggregates a result | same as above                                              |


### `Split Rows` is under the plus `+` icon, and it will list available attributes to populate the rows with. 
* Once Selected you can:
* modify the label
* Change the sort order 
* Define a maximum # of rows to display
* Click `Add to Table` to view the results. 
* For the Summary tab, you can add in `Totals` and `Percentages`

### `Split Columns`
* Also under the `+` is Split Columns, where you can select the desired split. 
* Specify the maximum number of columns and whether you want Totals

#### Configure a split row or split column element
The configuration options available for split row and split column elements depend on the type of field you choose for them. A few configuration options are specific to either split row or split column element

### Add Additional Filters
* Refine a pivot by filtering on key/value pairs
* `split by` is like rows, with columns being the fields to display. 
* Filters as field=value inclusion, or specific conditions to apply to the search (=, <,>, !=, *)
  
### Select a Visual Format
Display your pivot as a table or a visual
* With this selected, you can control with settings through the panel that appears. 
* You can limit fields, limit to specific series in the data, specify labels, set a sort, and many more options. 

### Drilldown in Pivot Tables

Pivot table drilldown mechanics are similar to those described in Use drilldown for dashboard interactivity in Dashboards and Visualizations. You can review that topic for an overview of drilldown functionality.

#### The Row drilldown mode

The drilldown action selects an entire row of the pivot table. 

Clicking on a specific row launches a search that focuses on the split row element values that belong to the row. 

* Web intelligence data where the rows have been split by URI and then again by HTTP_status. If you click on a row where the URI value is index.php and the HTTP_status is 200, you'll get a search that brings back only those events where URI = index.php AND HTTP_status = 200.

#### The Cell drilldown mode
* Choosing the Cell drilldown mode causes the drilldown action to select a specific cell of the pivot table. 
* Clicking on a specific cell launches a search that takes into account the values of the split row and split column elements that affect the cell. 

### Save a Pivot
* Save you pivots as reports, you can also include the time range picker here too!

### Mouse Actions!!
* Mouse over an object to reveal data about that object
* If drilldown is enabled, you can click the object to expose underlying event information. 

### On switching between Pivot visualization types

Switching between pivot visualizations Pivot will find the pivot elements it needs, discards what it doesn't need, and notify you when elements need to be defined. 

**For example: If you switch your Pivot from table mode to column chart mode but haven't defined a split row element while in table mode, the Y-Axis control panel for the column chart will be yellow and will be marked "Required"***

When you select a visualization type only used for a specific field to populate a panel, that panel will be pre-populated . If you switch to a line or area chart from a column chart, the X-Axis control will be pre-populated with time 

#### Controls common to all charts and single value visualizations

* The Time Range and Filter controls are common to all of the chart types and single value visualizations (including gauges) offered by the Pivot Editor.
Time Range

#### Filter
In the Filter control panel, you can set up multiple filters on different dataset fields

#### Column and bar chart controls

Column charts and bar charts use nearly the same controls

#### General

In the General control panel you can enable or disable chart drilldown functionality. 

### Instant Pivot Overview
----
* Instant Pivot allows you to utilize the pivot tool without a preexisting model. ( it uses the search entered )
* To Create an Instant pivot:
  * Execute a search
  * Click the `Statistics` or `Visualization` tab
  * Select the fields to be included
  * Create the object. 

1. In the Search view, run a non-transforming search. 
    `sourcetype=access_* status=200 action=purchase`

2. Go to the `Statistics` or `Visualization` tab and click `Pivot`.

3. Select the set of fields that you want to use to build your pivot table or chart in the Pivot Builder.
    * Each option displays the number of fields it represents in parenthesis:

      * **All Fields** provides all of the fields that were discovered by the search.

      * **Selected Fields** restricts you to the fields identified as Selected Fields for the search on the Fields tab. If you open a search in Pivot without making changes or selecting fields, the Selected Fields option provides the default selected fields: host, source, and sourcetype. To build your pivot table or chart using a different set of fields, go to the Fields tab and select the fields in the Selected Fields list before you move to the Statistics or Visualization tab and open the search in Pivot. 

      * **Fields with at least** lets you set a coverage threshold for your fieldset. For example, to work with fields that apply to the majority of your events, set the threshold to something high, like 70%. The fieldset you get in Pivot only includes fields that exist in 70% (or more) of the events returned by the search. 

4. Click `Ok` to go to the Pivot Editor.

5. Build your pivot table or chart.
   **Note: If you navigate away from the Pivot Editor without saving your table or chart, your work is lost.**

* While in the Pivot Editor, you can click `Open in Search` to open the pivot search in the Search interface and edit that search. This action takes you out of the Pivot Editor and prevents you from saving any pivot table or chart you created 

#### Save the finished pivot table or chart 
1. In the Pivot Editor, click `Save As` and select either `Report` or `Dashboard Panel`.
    Depending on which one you choose, either the `Save As Report` or `Save As Dashboard Panel` dialog box appears.

2. In the `Save As` dialog box provide information for the report or dashboard panel that you are saving.
    For more information about these fields, see the documentation on saving reports or saving dashboard panels.

3. In the Save as dialog box type the `Model Name `and `Model ID` for the data model that will support the report or dashboard panel.
    You can manage the model that Splunk Enterprise creates through this process if your role has admin-level capabilities. 

4. Click `Save` to save the report or dashboard panel and create the data model. 

#### Save these instant pivots as reports the same way you saved the others; You can also add a pivot to a new or existing dashboard
