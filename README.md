# Documentation for nonprofit data release at NICAR 20 - New Orleans


## What is this? 

This is a reference repository for data extracted from nonprofits' electronically filed tax returns, which are public. It contains documentation and reference materials as well as the contents of about 2.2 million returns in data files detailed below.

This information is not freely available elsewhere, although employees are searchable at American University's [Public Accountability](https://www.publicaccountability.org/dataset-search/) search and ProPublica's [Nonprofit Explorer](https://projects.propublica.org/nonprofits/advanced_search).  


## What data is available?

All of these files except for employees.csv comes from Schedule L. The best place to get started is with  [a blank copy of Schedule L](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/sample_schedules/f990sl.pdf) and the [IRS' instructions on how to fill it in](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/schedule_instructions/i990sl.pdf) for more information on who needs to be included. See more about the employees file below. 

 - [Employees.csv.gz](http://www.jacobfenton.com/990data/NICAR20/employees.csv.gz) is a 400MB gzipped file of best paid employees and directors disclosed on forms 990, 990PF and 990EZ. About 20 million rows. Documentation [is here](documentation/employees_documentation.csv). 

 
- [Loans_from.csv](http://www.jacobfenton.com/990data/NICAR20/loans_from.csv) Is a file of loans from nonprofits to insiders. Many states prohibit loans to directors, although there are some caveats. From Schedule L "Part II Loans to and/or From Interested Persons". About ~45,000 rows. Documentation [is here](documentation/loans_from_documentation.csv).
 
- [Loans_to.csv](http://www.jacobfenton.com/990data/NICAR20/loans_to.csv) Is a file of loans to nonprofit organizations from insiders. From Schedule L "Part II Loans to and/or From Interested Persons". About 61,000 rows. Documentation [is here](documentation/loans_to_documentation.csv).

- [Insider_assistance.csv](http://www.jacobfenton.com/990data/NICAR20/insider_assistance.csv) Is a file of loans to nonprofit organizations from insiders. From Schedule L "Part II Loans to and/or from Interested Persons". About 11,000 rows. Documentation [is here](documentation/insider_assistance_documentation.csv).

- [Insider_transactions.csv](http://www.jacobfenton.com/990data/NICAR20/insider_transactions.csv) Is a file of "grants or assistance benefitting interested persons" from Schedule L, Part III. About 178,000 rows. Documentation [is here](documentation/insider_transactions_documentation.csv).

- [Excess_benefits.csv](http://www.jacobfenton.com/990data/NICAR20/excess_benefits.csv) Is a file of "excess benefit transactions" with insiders, from Schedule L, Part I. Very rarely used, total of about 450 rows. Documentation [is here](documentation/excess_benefit_documentation.csv).

## How can I run SQL queries against these dbs?

All of the Schedule L tables [are available to query here via datasette](http://datasette.publicaccountability.org/), click on the table name to run queries. The employees file is a big too big for datasette to serve, so you'll have to load it into your own rig. 

We're not quite sure how well this will work under load, we're making these dbs available as datasettes during NICAR though they may be password protected later to save our servers. 

ED NOTE: THE SPECIFICS OF THIS AREN'T NAILED DOWN, WE PROBABLY WILL MOVE THE DATASETTES TO CLOUDRUN DURING NICAR. THE ADVANTAGE OF THEM BEING ON OUR SERVERS IS THAT WE CAN USE OUR PASSWORDS WITH THEM (ONCE I SET IT UP ANYWAYS).


## How can I easily read a raw filing if I just know the EIN and object_id? 

Each of the files contains an EIN, which identifies an organization, and an object_id, which identifies a particular filing. You can find that filing on propublica's nonprofit explorer using the following url pattern:

https://projects.propublica.org/nonprofits/organizations/[EIN]/[object_id]/full

Occasionally a very new filing will not be on their site, but most are.

## Why should I care?

If you report on government, health, schools, politics, housing or the arts you've no doubt spent time reading nonprofit tax filings, which are public documents. Beginning in 2016 this data became available electronically, although the format and lack of documentation has made it difficult to use. I hope that releasing thiedata in tabular format will lower the bar for media and other orgs. 

## What are some common pitfalls?

This data represents all filings, not just the most recent ones. If multiple filings representing the same tax year are found, the one with the last signature date is probably the one to use.


## How recent is the data?

This release should include all electronic tax filings released by Jan. 29, 2020. Tax filings using "schemas" from 2013 and earlier are not included, so all of the filings are later than TY 2014. There is a significant delay from tax filing to release. By the end of 2019, most tax year 2018 filings have been released, but many orgs are given extensions. 

## What data is missing?

A new law signed in 2020 requires all nonprofits to file their returns electronically beginning next year although private foundations are given an extra year. Previously only the largest nonprofits were required to file their returns electronically, though many more did so for convenience. 

Estimates vary, but probably at least 80 % of organizations file electronically (the figure is over 95% when computed by assets). Private foundations have the lowest rate of electronic filing at about 50% or organizations.

## How can I tell who has filed electronically?

TKTK 

## What specific sections of the 990 does the employees file use?  

The queries used to create these files are listed in [the queries file](documentation/queries.md) and it's possible if impractical to follow the variable name transformations applied.

The employees listed in this file are those disclosed in forms 990, 990PF and 990EZ. Additional information about a subset of these employees is available in schedule J, but these fields were chosen because they provide the broadest listing of directors and best-paid employees. 

- On form 990, the salaries come from Part VII Section A; see IRSX documentation for this part [here](http://www.irsx.info/metadata/groups/Frm990PrtVIISctnA.html), or find it on the [blank 990](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/sample_schedules/f990.pdf).
- On form 990PF, the salaries come come from 990PF's part VIII table of Officer Directors KeyEmployees; see IRSX documentation for this part [here](http://www.irsx.info/metadata/groups/PFOffcrDrTrstKyEmpl.html), or find it on the [blank 990PF](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/sample_schedules/f990pf.pdf). 
- On form 990EZ, the salaries come come from 990EZ's part IV table of Officers, directors, trustees and key employees; see IRSX documentation for this part [here](http://www.irsx.info/metadata/groups/PFOffcrDrTrstKyEmpl.html), or find it on the [blank 990EZ](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/sample_schedules/f990ez.pdf).

## Where did this come from?

The IRS releases electronic filings as .xml files to a public amazon bucket, read more about it [here](https://docs.opendata.aws/irs-990/readme.html). This data was created by extracting values with a python library called [IRSx](https://github.com/jsfenfen/990-xml-reader) I wrote a few years ago and dumped into a [database](https://github.com/jsfenfen/990-xml-database). In general, the technical hurdle to using this data is that the xpaths to a given variable can change slightly between years; IRSx is intended to create a consistent name, whenever possible.

The naming convention used by [IRSx is here](http://www.irsx.info/), but the original names have been altered to prevent confusion when two parties are present. 

Each dataset above includes a link to documentation with the column names used.



## Where can I get a listing of all nonprofits

It does not exist. You can find a mostly complete listing of approved nonprofits in the IRS exempt organizations master file, but that listing may not include recent organizations that have yet to file a tax return, filers who have been automatically removed fof failing to file taxes for three years, or any organization that decides to operate as a nonprofit without seeking nonprofit status, as allowed by federal law.

