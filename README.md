# dmarcts-report-viewer
A PHP viewer for DMARC records that have been parsed by [John Levine's rddmarc script](http://www.taugh.com/rddmarc/) or the [dmarcts-report-parser.pl](https://github.com/techsneeze/dmarcts-report-parser) into a MySQL database.

### Features
* View a table of parsed reports
* Easily identify potential DMARC related issues through red, orange, yellow and green icons
* Filter report list by DMARC result, month, domain and reporting organization
* Sort report list table by any column
* View DKIM/SPF details for each report in a table, with the same red/orange/yellow/green colour-coding
* Sort report detail table by any column
* View the raw XML of the report beside the report detail table
* Uses AJAX calls to the MySQL database; no external Javascript libraries are needed

## Screenshots
### Screenshot: Initial Report Listing
![alt text](http://www.techsneeze.com/wp-content/uploads/2020/07/dmarcts-report-viewer.InitialReportListing-300x252.png "Screenshot: Initial Report Listing")

[Larger version](http://www.techsneeze.com/wp-content/uploads/2020/07/dmarcts-report-viewer.InitialReportListing.png)

### Screenshot: Report Detail
![alt text](http://www.techsneeze.com/wp-content/uploads/2020/07/dmarcts-report-viewer.ReportDetail-300x252.png "Screenshot: Report Detail")

[Larger version](http://www.techsneeze.com/wp-content/uploads/2020/07/dmarcts-report-viewer.ReportDetail.png)

### Screenshot: Report Detail with Raw XML
![alt text](http://www.techsneeze.com/wp-content/uploads/2020/07/dmarcts-report-viewer.ReportDetailWithXML-300x252.png "Screenshot: Report Detail with Raw XML")

[Larger version](http://www.techsneeze.com/wp-content/uploads/2020/07/dmarcts-report-viewer.ReportDetailWithXML.png)

## Installation and Configuration

### Requirements

* A MySQL database populated with data from [techsneeze.com's dmarcts-report-parser.pl](https://github.com/techsneeze/dmarcts-report-parser) script or [John Levine's rddmarc script](http://www.taugh.com/rddmarc/).

* A working webserver (apache, nginx, ...) with PHP

* Installed `php-mysql` and `php-xml`

### Download dmarcts-report-viewer:
```
git clone https://github.com/techsneeze/dmarcts-report-viewer.git
```

### Configuration

Ensure that all the files are in their own sub-folder.

#### dmarcts-report-viewer-config.php

Copy `dmarcts-report-viewer-config.php.sample` to `dmarcts-report-viewer-config.php`.

```
cp dmarcts-report-viewer-config.php.sample dmarcts-report-viewer-config.php
```

Next, edit these basic configuration options near the top of the `dmarcts-report-viewer-config.php` file with your specific information:

```
$dbhost="localhost";
$dbname="<dmarc-database-name>";
$dbuser="<dmarc-database-username>";
$dbpass="<password-for-dmarc-database-username>";
$dbport="3306";

$cssfile="default.css";

$default_hostlookup = 1;  // Hostname resolution: 1=on 0=off (Turning off host lookup greatly speeds up the program in the case of mail servers that have ceased to exist)
$default_sort = 1;  // Report listing Start Date: 1=ASCdending 0=DESCending (ASCending is default behaviour )
```
#### dmarcts-report-viewer-js
Finally, edit these basic configuration options near the top of the `dmarcts-report-viewer.js` file with your preferences:

```
var default_reportlist_height = 60;  // Main Report List height as a percentage of 
                                     // browser window height (without the % mark)
```

## Usage

Navigate in your browser to the location of the `dmarcts-report-viewer.php` file.

You should be presented with the basic Report List view, allowing you to navigate through the reports that have been parsed.

### Icon Color Legend
* Green : *Both* DKIM and SPF = pass
* Red : *Both* DKIM and SPF = fail
* Orange : *Either* DKIM or SPF (but not both) = fail
* Yellow : Some other condition, and should be investigated (e.g. DKIM or SPF result are missing, "softfail", "temperror", etc.)

### Option Bar
At the top of the page you will find the option bar where you can set:

1. Hostname on/off: This determines whether or not the IP address of the mailserver is resolved into a hostname in the Report Detail.  
   Hostname resolution is fine until an IP address no longer has a reverse DNS entry (as when a mail server is de-commissioned) and it takes an excessive amount of time before the DNS resolution times out. If this is the case, you can turn off hostname resolution.

2. Filter Controls:

   * DMARC Result: Filter by the combined result of DKIM/SPF: pass/pass, fail/fail, pass or fail, other condition 
   * Month: Filter by any month of reports
   * Domain(s): Filter by any domain
   * Reporter(s): Filter by any reporting organization

   If the filter returns no reports, an error message will inform you that no reports meet the criteria you have set. In this case, you can change the filter settings or click on the *Reset* button to clear the filter.

3. Buttons

   * Refresh: This will refresh the data in the Report List while maintaining the currently set filter.
   * Reset: This will reset the filter to show all reports in the Report List and then refresh the data.

### Report List
The Report List table displays all the parsed DMARC reports, initially sorted by Start Date (whether initially ascending or descending is determined by the `$default_hostlookup` option in `dmarcts-report-viewer-config.php`) and initially filtered to show only those reports from the latest month available.

Clicking on a column heading will toggle the sort direction of the Report List table by that column. Clicking on any line of the Report List will display the detailed DMARC information of the selected report below the Report List table.

### Report Detail
The Report Detail table displays the details of the selected DMARC report, initially sorted by IP Address ascending.

Clicking on a column heading will toggle the sort direction of the Report Detail table by that column.

### Raw Report XML
Clicking on the XML icon ![alt text](http://www.techsneeze.com/wp-content/uploads/2020/07/xml.png "XML Icon") will display the raw XML of the currently displayed report. Clicking on the HTML icon ![alt text](http://www.techsneeze.com/wp-content/uploads/2020/07/html.png "HTML Icon") will hide the raw XML report.


More info can currently be found at : [TechSneeze.com](http://www.techsneeze.com/dmarc-report/)
