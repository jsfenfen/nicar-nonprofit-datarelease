# Documentation for nonprofit data release at NICAR 20 - New Orleans


## What is this? 

This is a reference repository for data extracted from nonprofits' electronically filed tax returns, which are public. It contains documentation and reference materials as well as the contents of about 2.2 million returns in data files detailed below.

## What data is available

More details are available below, here are links for the impatient. All of these files except the first come from Schedule L, also see [the blank form](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/sample_schedules/f990sl.pdf) and the [filing instructions](https://github.com/jsfenfen/990-xml-reader/blob/master/irs_reader/schedule_instructions/i990sl.pdf) for nitty-gritty details about who must be included. 

 - [Employees.csv.gz](http://www.jacobfenton.com/990data/NICAR20/employees.csv.gz) is a 400MB gzipped file of best paid employees and directors disclosed on forms 990, 990PF and 990EZ. About 20 million rows. Documentation [is here](documentation/employees_documentation.csv).

 
- [Loans_from.csv](http://www.jacobfenton.com/990data/NICAR20/loans_from.csv) Is a file of loans from nonprofits to insiders. Many states prohibit loans to directors, although there are some caveats. From Schedule L "Part II Loans to and/or From Interested Persons". About ~45,000 rows.
 
- [Loans_to.csv](http://www.jacobfenton.com/990data/NICAR20/loans_to.csv) Is a file of loans to nonprofit organizations from insiders. From Schedule L "Part II Loans to and/or From Interested Persons". About 61,000 rows.

- [Insider_assistance.csv](http://www.jacobfenton.com/990data/NICAR20/insider_assistance.csv) Is a file of loans to nonprofit organizations from insiders. From Schedule L "Part II Loans to and/or from Interested Persons". About 11,000 rows.

- [Insider_transactions.csv](http://www.jacobfenton.com/990data/NICAR20/insider_transactions.csv) Is a file of "grants or assistance benefitting interested persons" from Schedule L, Part III. About 178,000 rows. 




## What data is being released

There are two types of data being released: salary and employment data, and disclosures about insider compensation. 

## Where can I read more about the data




## How recent is the data?

This release should include all electronic tax filings released by Jan. 29, 2020. Tax filings using "schemas" from 2013 and earlier are not included. 

## What data is missing?

A new law signed in 2020 requires all nonprofits to file their returns electronically beginning next year although private foundations are given an extra year. Previously only the largest nonprofits were required to file their returns electronically, though many more did so for convenience. 

Estimates vary, but probably at least 80 % of organizations file electronically (the figure is over 95% when computed by assets). Private foundations have the lowest rate of electronic filing at about 50% or organizations.




## Why won't the IRS say who is a nonprofit and who isn't, how can state regulators do their job if they can't even get a list of the organizations they are supposed to regulate?

That's a rhetorical question, right? 
