### Naming conventions
----
####  Set up a naming convention for reports

| Group | Search type | Platform | Category   | Time interval | Description  |
| --    | --          | --       | --         | ------------  | ------------ |
| SEG   | Alert       | Windows  | Disk       | arbitrary     | arbitrary    |
| NeG   | report      | Iseries  | Wxchange   | arbitrary     | arbitrary    |
| ops   | summary     | network  | sql        | arbitrary     | arbitrary    |
| noc   |             |          | Event Log  |               |              |
|       |             |          | CPU        |               |              |
|       |             |          | Jobs       |               |              |
|       |             |          | SubSystems |               |              |
|       |             |          | services   |               |              |
|       |             |          | security   |               |              |

**Group**: Corresponds to the working group(s) of the user saving the search.

**Search type**: Indicates the type of search (alert, report, summary-index-populating)

**Platform**: Corresponds to the platform subjected to the search.

**Category**: Corresponds to the concern areas for the prevailing platforms.

**Time interval**: The interval over which the search runs (or on which the search runs, if it is a scheduled search)

**Description**: A meaningful description of the context and intent of the search, limited to one or two words if possible. Ensures the search name is unique.


### What are datasets?
----
* A dataset is a collection of data that you define and maintain 
* It is represented as a table, with fields for columns and field values for cells. 
* You can view and manage datasets with the Datasets listing page.

#### Dataset types
----
You can work with three dataset types: 
   *  (1) **Lookups** and (2) **data models**, are existing knowledge objects that have been part of the Splunk platform for a long time. 
   * (3) **Table datasets**, or **tables**, are a new dataset type that you can create and maintain in Splunk Cloud, **and** after you download and **install** the **Splunk Datasets Add-on**

#### Use the Datasets listing page to view and manage your datasets. 

### Lookups
-----
The Datasets listing page displays two categories of lookup datasets: **lookup table files** and **lookup definitions**. 

It lists lookup table files for .csv lookups and lookup definitions for .csv lookups and KV Store lookups. 
Other types of lookups, such as **external lookups** and **geospatial lookups**, are not listed as datasets.

You upload lookup table files and create file-based lookup definitions through the Lookups pages in Settings. 


### Data model datasets
----
Data models are made up of one or more data model datasets. When a data model is composed of multiple datasets, those datasets can be arranged hierarchically, with a root dataset at the top and child datasets beneath it. In data model dataset hierarchies, child datasets inherit fields from their parent dataset but can also have additional fields of their own.

You create and edit data model dataset definitions with the Data Model Editor. 

#### Table datasets
----
Table datasets, or tables, are focused, curated collections of event data that you design for a specific business purpose. You can derive their initial data from a simple search, a combination of indexes and source types, or an existing dataset of any type. For example, you could create a new table dataset whose initial data comes from a specific data model dataset. After this new dataset is created, you can modify it by updating field names, adding fields, and more.

You define and maintain datasets with the Table Editor, which translates sophisticated search commands into simple UI editor interactions. It is easy to use, even if you have minimal knowledge of Splunk search processing language (SPL).

### What is the Common Information Model (CMI)?
-----
The Common Information Model Add-on is based on the idea that you can break down most log files into two components:

 **fields**
 
 **event category tags**

With these two components, a knowledge manager can normalize log files at search time so that they follow a similar schema. 

The Common Information Model details the standard fields and event category tags that Splunk software uses when it processes most IT data.

In the past, the Common Information Model was represented here as a set of tables that you could use to normalize your data by ensuring that they were using the same field names and event tags for equivalent events from different sources or vendors.

Now, the Common Information Model is delivered as an add-on that implements the CIM tables as data models. You can use these data models in two ways:

1. Initially, you can use them to test whether your fields and tags have been normalized correctly.
2. After you have verified that your data is normalized, you can use the models to generate reports and dashboard panels via Pivot.

### Install Common Information Model Lab
-----
1. Sign into splunkbase
2. Download the `zip`
3. Upload the zip through the apps portal
