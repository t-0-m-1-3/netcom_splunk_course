## Correlating Events
----
inding associations and correlations between data fields, and operating on multiple search results, can provide powerful insights into your data. The following commands can be used to build correlation searches:
| Search command | Description                                                                                                                |
|----------------|----------------------------------------------------------------------------------------------------------------------------|
| append         | Appends subsearch results to current results.                                                                              |
| appendcols     | Appends the fields of the subsearch results to current results, first results to first result, second to second, etc. |
| appendpipe     | Appends the result of the subpipeline applied to the current result set to results.                                        |
| arules         | Finds association rules between field values.                                                                              |
| associate      | Identifies correlations between fields.                                                                                    |
| contingency    | Builds a contingency table for two fields.                                                                                 |
| correlate      | Calculates the correlation between different fields.                                                                       |
| diff           | Returns the difference between two search results.                                                                         |
| join           | SQL-like joining of results from the main results pipeline with the results from the subpipeline.                          |
| lookup         | Explicitly invokes field value lookups.                                                                                    |
| selfjoin       | Joins results with itself.                                                                                                 |
| set            | Performs set operations (union, diff, intersect) on subsearches.                                                           |
| stats          | Provides statistics, grouped optionally by fields. See also, Functions for stats, chart, and timechart.                    |
| transaction    | Groups search results into transactions.                                                                                   |

### Identify transactions
----
* The transaction command finds transactions based on events that meet various constraints. 
* Transactions are made up of the raw text (the _raw field) of each member, the time and date fields of the earliest member, as well as the union of all other fields of each member.
* Additionally, the transaction command adds two fields to the raw events, `duration` and `eventcount`. 
* If there are more than 5 events in a transaction, the remaining events are collapsed. A message appears at the end of the transaction which gives you the option to show all of the events in the transaction. 
### Lab Work:
1. Transactions wit hthe same host:
    1. `... | transaction host cookie maxspan=30s maxpause=5s`
2. Transactions with the same "from" value, time range, and pause:
    1. `... | transaction from maxspan=30s maxpause=5s`
3. Transactions with the same field values:
    1. `... | streamstats window=2 current=t latest(alert_level) as last earliest(alert_level) as first | transaction endswith=eval(first!=last) | table _time duration first last alert_level eventcount`
4. Transactions of Web Access events based on IP address:
    1. `sourcetype=access_* | transaction clientip maxspan=30s maxpause=5s`
5. Transactions of Web Access events based on host and client IP:
    1. `sourcetype=access_* | transcation clientip host maxspan=30s maxpause=5s`
6. Purchase transactions based on IP and time range:
    1. `sourcetype=access_* action=purchase | transaction clientip maxspan=10m maxevents=3`
7. Transactions with the same sessionID and IP:
    1. `sourcetype=access_* | transction JSESSIONID clientip startswith="view" endswith="purchase" | where duration>0`

* Determine when to use `transactions` vs. `stats`
##### The transaction command is most useful in two specific cases:
* Unique id (from one or more fields) alone is not sufficient to discriminate between two transactions. This is the case when the identifier is reused, for example web sessions identified by cookie/client IP. 
  * In this case, time span or pauses are also used to segment the data into transactions. 
  * In other cases when an identifier is reused, say in DHCP logs, a particular message may identify the beginning or end of a transaction.

* When it is desirable to see the raw text of the events combined rather than analysis on the constituent fields of the events.

* In other cases, it's usually better to use stats as the performance is higher, especially in a distributed search environment.
    * Often there is a unique id and stats can be used.
[![Foo](https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_1.png)](https://www.3pillarglobal.com/wp-content/uploads/2016/04/splunk_1.png)
