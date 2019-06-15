## Creating and Using Field Aliases and Calculated Fields
----
* Fields Aliases and Calculated fields can help streamline some of the searching and reporting for your data
* **Field aliases** are an alternate name that you assign to a field. 
* You can use that alternate name to search for events that contain that field. 
* A field can have multiple aliases, but a single alias can only apply to one field. 
* When you run a search, Splunk runs operations to derive knowledge objects and apply them to the events. Splunk applies field aliases after it performs key-value field extraction, but before it processes calculated fields, lookups, event types, and tags.
* Create aliases for fields that are extracted at index time or search time, you cannot create aliases for calculated fields, event types, tags, or fields that are added to your events by a lookup.
* You can reference field aliases in the configurations for search-time operations that follow the field aliasing process. 
    * For example, you can design a lookup table that is based on a field alias. You might do this if one or more fields in the lookup table are identical to fields in your data but have different names. 

* **Calculated fields** are fields added to events at search time that perform calculations with the values of two or more fields already present in those events. Use calculated fields as a shortcut for performing repetitive, long, or complex transformations using the eval command
*  If a calculated field has the same name as a field that has been extracted by normal means, the calculated field will override the extracted field, even if the eval statement evaluates to null. You can cancel this override with the `coalesce` function for eval in conjunction with the eval expression

### Creating Field Aliases
----
*  You can change the behavior of a field alias by selecting Overwrite field values
#### Field Aliases example

One data model might have a field called http_referrer. This field might be misspelled in your source data as http_referer. Use field aliases to capture the misspelled field in your original source data and map it to the expected field name

| When Overwrite fields Values is | And the eents searched for have`src` and `dest`                   | Events only have `dst`                                                                |
| ---                             | ---                                                               | ---                                                                                   |
| is not selected                 | value of the field alias `dst` is unchanged                       | `dst` remains as is                                                                   |
| is selected                     | search head replaces the value of alias `dst` with value of `src` | search head removes `dst` from event because `dst` is an alias of a field not present |
|                                 |                                                                   |                                                                                       |

**Steps**
----
1. Locate a field within your search that you would like to alias.
2. Select Settings > Fields > Field aliases.
3. (Required) Select an app to use the alias.
4. (Required) Enter a name for the alias. Currently supported characters for alias names are a-z, A-Z, 0-9, or _.
5. (Required) Select the host, source, or sourcetype to apply to a default field.
6. (Required) Enter the name for the existing field and the new alias. The existing field should be on the left side, and the new alias should be on the right side.
7. (Optional) Select Overwrite field values if you want your field alias to remove the alias field name when the original field does not exist or has no value, or replace the alias field name with the original field name when the alias field name already exists.
8. Click Save.

### Creating Calculated Fields
----
* Using the Web GUI:
  1.  Select Settings > Fields.
  2.  Select Calculated Fields > New.
  3.  Select the app that will use the calculated field.
  4.  Select host, source, or sourcetype to apply to the calculated field and specify a name.  
        You can also enter a wildcard if you want to apply this for all hosts, sources, or sourcetypes. 
  5.  Name the resultant calculated field.
  6.  Define the eval expression.

### Lab: Creating the Fields:
* We're goingt to search the "web_application" host and create two fields using the `eval` command:
[![Foo](https://www.tutorialspoint.com/splunk/images/calculated_fields_3.jpg)](https://www.tutorialspoint.com/splunk/images/calculated_fields_3.jpg)
* Take those two eval functions and create them using the Web GUI
