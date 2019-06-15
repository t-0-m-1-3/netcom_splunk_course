## Creating and Using Search Macros
----
## Describe macros
[![Foo](https://www.tutorialspoint.com/splunk/images/search_macro_1.jpg)](https://www.tutorialspoint.com/splunk/images/search_macro_1.jpg)
* Search Macros are reusable chunks of Search Processing Language, that you can insert into other searches.
* They can be any part of a search, such as an `eval` statement or search term. 
* `sourcetype=access_* | `mymacro` `
* A macro inside of quotes won't be expanded `"foo `bar` baz"` 
* You can preveiw a search macro in the search bar:
    * `command-shift-E` or `control-shit-E`
* When you use a search macro in a search string, if  it begins with `from`, `search`, `,metadata`, `inputlookup`, `pivot`, `tstats`; you will need a `|` command. 
* If you use arguments with macros:
    * `argomarcro(2)` might have to be represented as ``` `argmacro(120,300)` ```
     
**Steps**
* Select Settings > Advanced Search > Search macros.
* Click New to create a search macro.
* (Optional) Check the Destination app and verify that it is set to the app that you want to restrict your search macro to. Select a different app from the Destination app list if you want to restrict your search macro to a different app.
* Enter a unique Name for the search macro.
* If your search macro includes an argument, append the number of arguments to the name. For example, if your search macro mymacro includes two arguments, name it mymacro(2).
* In Definition, enter the search string that the macro expands to when you reference it in another search.
* (Optional) Click Use eval-based definition? to indicate that the Definition value is an eval expression that returns a string that the search macro expands to.
* (Optional) Enter any Arguments for your search macro. This is a comma-delimited string of argument names. Argument names may only contain alphanumeric characters (a-Z, A-Z, 0-9), underscores, and dashes. The string cannot contain repetitions of argument names.
* (Optional) Enter a Validation expression that verifies whether the argument values used to invoke the search macro are acceptable. The validation expression is an eval expression that evaluates to a Boolean or string value.
* (Optional) Enter a Validation error message if you defined a validation expression. This message appears when the argument values that invoke the search macro fail the validation expression.
* Click Save to save your search macro.

For example, you have a search macro named mygeneratingmacro that has the following definition:

`tstats latest(_time) as latest where index!=filemon by index host source sourcetype`

The definition of mygeneratingmacro begins with the generating command tstats. Instead of preceding tstats with a pipe character in the macro definition, you put the pipe character in the search string, before the search macro reference. For example:

``` | `mygeneratingmacro` ```

## Create and use a basic macro
* Want to show various stats about the file size from the web_applications log. 
* The stats are about max, min and avg value of the filesize using the bytes field in the log. 
* The result should display these stats for each file listed in the log.

**Steps**
Create a search macro named`iis_search(1)`with the following definition:
    `sourcetype="iis" cs_username!="-" /$fragment$/ .pdf`
In the Arguments field, enter `fragment` as the argument.  Click `Save`.

You can insert `iis_search(fragment=TM)` into your search string to call the search macro for the TM fragment. 

### Preview your search to see the contents of your macro
Use the the search preview feature to see the contents of search macros that are embedded within the search, without actually running the search. When you preview a search, the feature expands all of the macros within the search, including macros that are nested within other macros.

**Steps**
* Navigate to the Splunk Search page.
* In the Search bar, type the default macro `audit_searchlocal(error)`.
* Use the keyboard shortcut
[![Foo](https://docs.splunk.com/images/thumb/5/58/Expanded_search_string2.png/700px-Expanded_search_string2.png)](https://docs.splunk.com/images/thumb/5/58/Expanded_search_string2.png/700px-Expanded_search_string2.png)

## Creating More Macro Labs:
-----

[![Foo](https://www.tutorialspoint.com/splunk/images/search_macro_2.jpg)](https://www.tutorialspoint.com/splunk/images/search_macro_2.jpg)
----
[![Foo](https://www.tutorialspoint.com/splunk/images/search_macro_3.jpg)](https://www.tutorialspoint.com/splunk/images/search_macro_3.jpg)
----
[![Foo](https://www.tutorialspoint.com/splunk/images/search_macro_4.jpg)](https://www.tutorialspoint.com/splunk/images/search_macro_4.jpg)

## Combine search macros and transactions
* You can combine transactions and macro searches to simplify your transaction searches and reports. 
* A search macro named `makesessions` defines a transaction session from events that share the same clientip value, and that occur within 30 minutes of each other:

`transaction clientip maxpause=30m`

The following search uses the makesessions search macro to take web traffic events and break them into sessions:

```sourcetype=access_* | `makesessions` ```

The following search uses the makesessions search macro to return a report of the number of pageviews per session for each day:

```sourcetype=access_* | `makesessions` | timechart span=1d sum(eventcount) as pageviews count as sessions```

To build the same report with varying span lengths, save the report as a search macro with an argument for the span length. Name the macro pageviews_per_session(1). The macro references the original makesessions macro. Following is the definition for this macro:

```sourcetype=access_* | `makesessions` | timechart $span$ sum(eventcount) as pageviews count as sessions```

When you insert the pageviews_per_session(1) macro into a search string, you use the argument to specify a span length.

`pageviews_per_session(span=1h)`


## Add and use arguments with a macro
Validate arguments to determine whether they are numeric

The following example demonstrates search macro argument validation.

**Steps**

* Select Settings > Advanced Search > Search Macros.
* Click New to create a new search macro.
* For Name, enter newrate(2). The (2) indicates that the macro contains two arguments.
* For Definiton, enter the following:

    ```eval new_rate=$val$*$rate$```
* This definition includes the argument variables val and rate.
* For the Argument field, enter val and rate.
* Enter a Validation expression that verifies that the value supplied for rate is numeric, as follows:
    ```isnum($rate$)```
* Enter the following Validation `error message: The rate value that you provided is not numeric. Enter a numeric rate value`
* Click Save.

When another user includes the newrate(2) macro in a search, they might fill out the arguments like this: `newrate(revenue, 0.79)`.

If they leave the 0 out (`newrate(revenue, .79)`) the macro is invalid because the value .79 lacks a leading zero and is interpreted as a string. To ensure that the argument is read as a floating point number, the user should use the tonumber function as follows: `newrate(revenue, tonumber(.79))` 
