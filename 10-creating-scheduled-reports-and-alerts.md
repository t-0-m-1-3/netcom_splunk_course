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
----
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

### Common Cron Examples
----
```text
*/5 * * * *       Every 5 minutes.
*/30 * * * *      Every 30 minutes.
0 */12 * * *      Every 12 hours, on the hour.
*/20 * * * 1-5    Every 20 minutes, Monday through Friday.
0 9 1-7 * *       The first 7 days of every month at 9 AM.
```

## Best Practices
----
**Coordinate an alert schedule and search time range**

**Coordinating an alert schedule with the search time range** prevents event data from being evaluated twice by the search. If search time range exceeds the search schedule, event data sets can overlap.
When a search time range is shorter than the time range for the scheduled alert, an event might never be evaluated.

**Schedule alerts with at least one minute of delay**
This practice is important in distributed search deployments where event data might not reach the indexer immediately. A delay ensures that you are counting all events, not just the events that were indexed first.

**Best practices example**

This example shows how to configure an alert that builds 30 minutes of delay into the alert schedule. Both the search time range and the alert schedule span one hour, so there are no event data overlaps or gaps.

1. From the Search Page, create a search and select Save As > Alert.
2. In the Save As Alert dialog, specify the following options as shown.

```  
Title: Alert Example (30 Minute Delay)
Alert Type: Scheduled
Time Range: Run on Cron Schedule
Earliest: -90m
Latest: -30m
Cron Expression: 30 * * * *
```
3. Continue defining actions for the alert.

### Differences between scheduled reports and alerts
----
* A scheduled report is like a scheduled or real-time alert in certain ways. 
    * You can schedule a report and set up an action to run each time the scheduled report runs.
* Scheduled reports are different from alerts, however, because a scheduled report's action runs every time the report is run. 
    * The report action does not depend on trigger conditions.
* As an example, you can monitor guest check-ins at a hotel using an hourly search. Here are the differences between a scheduled report and a scheduled alert with email notification actions.

#### Scheduled report: runs its action and sends an email every time the report completes, even if there are no search results showing check-ins. In this case, you get an email notification every hour.

#### Scheduled alert: only runs alert action when it is triggered by search results showing one or more check-in events. In this case, you only get an email notification if results trigger the alert action.

## Lab for Scheuled Alerts
-----
### Scheduled Alert 
----
**Alert example summary**  

**Use case**
    `Track errors on a Splunk instance. Send an email notification if there are more than five errors in a twenty-four hour period.`

**Alert type**
    `Scheduled`

**Search**
    `Look for error events in the last twenty-four hours.`

**Schedule**
    `Run the search every day at the same time. In this case, the search runs at 10:00 A.M.`

**Trigger conditions**
    `Trigger the alert action if the search has more than five results.`

**Alert action**
    `Send an email notification with search result details.`
    
#### Set up the alert

* From the Search Page, create the following search.

    `index=_internal " error " NOT debug source=*splunkd.log* earliest=-24h latest=now`

* Select Save As > Alert.
* Specify the following values for the fields in the Save As Alert dialog box.
```  
Title: Errors in the last 24 hours
Alert type: Scheduled
Time Range: Run every day
Schedule: At 10:00
Trigger condition: Number of Results
Trigger when number of results: is greater than 5.
```
*  Select the Send Email alert action.
*  Set the following email settings, using tokens in the Subject and Message fields.
```  
To: email recipient
Priority: Normal
Subject: Too many errors alert: $name$
Message: There were $job.resultCount$ errors reported on $trigger_date$.
Include: Link to Alert and Link to Results
```

* Accept defaults for all other options.
* Click Save.

#### Real-time alert example

A real-time alert searches continuously for results in real time. You can configure real-time alerts to trigger every time there is a result or if results match the trigger conditions within a particular time window.

**Use case**
    `Monitor for errors as they occur on a Splunk instance. Send an email notification if more than five errors occur within one minute.``

**Alert type**
    `Real-time`

**Search**
    `Look continuously for errors on the instance.`

**Trigger conditions**
    `Trigger the alert if there are more than five search results in one minute.`

**Alert action**
    `Send an email notification.`

#### Set up the alert

* From the Search Page, create the following search.

    `index=_internal " error " NOT debug source=*splunkd.log*`

* Select Save As > Alert.
* Specify the following values for the alert fields.
```   
Title: Errors reported (Real-time)
Alert type: Real-time
Trigger condition: Number of Results
Trigger if number of results: is greater than 5 in 1 minute.
```
* Select the Send email alert action.
* Specify the following email settings, using tokens in the Subject and Message fields.
```
To: email recipient
Priority: Normal
Subject: Real-time Alert: $name$
Message: There were $job.resultCount$ errors.
Include: Link to Alert, Link to Results, Trigger Condition, and Trigger Time.
```        
* Accept defaults for all other options.  Click Save.

#### Throttle the real-time alert
----

Throttle an alert to reduce its triggering frequency and limit alert action behavior. 

Throttle the example real-time alert. The following settings change the alert triggering behavior so that email notifications only occur once every ten minutes.

1.  From the Alerts page in the Search and Reporting app, select the alert. The alert details page opens.
2.  Next to the alert Trigger conditions, select Edit.
3.  Select the Throttle option. Specify a 10 minute period.
4.  Click Save.

#### Custom trigger condition example
----
When you create an alert you can use one of the available result or field count trigger condition options. You can also specify a custom trigger condition. The custom condition works as a secondary search on the initial results set.

#### Alert example summary

**Use case**
    `Use the Triggered Alerts list to record WARNING error instances.`

**Alert type**
    `Real-time`

**Search**
    `Look for all errors in real-time.`

**Triggering condition**
    `Check the alert search results for errors of type WARNING. Trigger the alert action if results include any WARNING errors.`

**Alert action**
    `List the alert in the Triggered Alerts page`.

#### Set up the alert
* From the Search and Reporting home page, create the following search.

    `index=_internal source="*splunkd.log" ( log_level=ERROR OR log_level=WARN* OR log_level=FATAL OR log_level=CRITICAL)`

* Select Save As > Alert.
* Specify the following alert field values.
```
Title: Warning Errors
Alert type: Real-time
Trigger condition: Custom
Custom Condition: search log_level=WARN* in 1 minute
```

* Select the List in Triggered Alerts alert action.  Click Save.

