## Creating Reports and Dashboards
---
* Reports are saved searches
* They can show events, statistics, or visuals
* Running reports returns a fresh result each time
* Statistics and Visualizations allow you to drill down by default to the underlying event data
#### Smart Naming Conventions
* It's important to go with your organizations naming conventions for things
* If you do not have one, define one
`<group>_<object>_<description>`
`csuit_report_balanceOfaccounts`

### Creating a Report from a Search
----
* Run any search you would like and select the `Save As` button and then select  `Report`
* Give the report a meaningful title
* Specify a description
* Select a time range to include or not. ( adding the picker will allow you to adjust the time when you run the report next )
* You can change the Additional Settings down at the bottom of the wizard once the report is created. 

### Running Reports
* Click the `Reports` tab and the name of the report you would like to run. 
* Use the time range picker to change the time of the report to run it. 

### Editing Reports
* To edit the underlying search on a report select the `Edit>Open In Search` tab.
* Here you can edit the description, permissions, schedule, and accerlations. 
* You can also clone or delete the report. 

### Creating Tables of Visualizations
Three Main Methods of creating tables and visuals:
1. Selcet a field from the fields sidebar and choose a report to run.
2. Use the Pivot interface ( start with a dataset or a pivot )
3. Using SPL to transform the results into what you want.

### Viewing Tables and Visuals
* `Statistics` tab using Splunk's built ins for visuals or tables
* Creating a report from the field window  selecting a field and then choosing the type of report that appears in the window
* For text,you can create a Top Values Report the same way.
* A bar chart is the default visual, with the option to change to many other splunk built in charts or formats. 
  * Stack series, change X-Y Orientations, overlay other charts on top of each other, format a legend, and stack colors
* Statistics tab will change the reults to a table
* Data overlays allow you to create a heatmap and highlight max/min values visually

## What is a Dashboard
* consist of a paneled display of data 
* reports can be used to create panels on a dashboard
* To add a report to a dashboard, click the `Add to Dashboard` button
* Name the dashboard, and provide a description
* Change the permissions
* Enter a title
* Click `Report` for `Panel Powered By` 
* For content, click `Column chart` 
* Single reports can be used across dashboard, therefore, it's efficient to create more dashboard panels.
* changes to the underlying report will be displayed in the dashboard.

## Editing Panels
* After sainvg a panel, a window will appear to update the dashboard
* click `Edit` to customize it. 
* after clicking the dotted bar on the top, you can drag the visual around.     
* in the triple dots menu for More Options, you can Edit the Drill Down
* In this editor you can link to search to access the search directly from the visual. ( You can now click objects in a chart to see the search query)
* The ellipsis menu next to `Edit` allows you to clone a dashboard. 
* Set and View Default dashboards for login