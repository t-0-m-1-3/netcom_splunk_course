## Creating Data Models
----
## Describe the relationship between data models and pivot
-----
* Data models drive the pivot tool. 
* Data models enable users of Pivot to create compelling reports and dashboards without designing the searches that generate them. 
* Data models can have other uses, especially for Splunk app developers. 
>A data model is a hierarchically structured search-time mapping of semantic knowledge about one or more datasets

* Familiar with relational database design, think of data models as analogs to database schemas. 
* a data model derived from a heterogeneous system log might have several root datasets (events, searches, and transactions). 
* Each of these root datasets can be the first dataset in a hierarchy of datasets with nested parent and child relationships.

#### Datasets

* Each data model dataset corresponds to a set of data in an index. You can apply data models to different indexes and get different datasets.
* Datasets break down into four types. These types are: Event datasets, search datasets, transaction datasets, and child datasets.
* Datasets are hierarchical. Datasets in data models can be arranged hierarchically in parent/child relationships. The top-level event, search, and transaction datasets in data models are collectively referred to as "root datasets."
* Child datasets have inheritance. Data model datasets are defined by characteristics that mostly break down into constraints and fields. Child datasets inherit constraints and fields from their parent datasets and have additional constraints and fields of their own.
* Child datasets provide a way of filtering events from parent datasets
#### Root and Child Datasets
[![Foo](https://docs.splunk.com/File:6.0-dm-root_objects_example.png)](https://docs.splunk.com/File:6.0-dm-root_objects_example.png)

### Dataset constraints
----
* All data model datasets are defined by sets of constraints. Dataset constraints filter out events 

## Create a data model
| Option                                 | Additional Step                                                                       |
| ---                                    | ---                                                                                   |
| From the Data Models page in Settings. | Find the data model you want to edit and select Edit > Edit Datasets.                 |
| From the Datasets listing page         | Find a data model dataset that you want to edit and select Manage > Edit data model.  |
| From the Pivot Editor                  | Click Edit dataset to edit the data model dataset that the Pivot editor is displaying |
|                                        |                                                                                       |

[![Foo](https://docs.splunk.com/images/4/42/Bubbles_dm_createnew_mod.png)](https://docs.splunk.com/images/4/42/Bubbles_dm_createnew_mod.png)

#### THe Data Model Editor
----
[![Foo](https://docs.splunk.com/images/thumb/e/e6/Data_model_builder.png/720px-Data_model_builder.png)](https://docs.splunk.com/images/thumb/e/e6/Data_model_builder.png/720px-Data_model_builder.png)

### Add a root event dataset to a data model

* Data models are composed chiefly of dataset hierarchies built on root event dataset. 
* Each root event dataset represents a set of data that is defined by a constraint: a simple search that filters out events that aren't relevant to the dataset.
* Click **Add Dataset** in the Model Editor, and select **Root Event**
[![Foo](https://docs.splunk.com/images/thumb/4/46/Add_event_object.png/720px-Add_event_object.png)](https://docs.splunk.com/images/thumb/4/46/Add_event_object.png/720px-Add_event_object.png)

### Add a root search dataset to a data model
* Root search datasets enable you to create dataset hierarchies

[![Foo](https://docs.splunk.com/images/thumb/e/e8/Base_search_dataset.png/720px-Base_search_dataset.png)](https://docs.splunk.com/images/thumb/e/e8/Base_search_dataset.png/720px-Base_search_dataset.png)

### Add a Transaction dataset 
`status < 600 sourcetype=access_* OR source=*.log | transaction...`

[![Foo](https://docs.splunk.com/images/thumb/5/5c/Add_transaction_dataset.png/720px-Add_transaction_dataset.png)](https://docs.splunk.com/images/thumb/5/5c/Add_transaction_dataset.png/720px-Add_transaction_dataset.png)

### Add a child dataset to a data model
* A child dataset inherits all of the constraints and fields that belong to its parent dataset. 
* A single dataset can be associated with multiple child datasets.
* When you define a new child dataset, you give it one or more additional constraints, to further focus the dataset

[![Foo](https://docs.splunk.com/images/thumb/b/b7/Add_child_dataset.png/720px-Add_child_dataset.png)](https://docs.splunk.com/images/thumb/b/b7/Add_child_dataset.png/720px-Add_child_dataset.png)

### Labs: Dataset
----

[![Foo](https://www.tutorialspoint.com/splunk/images/datasets_pivot_1.jpg)](https://www.tutorialspoint.com/splunk/images/datasets_pivot_1.jpg)

#### Select the dataset
[![Foo](https://www.tutorialspoint.com/splunk/images/datasets_pivot_2.jpg)](https://www.tutorialspoint.com/splunk/images/datasets_pivot_2.jpg)

#### Choosing Dataset Fields
[![Foo](https://www.tutorialspoint.com/splunk/images/datasets_pivot_3.jpg)](https://www.tutorialspoint.com/splunk/images/datasets_pivot_3.jpg)


## Use a data model in pivot
[![Foo](https://www.tutorialspoint.com/splunk/images/datasets_pivot_5.jpg)](https://www.tutorialspoint.com/splunk/images/datasets_pivot_5.jpg)

### Choosing the Pivot Fields

[![Foo](https://www.tutorialspoint.com/splunk/images/datasets_pivot_6.jpg)](https://www.tutorialspoint.com/splunk/images/datasets_pivot_6.jpg)

