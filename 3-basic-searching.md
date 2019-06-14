# Book Stuff
----
## Chapter 2: Understanding Search:
* Using terms effectively:
    * Search terms are case insensitive
    * search terms are additive
    * only the time frame specified is quereied
    * Search terms are words, not parts of words
    * A word is anything surrounded by whitespace
    * Splunk is not grep with an interface
    * Numbers are not numbers until after they have been parsed at search time
    * Field names are case sensitive
    * Fields do not have to be defined before indexing data
* Boolean and Grouping Operators:
    * Few operators you can use to refine your searches:
        * `AND` is implied between terms
        * `OR` allows you to specify multiple values
        * `NOT` applies to the next term or group
        * `""` identifies phrases
        * `()` is used for grouping terms
        * `=` is reversed for specifying fields
        * `[]` is used for subsearches
    * You can use these operators in fairly complicated wyas
* Clicking to modify your search:
    * CLicking any word or fields value will add it to the search, if it's already there it will remove it
    * CLicking while holding `ALT` will append the search to the query appended by `NOT`
* Event Segementation:
    * Different options change what is highlighted as you mouse over text
* Field Widgets:
    * clicking on values will append them to the query
    * field values look like `key=value` in the text of an event
* Time:
    * You can click in on the time frame to zoom in on a search and not repeat that search
* Using Fields in Search:
    * Fields appeart in the field picker on the left andudner every event
* Using the Field Picker:
    * field picker gives us easy access to the fields currently defined for the results of our query. Click on any fields presents the details of that field
    * Wealth of information inside of the widget:
        * Appeasrs inX% of Results
        * Show only events with this field
        * Select and Show in results
        * Charts ( both top values by time and overall )
        * Values
* Using Wildcards Effectively:
    * possible to use wildcards when needed
    * Only Trailing wildcards are efficient `*bob` is less effieicnt thant `bob*`
    * Wilcards are tested last: `world message="*world*"` is a more restrictive search 
* All About Time:
    * Time is an important and confusing topic inside of Splunk. Time must be parsed on the way into the index, once data is indexed, it cannot be changed. 
* How Splunk Parses Time:
    * You can congigure your own time format; but splunk does it's best to be intuitive
* How Splunk Stores Time:
    * GMT epoch Time, which is the number of seconds since the birth of unix; 1/1/1970
    * Splunk displays time as it is received. 
* How time zones are determeiend and why that matters:
    * time zone of an event only matters at parsing time
    * The docs contain a listing of the order of precedence for time parsing. 
* Different Ways to Search Against Time:
    * In the search apps drop down menu you can select exact or relative times. 
    * **Snap**ing to times, is done using the `@` character, it will round DOWN towards the time you are searching
    * You can also specify time, inline, in searches using `earliest` and `latest` fields 
* `_indextime` vs `_time`:
    * events are generally not recieved at the exact same time; discrepancy can cause latency issues; 
    * important to remember time displayed is in the `_time` fields
* Making Searching Faster:
    * Starting a new search, you can speciffy the minimum time frame require to locate events
    * Specify the index, if you have multiple indexes
    * Specify other fields that are relevant `sourcetpye` and `host` are the most common
    * Add more words relevant to the messages
    * Expand you time range once you found events you need
    * disable **Field Discovery**
* Sharing Results:
    * You can save and share results with people as long as you edit the appropriate permissions
    * The Results will also be in the `jobs` window for a set amount of time **10 minutes default**
    * Building a query, saving it, and making an alert out of it is a great way to streamline your applications monitoring processes. 
    * Enter a name for the search after clicking on the menu dropdown. 
    * Share allows you to edit permissions
* Creating Alerts from Searches:
    * Saved Searches can be run on a schedule, triggering alerts based on conditions of data found 
    * Click **Alert** to bring up the menu
    * **Schedule** step: provides a gui for real-time or onve every...
    * **Trigger** lets you choose the conditions of how the alert will be triggered
    * **Actions** lets you choose how the system will respond to the alerts found. 
    * Running a script is deprecated, en leu of custom actions

## Run basic searches
Just start typing

## Use autocomplete to help build a search
  * Once you start typing a search, the assistant will pop open with Autocomplete suggestions for you
  * You can keep typing or select word from the list
  * Once you type a pipe `|` the assistant shows a list of commands to enter into the string
  * You can then, keep typing or scroll through and select a string.
  * Mouse over for more information on a command
      * You can change the preferences for search assistant, enabled by default in the user preferences menu
      * Default selection is *Compact*, choose *Full* for more information
      * *Parentheses* are matched as you type, and matching ones are highlighted.
      
## Identify the contents of search results
   * Matching results are returned immediately in reverse chronological order
   * Events are parsed from machine data, to have time and metadata fields extracted
      * timestamp, host, source, sourcetype, index
## Refine searches
   * Mousing over a search, keywords are highlighted, clicking these will add/exclude then to the search
   * Results can be listed as *tables*, *lists*, and *raw* format. 
      
## Set the time range of a search
   * Selecting a Specific Time in the dropdown next to the searchbar.
   * *Time Range Abbreviations*:
       * ranges are specified in the *Advanced* tab, 
       * Unit abbreviations are: `s = seconds`, `m = minutes`, `h = hours`, `d= days`, `w = week`, `mon = months`, `y = year`
       * `@` will round down to the time unit you specify:
           * a time of `09:37:12` for a search with `-30@h` looks back to `09:00:00`
           * time ranges can also be specified into the search bar:
               * `earliest=-h` will look back an hour
               * `earliest=-2d@d latest=@d` looks back form two days ago, up to the beginning of today
               * `earliest=6/15/2017:12:30:00` looks back to that specific time
## Use the timeline
   * The Timeline will show distributions of events specified in the range
## Work with events
   * mouse over for details
## Control a search job
   * You can select a narrower range of time range, and you can click and drag across a series of bar. It will filter the search, without reexecuting it. 
   * There are other timeline controls, *Format Timeline*, *Zoom Out*, *Zoom To Selection*, *Deselect*
   * *Controlling and Saving Search Jobs*
       * every search is a job
       * use the job bar to control the execution. *Pause* and *Stop*
       * Setting Job Permissions: *Private*, *Everyone*, and *Lifetime*
   * *Sharing Search Results* Click the share bar
       * Give everyone read permissions, extend sharable results for 7 days, get a shareable link
       * Multiple users can see the same thing
    * *Exporting Search Results*: *CSV*, *XML*, *JSON*, or *Raw*
## Save search results
   * **Viewing Your Saved Jobs** in the activity window.
   * **Viewing Your Search History** in the Search and Reporting App Home Page

   
# Queries:
----
1. `source="linux_secure" process=sudo` and then pipe this to `top` 
2. `source="linux_secure" process=sudo | top _time` we get some counts but lets get more granular
3. `source="linux_secure"  | top _time limit=15 process=sshd`
4. `source="linux_secure"  | top _time limit=5 useother=true otherstr="everything else" process=*`
5. `source="linux_secure" process=* | top _time by process` this looks like a decent layout; now for timechart 
6. Basic stats statement: `stats function by fields`:
    1. `sourcetype=linux_secure process | stats count`
    2. `sourcetype=linux_secure process | stats count by host` 
    3. `sourcetype=linux_secure process | stats count avg(_time) max(_time) as "Slowest Times" by host`
    4. find the most recent time: `sourcetype=linux_secure process | stats count max(_time) as _time by process`
    5. using chart to turn stuff: `sourcetpye=linux_secure process | chart count over FIELD by FIELD`:
        1. Chart generates the intersection of fields, can be used to turn data
        2. `sourcetpye=linux_secure process | chart usenull=false count over _time by host`
    6.  `sourcetpye=linux_secure process | timechart count`: the default chart is like a sparkline
        1. `sourcetpye=linux_secure process | timechart count by FIELD`
        2. `sourcetpye=linux_secure process |timechart bins=100 limit=3 useother=false usenull=false count as "SET Count" by admin`

