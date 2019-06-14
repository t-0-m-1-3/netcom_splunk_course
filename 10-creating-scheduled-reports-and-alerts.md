## Creating Scheduled Reports and Alerts
Are great for time-based, repetitive managerial roll-up reports
Dashboard performance
Sending reports via email

### Creating a Scheduled Report
* Create the search you would like to base this off of
* From the `Save As` menu select `Report`
* Enter the information and select `No` for the `time picker` ( it cannot be used )
* After it is created click schedule
###### Define the schedule to run in whatever period you need.

#### Select the Time Range
* By default the search time range is used.
* You can select from presets, relative, or advanced
* Typically the time range is relative to the schedule.     

#### Scheduled Window
* This setting determines the time frame to run the report
* You can provide a window to run the report, if other reports are in the way.

#### Add Actions
* `Log Events`: creates an indexed, searchable log event
* `Output Results to lookup`: sends the results to a CSV lookup file
* `Output Results to telemtry endpoint`: will send usage metrics back to  Splunk ( if you opt into this )
* `Run a Script`: runs a previously created script
* `Webhook`: sends an HTTP Post request 
* `Send Email`: sends the results in a formatted email.

### Managing Reports: Editing Permissions
* Under the `Edit` icon, there is an option to Edit the Permissions.
* You can select `Run As ` options for users to make sure that users do not see data they were not supposed to. 
* You can also `embed` a report results, to make them acccessible from the web. 

## What Are Alerts
### The alerting workflow
* Alerts combine a saved search, configurations for type and trigger conditions, and alert actions. Here are some details about how the different parts of an alert work together.

* **Search: What do you want to track?**

* **Alert type: How often do you want to check for events?**
  * The alert uses the saved search to check for events. 
  * Adjust the alert type to configure how often the search runs. 
  * Use a scheduled alert to check for events on a regular basis. You can also use a real-time alert to monitor for events continuously.

* **Alert trigger conditions and throttling: How often do you want to trigger an alert?**  
  * An alert does not have to trigger every time it generates search results. 

* **Alert Action: What happens when the alert triggers?**
  * When an alert triggers, it can initialize one or more alert actions. 
  
| Alert Type    | WHen it Searches for events  | Trigger Options                      | Throttle Options                          |
| ----          | ----                         | ----                                 | ----                                      |
| **Scheuled**  | accoring to scheulde or cron | specify conditions to trigger off of | Specify a time period to suppress         |
| **Real Time** | Search Contiuously           | **Per-Results**                      | specify time period or fields to suppress |
| **RealTime**  | Search Continuously          | **Rolling Time Winwdow**             | specifiy a time period to suppress        |
|               |                              |                                      |                                           |
  
## Creating an Alert
* Run a search
* Save As > `Alert`
* Give the alert a title and description
### Setting Permissions
* `Private`: only you can access, edit, and view triggered alerts
* `Shared in app`: all users of the app can view the alerts ( by default everyone has read access, with power users having write access )

### Choosing a Real-Time or Scheduled Alert Type
* The Alert Type determins how splunk will search for events that match the alert
* `Scheduled`: searches run at defined intervals, evaluates the trigger conditions when the search completes
* `Real Time`: search is running consistenly in teh background; Trigger conditions are evaluated based on the window of time set 
  * Alerts can be set to trigger:
    * `Per-Result`
    * `Number of Results`
    * `Number of Hosts`
    * `Number of Sources`
    * `Custom`
* You can Throttle the execution of actions on results for periods of time
* `Add Trigger Actions` to handle a variety of responese to the conditions once they are met. 
* From the frequency menu, choose to run the search every hour, day, week, month, or a cron job schedule. 
