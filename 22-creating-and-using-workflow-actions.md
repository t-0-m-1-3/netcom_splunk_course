## Creating and Using Workflows
----
## Describe the function of GET, POST, and Search workflow actions
| Workflow action type        | Description                                                                                                                                                                                                                                                 |   |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
| **GET workflow actions**    | GET workflow actions create typical HTML links to do things like perform Google searches on specific values or run domain name queries against external WHOIS databases.                                                                                 |   |
| **POST workflow actions**   | POST workflow actions generate an HTTP POST request to a specified URI. This action type enables you to do things like creating entries in external issue management systems using a set of relevant field values.                                   |   |
| **Search workflow actions** | Search workflow actions launch secondary searches that use specific field values from an event, such as a search that looks for the occurrence of specific combinations of ipaddress and http_status field values in your index over a specific time range. |   |


## Create a GET workflow action

#### Google seach from field values
[![Foo](https://www.tutorialspoint.com/splunk/images/search_macro_2.jpg)](https://www.tutorialspoint.com/splunk/images/search_macro_2.jpg)

     
####  Provide an external IP lookup
1. In the Workflow actions details page, set Action type to link and set Link method to get.

2. Set a Label value of WHOIS: $`domain$`. Set a URI value of `http://whois.net/whois/$domain$.`

After that, you can determine:

* Whether the link shows up in the field menu, the event menu, or both.
* Whether the link opens the WHOIS search in the same window or a new one.
* Restrictions for the events that display the workflow action link. 
* You can target the workflow action to events that have specific fields, that belong to specific event types, or some combination of the two.

## Create a POST workflow action
You set up POST workflow actions in a manner similar to that of GET link actions. However, POST requests are typically defined by a form element in HTML along with some inputs that are converted into POST arguments. 
    
[![Foo](https://docs.splunk.com/images/1/1c/POST_wf_action_example_b.png)](https://docs.splunk.com/images/1/1c/POST_wf_action_example_b.png)

## Create a Search Workflow
* To set up workflow actions that launch dynamically populated secondary searches, you start by setting `Action` type to search on the Workflow actions detail page. 
* This reveals a set of Search configuration fields that you use to define the specifics of the secondary search.

In Search string enter a search string that includes one or more placeholders for field values, bounded by dollar signs. 
>For example, if you're setting up a workflow action that searches on client IP values that turn up in events, you might simply enter`clientip=$clientip$`in that field. 

## Launch a secondary search that finds errors originating from a specific Ruby On Rails controller

[![Foo](https://docs.splunk.com/images/b/b7/5.0-Mgr-Workflow_Actions_SearchEx_b.png)](https://docs.splunk.com/images/b/b7/5.0-Mgr-Workflow_Actions_SearchEx_b.png)

### Use special parameters in workflow actions
* There are special parameters for workflow actions that begin with an "@" sign. 
* Two of these special parameters are for field menus only. They enable you to set up workflow actions that apply to all fields in the events to which they apply.

|             |                                                   |
| ---         | --                                                |
| @field_name | Refers to the name of the field being clicked on. |
| @field_value |Refers to the value of the field being clicked on.                                                    |

The other special parameters are:
|              |                                                                                                                                                           |
| ---          | --                                                                                                                                                        |
| @sid         | Refers to the sid of the job that returned the event                                                                                                      |
| @offset      | Refers to the offset of the event in the job                                                                                                              |
| @namespace   | Refers to the namespace from which the job was dispatched                                                                                                 |
| @latest_time | Refers to the latest time the event occurred. It is used to distinguish similar events from one another. It is not always available for all fields. |

#### Create a workflow action that applies to all fields in an event
Update the Google search example so that it enables a search of the field name and field value for every field in an event to which it applies. 
All you need to do is change the title to Google this field and value and replace the URI of that action with `http://www.google.com/search?q=$@field_name$+$@field_value$`

**Remember**: Workflow actions using the `@field_name`and/or `@field_value` parameters are not compatible with event-level menus

#### Show the source of an event

This workflow action uses the other special parameters to show the source of an event in your raw search data.

The Action type is link and its Link method is get. 
Its Title is Show source. 
The URI is`/app/$@namespace$/show_source?sid=$@sid$&offset=$@offset$&latest_time=$@latest_time$` 


[![Foo](https://docs.splunk.com/images/thumb/3/36/6.0_wkflow_actions_field1.png/700px-6.0_wkflow_actions_field1.png)](https://docs.splunk.com/images/thumb/3/36/6.0_wkflow_actions_field1.png/700px-6.0_wkflow_actions_field1.png)
