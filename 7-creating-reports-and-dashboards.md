## Creating Reports and Dashboards
---
* When you create a search or a pivot you want to run again or share with others, you can save it as a report. 
* This means that you can create reports from both the Search and the Pivot sides of the Splunk platform.
* Reports are saved searches
* They can show events, statistics, or visuals
* Running reports returns a fresh result each time
* Statistics and Visualizations allow you to drill down by default to the underlying event data

### Once you create a report you can:
---
* View the results that the report returns on the report viewing page. You can get to the viewing page for a report by clicking the name of the report on the Reports listing page.
* Open the report and edit it so that it returns different data or displays its data in a different manner. 
* Your report opens in either Pivot or Search, depending on how it was created.

### In addition, if your permissions enable you to do so, you can:
----
* Change the report permissions to share it with other Splunk users. See Set report permissions, in this manual.
* Schedule the report so that it runs on a regular interval. Scheduled reports can perform actions each time they run 
* Accelerate slow-completing reports built in Search. 
* Embed scheduled reports in external websites. 
* Add the report to a dashboard as a dashboard panel. 

#### Smart Naming Conventions
* It's important to go with your organizations naming conventions for things
* If you do not have one, define one
`<group>_<object>_<description>`
`csuit_report_balanceOfaccounts`

### Creating a Report from a Search
----
You can create reports via Splunk Web four ways:
    * From Search, by saving a search as a report.
    * From Pivot, by saving a pivot as a report.
    * By selecting Settings > Searches, reports, and alerts and clicking New Report to add a new report.
    * From a dashboard, by converting an inline-search-powered dashboard panel to a report.

* Run any search you would like and select the `Save As` button and then select  `Report`
* Give the report a meaningful title
* Specify a description
* Select a time range to include or not. ( adding the picker will allow you to adjust the time when you run the report next )
* You can change the Additional Settings down at the bottom of the wizard once the report is created. 

[![Foo]( https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_1.png)]( https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_1.png)


[![Foo]( https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_1.png)]( https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_3.png)

#### After clicking View
----
[![Foo]( https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_1.png)]( https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_4.png)


### CLicking Open in Search
----

[![Foo](https://www.tutorialspoint.com/splunk/images/reports_5.jpg)](https://www.tutorialspoint.com/splunk/images/reports_5.jpg)

### Preview your saved search
----
* You can preview a search before running it by using search expansion. 
* Search expansion allows you to preview your search by expanding the entire search, including saved searches, without running the search.

**Steps**
1. Navigate to the Splunk Search page.
2. In the Search bar, type the default report`Errors in the last 24 hours`
3. Open search expansion by using the keyboard shortcut `Command-Shift-E` (Mac OSX) or `Control-Shift-E` (Linux or Windows).
4. The search expansion preview shows syntax highlighting and line numbers, if those features are turned on. 


### Running Reports
* Click the `Reports` tab and the name of the report you would like to run. 
* Use the time range picker to change the time of the report to run it. 

### Editing Reports
If you want to edit a report's definition, are two ways, whether you're on the Reports listing page or looking at the report itself.

**If you're on the Reports listing page** locate the report you want to edit, go to the Actions column, and click Open in Search or Open in Pivot 

**If you've entered the report to review its results** click Edit and select Open in Search or Open in Pivot 

### Edit the definition of a report opened in Search
----
* After you open a report in search, you can change the search string, time range, or report formatting. 
* After you rerun the report, a Save button will be enabled towards the upper right of the report. Click this to save the report. 
* You also have the option of saving your edited search as a new report.

### Edit the definition of a report opened in Pivot
----
* Edit the definition of the pivot: 
    * add, remove, or redefine filters, split rows, split columns, or column values. 
    * You can also change the way the pivot results are formatted (change the visualization type, or fix the way a chart displays). 
    * When you are done, click Save at the upper right of the page to save your report. You also have the option of saving your edited pivot as a new report.

#### To edit a report's description, permissions, schedule, and acceleration settings
----
* From the Reports listing page, or from the report viewing page. Click Edit and choose:
1. Edit Description to change the name and description of the report.
2. Edit Permissions to change the report permissions. 
3. Edit Schedule to schedule the report or change the report schedule if it already has one. 
4. Edit Acceleration to change the way the report is accelerated. Note: This option is only available for certain kinds of reports created in Search. 

**Note: You can't perform these actions if you've opened the report in Search or Pivot. Save the report or return to the Reports listing page if you want to edit these aspects of the report.**

#### Use the Advanced Edit page to update a report configuration
 ----
 * If your role enables it, you can edit the full configuration of a report with the Advanced Edit page. The Advanced Edit page exposes a large variety of configuration settings for reports.
**Steps**
1. Select Settings > Searches, Reports, and Alerts.
2. Find a report that you want to edit and select Edit > Advanced Edit.
3. Update the report configuration as necessary.
4. Save your changes

### Creating Tables of Visualizations
Three Main Methods of creating tables and visuals:
1. Selcet a field from the fields sidebar and choose a report to run.
2. Use the Pivot interface ( start with a dataset or a pivot )
3. Using SPL to transform the results into what you want.


### Viewing Tables and Visuals
----
**Data and formatting requirements**

* Use specific search commands to generate results in the correct format; search using transforming commands, such as `stats`, `chart,` `timechart`, or `geostats` to render.

* Depending on the chart type or complexity, the number and ordering of data series can vary.

* Single value and gauge visualizations represent a single numerical value.

* Maps combine a query and other data components, data with coordinates or place information, lookup definitions, and geographical markup files.
Basic table making
`index = _internal | chart avg(bytes) over sourcetype`
or
`index = _internal | stats count by action, host`
Change the way the columns appear with table
`index = _internal | stats count by action, host | table host count`

### Using the statistics table
* `Statistics` tab using Splunk's built ins for visuals or tables
* The number and order of Statistics table columns show you the data structure that a search generated. 
* * Creating a report from the field window  selecting a field and then choosing the type of report that appears in the window
* For text,you can create a Top Values Report the same way.
* A bar chart is the default visual, with the option to change to many other splunk built in charts or formats. 
  * Stack series, change X-Y Orientations, overlay other charts on top of each other, format a legend, and stack colors
* Statistics tab will change the reults to a table
* Data overlays allow you to create a heatmap and highlight max/min values visually

## What is a Dashboard
----
### The dashboard and form workflow
    
* consist of a paneled display of data 
* reports can be used to create panels on a dashboard
* Build dashboards
  * Create a new dashboard
  * Add new visualizations to a dashboard

* Edit dashboards
  * Add a panel to a dashboard
  * Edit dashboard panels and panel visualizations
  * Manage dashboard searches

* Convert a dashboard to a form
  * Add user inputs to a dashboard to convert it to a form
  * Edit forms
  * Work with user input settings

* Customize Simple XML
  * Edit Simple XML source code to customize a dashboard or form.

* Add interactive and dynamic behavior
  * Use tokens to capture and transfer data.
  * Add event handlers to implement dynamic behavior.

[![Foo](https://www.tutorialspoint.com/splunk/images/dashboard_1.jpg)](https://www.tutorialspoint.com/splunk/images/dashboard_1.jpg)

* To add a report to a dashboard, click the `Add to Dashboard` button
* Name the dashboard, and provide a description
* Change the permissions
* Enter a title
* Click `Report` for `Panel Powered By` 
* For content, click `Column chart` 
* Single reports can be used across dashboard, therefore, it's efficient to create more dashboard panels.
* changes to the underlying report will be displayed in the dashboard.

### Saving the Dashboard
[![Foo](https://www.tutorialspoint.com/splunk/images/dashboard_2.jpg)](https://www.tutorialspoint.com/splunk/images/dashboard_2.jpg)

### Viewing the Complete Dashboard

[![Foo](https://www.tutorialspoint.com/splunk/images/dashboard_3.jpg)](https://www.tutorialspoint.com/splunk/images/dashboard_3.jpg)

#### Dashboard editor user interface

[![Foo](https://docs.splunk.com/File:7.1_AboutEditor.png)](https://docs.splunk.com/File:7.1_AboutEditor.png)

#### Simple XML
----
* Dashboards use Simple XML source code to define their content and behavior. 
* You can use the dashboard editor in Splunk Web to edit this source code.
* The editor provides validation, error messaging, and warnings as you make changes.
* Keyboard shortcuts consistent with Ace code editor shortcuts are available in the dashboard editor.

* You can format Simple XML source code using `Command + Shift + F` on a Mac or `CTRL + Shift + F` on Windows. 

[![Foo](https://docs.splunk.com/images/thumb/5/59/7.1_SourceEditor.png/700px-7.1_SourceEditor.png)](https://docs.splunk.com/images/thumb/5/59/7.1_SourceEditor.png/700px-7.1_SourceEditor.png)

#### Developer options
----
* Splunk Enterprise users can implement additional dashboard customizations:
  * Extend Simple XML using CSS and JavaScript.
  * Convert a Simple XML dashboard to HTML and use JavaScript to implement customizations including inputs and REST API access.
  
### Create a dashboard
----
* Dashboards are created in the context of a particular app. 
    * For example, if you are using the Search and Reporting app, dashboards use this app context.
* After you create a dashboard, you can modify its permissions to share or manage access for other users. You can also modify the app context.
**Steps**
1. Use one of the following options.

| From                   | What to do                                                                             |
| ----                   | ----                                                                                   |
| Dashboards page        | Click Create new dashboard                                                             |
| Saving a visualization | Select Save as > Dashboard panel;Click New to create a new dashboard using this panel. |
        
2. Provide a Title, ID, and Description for the dashboard.
3. Specify permissions.
4. Save the dashboard. 
Use one of the following options.

| From                   | Click                  |
| ----                   | ----                   |
| Dashboards page        | Click Create Dashboard |
| Saving a visualization | Click Save             |

5. Add panels, convert the dashboard to a form, or edit dashboard content.

## Editing Panels
* After sainvg a panel, a window will appear to update the dashboard
* click `Edit` to customize it. 
* after clicking the dotted bar on the top, you can drag the visual around.     
* in the triple dots menu for More Options, you can Edit the Drill Down
* In this editor you can link to search to access the search directly from the visual. ( You can now click objects in a chart to see the search query)
* The ellipsis menu next to `Edit` allows you to clone a dashboard. 
* Set and View Default dashboards for login

## Edit a panel search
Update the search driving a particular dashboard panel.

When you are working with inline searches, the dashboard search bar has syntax highlighting and auto-complete features that can help you build a search string. To learn more, see Help reading searches in the Search Manual.

Search editing options

| All search types  | Reports                                                          | Inline searches and inline pivot |
|-------------------|------------------------------------------------------------------|----------------------------------|
| Edit the title    | View and edit the report in a new window                         |Edit the search, specifying a new inline search or pivot|
| Delete the search |Open the report search in a new window                           |Convert the inline search or pivot to a report|
|                   |Clone to an inline search or pivot                               |Specify an automatic refresh interval delay and indicator option| 
|                   |Select a different report for the panel                          |                                  | 
|                   |Select the visualization specified in the report for this panel  |                                  | 
|                   |Specify an automatic refresh interval delay and indicator option |                                  | 

**Steps**
1.  From the Dashboards listing page, open the dashboard that you want to edit.
2.  Click Edit to open the dashboard editor.
3.  At the top right of each panel, editing icons appear. The first editing icon represents the search for the panel. The search icon varies to represent the type of search being used.
4.  Select the search icon to view configuration options for the search.
5.  Select the search configuration that you want to change. Depending on the option you select, additional configuration dialogs or windows might open.
6.  After editing the search, click Save to save changes to the dashboard.

### Edit a panel visualization
----
* Use the dashboard editor to edit a panel visualization for panels that are not generated with pivot or pivot report searches.

* If you are working with visualizations generated from pivot or pivot report searches, you can use the Pivot Editor. See Design pivot charts and visualizations with the Pivot Editor for details.

**Steps**
----
1.  From the Dashboards listing page, open the dashboard that you want to edit.
2.  Click Edit to open the editing dashboard.
3.  At the top right of each panel, editing icons appear. The second icon represents the Visualization Picker. The icon varies to represent the visualization type. The third editing icon represents the visualization Format menu.
4.  (Optional) Use the Visualization Picker to select a different visualization. Make sure that the panel search generates results in the correct format for the new visualization. You can select any visualization, but the panel search results might not render if they are not formatted for the selected visualization.
5.  (Optional) Use the Format menu to configure the visualization.
6.  Click Save to save changes to the dashboard.


### Edit dashboard source code
----
Edit dashboard Simple XML source code to customize settings that are not accessible from the user interface. The dashboard source code editor provides interactive validation as you make updates.


**Steps**
1.  From the Dashboards listing page, open the dashboard that you want to edit.
2.  Select Edit to open the dashboard editor.
3.  Click Source to open the dashboard XML source code editor.
4.  Edit the source code.
5.  (Optional) Observe that the editor provides automatic tag closing and validation. The editor displays validation warning or error messages as needed. Hover over a warning or error icon next to a line of source code to view the message for that line.
6.  (Optional) Validation warnings and errors disable the Save button. If the button is disabled, correct any code with validation warnings or errors.
7.  If there are no warnings or errors, the Save button is enabled. Click Save to save the source code edits.


#### Edit a prebuilt panel
----
**Steps**
1. From the home page, navigate to Settings > User Interface > Prebuilt Panels.
2. Locate the panel that you want to edit and select Edit.
3. Edit the Simple XML source code.
4. Click Save. The panel is updated in dashboards that include it by reference.
