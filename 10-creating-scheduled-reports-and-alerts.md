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
* Alerts are based on seraches that either ran:
  * On a regular schedule
  * in real time 
* Alerts can be triggered when results match criteria
* Based on needs, you can use alerts to:
  * Trigger an entry in Triggered Alerts
  * Log an event
  * Output results to a lookup file
  * send emails
  * use a webhook
  * Perform a custom action
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
