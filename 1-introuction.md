##       How to Use the Labs
  -----
  * VirtualBox Primer
  * Starting, stopping, snapshots
  * thing 
  * another thing
     
##        Overview of Data Examples
  -----
* Buttercup Games
* Navigate to $SPLUNK_HOME/etc/apps/sample_app/logs, select maillog, then click Open.
* For Source type, click Email, then select sendmail_syslog.
* [search tutorial](https://docs.splunk.com/Documentation/Splunk/7.2.6/SearchTutorial/Systemrequirements#Download_the_tutorial_data_files)
  * Download the data set from Add data tutorial and follow the instructions to load the tutorial data.
  * upload the entire `.zip`
  * In the add data wizard, at the `Input Settings` portion; the regular expression needed for **Host**:
    * click **Segment in Path**; Type `\\(.*)\/`
  * Download the CSV file from Use field lookups tutorial and follow the instructions to set up the lookup definition to add price and productName to the events. 
    * Click `Settings` look for `Lookups` and click
    * From there, upload the `.csv` and name it as the name of the `prices.csv` file. 
    * Share it with all Apps

* Updating the MMDB file
    * You can replace the version of the .mmdb file that ships with the Splunk software with a copy of the paid version of the file or with a monthly update of the free version of the file.

  1. From http://dev.maxmind.com/geoip/geoip2/geolite2/, download the binary gzipped version of the GeoLite2 City database file.
  2. Copy the file to the search head on your Splunk Enterprise instance.
  3. Expand the GZ file.
  4. Stop any real-time searches that are running.
  5. Copy the GeoLite2-City.mmdb file to the $SPLUNK_HOME/share/ directory to overwrite the file there.
  6. Restart the real-time searches.
  
  * DataGenerator
  * EventGen
