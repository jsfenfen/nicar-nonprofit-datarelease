# Documentation for nonprofit data release at NICAR 20 - New Orleans


## What is this? 

This is a reference repository for data extracted from nonprofits' electronically filed tax returns, which are public. It contains documentation and reference materials as well as the contents of about 2.2 million returns in data files detailed below.


## What data is available

More details are available below, here are links for the impatient. All of these files except the first come from Schedule L, also see [the blank form](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/sample_schedules/f990sl.pdf) and the [filing instructions](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/schedule_instructions/i990sl.pdf) for nitty-gritty details about who must be included. 

 - [Employees.csv.gz](http://www.jacobfenton.com/990data/NICAR20/employees.csv.gz) is a 400MB gzipped file of best paid employees and directors disclosed on forms 990, 990PF and 990EZ. About 20 million rows. Documentation [is here](documentation/employees_documentation.csv).

 
- [Loans_from.csv](http://www.jacobfenton.com/990data/NICAR20/loans_from.csv) Is a file of loans from nonprofits to insiders. Many states prohibit loans to directors, although there are some caveats. From Schedule L "Part II Loans to and/or From Interested Persons". About ~45,000 rows. Documentation [is here](documentation/loans_from_documentation.csv).
 
- [Loans_to.csv](http://www.jacobfenton.com/990data/NICAR20/loans_to.csv) Is a file of loans to nonprofit organizations from insiders. From Schedule L "Part II Loans to and/or From Interested Persons". About 61,000 rows. Documentation [is here](documentation/loans_to_documentation.csv).

- [Insider_assistance.csv](http://www.jacobfenton.com/990data/NICAR20/insider_assistance.csv) Is a file of loans to nonprofit organizations from insiders. From Schedule L "Part II Loans to and/or from Interested Persons". About 11,000 rows.

- [Insider_transactions.csv](http://www.jacobfenton.com/990data/NICAR20/insider_transactions.csv) Is a file of "grants or assistance benefitting interested persons" from Schedule L, Part III. About 178,000 rows. 

- [Excess_benefits.csv](http://www.jacobfenton.com/990data/NICAR20/excess_benefits.csv) Is a file of "excess benefit transactions" with insiders, from Schedule L, Part I. Very rarely used, total of about 450 rows. Documentation [is here](documentation/excess_benefit_documentation.csv).




## Where did this come from?

The data is released by IRS to a public amazon bucket. This data is extracted using a python library called [IRSx](https://github.com/jsfenfen/990-xml-reader) I wrote a few years ago and dumped into a [database](https://github.com/jsfenfen/990-xml-database). 

The naming convention is often consistent with the [IRSx names](http://www.irsx.info/), but have been altered to prevent confusion when two parties are present. The queries used to extract the data, which contain name transforms, are posted here. 




## What are some common pitfalls?

This data represents all filings, not just the most recent ones. If multiple filings representing the same tax year are found, the one with the last signature date is probably the one to use.


## How recent is the data?

This release should include all electronic tax filings released by Jan. 29, 2020. Tax filings using "schemas" from 2013 and earlier are not included, so all of the filings are later than TY 2014. There is a significant delay from tax filing to release. By the end of 2019, most tax year 2018 filings have been released, but many orgs are given extensions. 

## What data is missing?

A new law signed in 2020 requires all nonprofits to file their returns electronically beginning next year although private foundations are given an extra year. Previously only the largest nonprofits were required to file their returns electronically, though many more did so for convenience. 

Estimates vary, but probably at least 80 % of organizations file electronically (the figure is over 95% when computed by assets). Private foundations have the lowest rate of electronic filing at about 50% or organizations.

## How can I tell who has filed electronically? 




## Where can I get a listing of all nonprofits

It does not exist. You can find a mostly complete listing of approved nonprofits in the IRS exempt organizations master file, but that listing may not include recent organizations that have yet to file a tax return, filers who have been automatically removed fof failing to file taxes for three years, or any organization that decides to operate as a nonprofit without seeking nonprofit status, as allowed by federal law.