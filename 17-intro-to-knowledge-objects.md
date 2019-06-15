### Intro to Knowledge Objects
----
Splunk software knowledge is grouped into five categories:

* **Data interpretation:** Fields and field extractions - Fields and field extractions make up the first order of Splunk knowledge. The fields that are automatically extractd from your data.
* **Data classification**: Event types and transactions - Use event types and transactions to group together interesting sets. **Event types** group together sets of events discovered through searches, while **transactions** are collections of conceptually-related events that span time.
* **Data enrichment**: Lookups and workflow actions - extend your data in various ways. **Field lookups** enable you to add fields to your data. **Workflow actions** enable interactions between fields and other applications or web resources, such as a WHOIS lookup on a field containing an IP address.
* **Data normalization**: Tags and aliases - are used to manage and normalize sets. You can use tags and aliases to group sets of related field values together, and to give extracted fields tags that reflect different aspects of their identity.
* **Data models**: representations of one or more datasets, and drive the Pivot tool. 

### Manage knowledge objects through Settings pages
----
* As your organization uses Splunk software, people add knowledge to the base set of event data indexed within it. You and your colleagues might:

1. Save and schedule searches.
2. Add tags to fields.
3. Define event types and transactions that group together sets of events.
4. Create lookups and workflow actions. 
    
### Monitor and organize knowledge objects

As a knowledge manager, you should periodically check up on the knowledge object collections in your Splunk deployment. You should be on the lookout for knowledge objects that:

    Fail to adhere to naming standards
    Are duplicates/redundant
    Are worthy of being shared with wider audiences
    Should be disabled or deleted due to obsolescence or poor design

Regular inspection of the knowledge objects in your system will help you detect anomalies that could become problems later on.

Note: This topic assumes that as a knowledge manager you have an admin role or a role with an equivalent permission set.
Example - Keeping tags straight

Most healthy Splunk deployments end up with a lot of tags, which are used to perform searches on clusters of field-value pairings. Over time, however, it's easy to end up with tags that have similar names but which produce surprisingly dissimilar results. This can lead to considerable confusion and frustration.

Here's a procedure you can follow for curating tags. It can easily be adapted for other types of knowledge objects handled through Splunk Web.

1. Go to Settings > Tags > List by tag name.
2. Look for tags with similar or duplicate names that belong to the same app  Alternatively, you may encounter tags with identical names except for the use of capital letters, as in crash and Crash. Tags are case-sensitive. Keep in mind that you may find legitimate tag duplications 
3. Try to disable or delete the duplicate or obsolete tags you find, if your permissions enable you to do so. However, be aware that there may be objects dependent on it that will be affected

### The Field Extractor
----
* Splunk will automatically extract fields during indexing
* You can also extract more fields Using the Extractor

    
[![Foo](https://docs.splunk.com/images/5/5d/Em_FX_steps_diagram.png)](https://docs.splunk.com/images/5/5d/Em_FX_steps_diagram.png)

