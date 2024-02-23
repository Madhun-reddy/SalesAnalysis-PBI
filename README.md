# Sales Analysis
### Objectives
|#| **Create a BI Dashboard/Report that would display the sale analysis; (please make sure that below requirements are captured in the report)** |
|-|--|
|1|	Load all the three tables and use data modelling to relate the tables
|2|	Calculate **revenue** contribution of **producer** (if there is a producer X with 10,000 revenue out of total 100,000; when I select producer X result should show 10%; in the same way with revenue) 
|3|	Group the accounts based on revenue as Tier1 (having revenue > 10000), Tier 2(5000 to 10000), Tier 3 (<5000) 
|4|	Compare quarterly sales for different years (Use Date field)
|5|	Create a tab/Page to show the data or visuals for just the Construction Niche.
|6|	Create a calculated column to derive stage numbers from Stage Name (ex: 3-Closed Won stage number is 3)
|7|	Create a textbox to show producer selected; if there is no producer select show as "Please select a producer"
|8|	User should be able to drill through the report to get producer details. (A bar graph with sales by producer has to be created in one page; a table with account information in one page; user should be able to drill through from graph to view account details page)
|9|	Count of Account ID's that are closed (Stage Name - 3-Closed Won)
|10|	Use Bookmarks feature to navigate to multiple pages; add a button to clear filters; 
|11|	Create a measure to calculate cumulative sales over the quarter 

### Data Source
Assessment excel file contains 3 tables of Sales, Accounts & Producer and they are imported to Power Query to check for 

### Data Transformation
As a first step to checking for data consistency and quality. I have done column & data quality analysis,

### Findings & Steps Taken
    
##### SALES TABLE
|Column Name|	Lab: Data understanding on each variable 	|Solution|	Remarks|
|--|------|-----|----|
|All columns	|Data types was inconsistence with the columns at the first look, so converted data types to their required data types.	|Applied Changed Data Type	||
|Date|	Row-12 contains inconsistence Date values 01-01-1990|		| Need more clarification on this. Task assigned to data engineering team 
|Niche Affiliations|	Row-14 contains Blank values & Errors.|	Blank values are replaced with "na" values for the moment.	|Need more clarification as to what to do with these "na" values. Task assigned to DE team.
|Expected Decision Date|	167 contains errors	| |	Task assigned to DE team.
|Annual revenue|	Row-171 contains an error. Error percent is less than 10% so we will replace error with mean value of Annual Revenue at the moment of discovery	| 1. =Table.ReplaceErrorValues(#"Replaced Blank to Na", {{"Annual Revenue",null}}) 2. =Table.ReplaceValue(#"Replaced Errors",null, List.Average(#"Replaced Errors"[Annual Revenue]),Replacer.ReplaceValue,{"Annual Revenue"}) | Task assigned to DE team. Recommended to check at the backend.  
    - If I try to SORT date column for better visual order. I m getting error as DataFormat.Error: We couldn't parse the input provided as a Date value.Details:
    - So we need further clarity & assistance from the data engineering team to solve the related issues. Once we get clarity we are ready for further exploration.

##### PRODUCER & ACCOUNTS TABLES
Producer and Accounts tables looks to be clean and ready for analysis.

##### CALENDAR TABLE
Calendar table is very much required for doing Date & Time intelligence functions to work in PBI. So created a calendar table using Power query with the help of **StartDate** and **EndDate** parameters. Then used a new query to create a calendar with below mentioned function, 

'= List.Dates(StartDate,Duration.Days(EndDate - StartDate)+1,#duration(1,0,0,0))'


### Data Modelling
Connected all dimension tables like Calendar, Accounts & Producer to fact Sales table using one-many relationships in the model view window.

### Analysis & Visualisation

###### All_measures
Created a new empty folder for holding all measures,

|Measure Name| Description
|--|--|
|Total Sales| Objective No.2 (Revenue contribution of Producer)
|Producer MS%| Producer market share to total sales
|Construction Sales| Construction segment sales from Niche affiliations		
|Closed Acs| Account Ids featured as closed in stage num 3		
|Quarterly Cum Sales | Quarterly Cum Sales = CALCULATE([Total Sales], FILTER(ALL('Calendar'),'Calendar'[CalendarDate] <= MAX('Calendar'[CalendarDate])))	

###### Additional Columns
    1. To get tier group in revenue created a new rev_group column in the power query editor.
    2. To get stage number I have created derived column from stage name in sales table then converted column data type to whole number:  'Stage Num = LEFT(Sales[Stage Name],1)'

### Conclusion & Recommendations
    • STILL PENDING TO COMPLETE
