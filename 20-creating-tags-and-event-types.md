## Creating Tags and Event Types
----

#### **Tags** 
* Enable you to assign specific field and value combinations:
* You can tag hosts with certain terms likes `compliance` if they are dealing with certain types of data    
##### Search for all routers not in Building1 of San Francisco
 * `tag=router tag=SF NOT (tag=Building1)`

#### Event Types
* Categorization system to help you make sense of your data. Event types let you sift through huge amounts of data, find similar patterns, and create alerts and reports. 
    * **Note**: Using event types as a short cut for search is not recommended. 
    * If you want to shorten a portion of a search, it is much better to use a **search macro**
* Important event type definition restrictions:
  * You cannot base an event type on a search that:
  * Includes a pipe operator after a simple search.
  * Includes a subsearch.
  * Is defined by a simple search that uses the`savedsearch`command    
### Create and use tags
* Create the appropriate event types in the Events type manager in Splunk Web by going to Settings > Event types. You can also edit the eventtypes.conf file directly.
* Create the appropriate tags in Splunk Web. Select Settings > Event types, locate the event type that you want to tag and click on its name. You can also edit the tags.conf file directly.
* We're going to search the "web_applcation" host and tag error codes:
     
[![Foo](https://www.tutorialspoint.com/splunk/images/tags_1.jpg)](https://www.tutorialspoint.com/splunk/images/tags_1.jpg)

* Create Tags by clicking the **Edit Tags** option
[![Foo](https://www.tutorialspoint.com/splunk/images/tags_2.jpg)](https://www.tutorialspoint.com/splunk/images/tags_2.jpg)
----
   
[![Foo](https://www.tutorialspoint.com/splunk/images/tags_3.jpg)](https://www.tutorialspoint.com/splunk/images/tags_3.jpg)

* Perform a search using the tag syntax
    `tag::status="server_error"`

[![Foo](https://www.tutorialspoint.com/splunk/images/tags_4.jpg)](https://www.tutorialspoint.com/splunk/images/tags_4.jpg)


### Create an event type

[![Foo](https://www.tutorialspoint.com/splunk/images/event_type_1.jpg)](https://www.tutorialspoint.com/splunk/images/event_type_1.jpg)


[![Foo](https://www.tutorialspoint.com/splunk/images/event_type_2.jpg)](https://www.tutorialspoint.com/splunk/images/event_type_2.jpg)

[![Foo](https://www.tutorialspoint.com/splunk/images/event_type_5.jpg)](https://www.tutorialspoint.com/splunk/images/event_type_5.jpg)

#### You can also use `Settings>Event Types`
----
[![Foo](https://www.tutorialspoint.com/splunk/images/event_type_3.jpg)](https://www.tutorialspoint.com/splunk/images/event_type_3.jpg)
----

[![Foo](https://www.tutorialspoint.com/splunk/images/event_type_4.jpg)](https://www.tutorialspoint.com/splunk/images/event_type_4.jpg)
----

[![Foo](https://www.tutorialspoint.com/splunk/images/event_type_6.jpg)](https://www.tutorialspoint.com/splunk/images/event_type_6.jpg)


Tag event types to organize your data into categories. There can be multiple tags per event. You can tag an event type in Splunk Web or configure it in tags.conf. For more information about event type tagging, see Tag event types.

Let's say that we have saved a search for page not found as the event type status=404 and then saved a search for failed authentication as the event type status=403. If you tagged both of these event types with HTTP client error, all events of either of those event types can be retrieved by using the search:

`tag::eventtype=HTTP client error`




Event type tags are commonly used in the Common Information Model (CIM) add-on for the Splunk platform in order to normalize newly indexed data from an unfamiliar source type. We can use tags to identify different event types within a single data source.

You can apply CIM-compliant tags to your data.  For example, the cpu_load_percent object in the Performance data model has the following tags associated with it:

`tag = performance `

`tag = cpu`

For more information about the Common Information Model and event tagging, see Configure CIM-compliant event tags. 
