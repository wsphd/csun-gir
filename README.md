# csun-gir

**Resources for high-Performance Computing (at no charge!)**

**7th Annual "Gitting into Research" Event**\
**Office of Undergraduate Researc**\
**California State University, Northirdge (CSUN)**

**Friday, September 20, 2024**\
**University Student Union (USU) Northridge Center**

**Zack Hillbruner, Information Technology, <mailto:zack.hillbruner@csun.edu>**

**Wayne Smith, Ph.D., Department of Management, <mailto:ws@csun.edu>**

--------

# fcculs

**Summary**: (enough to start)

This R script contains three main parts:

1. Download the relevant FCC ULS Database files,
2. Combine them using various database techniques, especially "joins", and
2. Generate a set of "flat files" that a 'HAM/Scanner' user might find useful.

There isn't enough room on GitHub to host the resulting files (datasets), so instead they are posted at:\
https://www.qsl.net/n6lhv/scma/fcculs/

There is a file for each SoCal county in two formats: .csv and .xlsx.
  All .csv files (except for LOS ANGELES county) are small enough to be loaded into either the (non-commercial) LibreOffice Calc spreadsheet or the (commercial) MS-Excel spreadsheet.
  Those two spreadsheets have a limit of 1,048,576 rows, and LOS ANGELES county simply won't fit.  The row limit in Google Sheets is even more constrained.
  Therefore, most files will need to loaded either into a database, such as Sqlite or DuckDB, or loaded from a programming language such as R, Python, or Julia.
  Upon filtering, a smaller .csv or even .xlsx file can be written as needed by a user.

**Details**: (for the curious)

I've made specific (opinionated) decisions about the data.
  This was partly done to make the overall process feasible, but also so that a single `.csv` file would load (directly, or indirectly via a database) into a spreadsheet completely.
  It should be relatively easy to modify the code to alter these assumptions.

**Data**:

1. ***Inclusion***
    * I've included both the entire state of California and the entire United States.
    * As to counties, I've included the ten (10) southernmost counties in SoCal beginning with Kern and San Luis Obispo counties in the north.
    * I've included only the 'Active' licenses.  This is just under 70% of the entire ULS database.
    * Either the 'location_county' column or 'control_county' column has to be populated.  If both are blank, the record isn't included in the state-level or county-level files.
    * I've included the 'Emissions' column due to the growing importance (not to mention variety) of digital systems.
    * (Note: The 'Emissions' column is excluded in the US file but its inclusion is on the near-term development roadmap.
2. ***Exclusion***
    * I've excluded the cellular bands.
    * I've excluded all frequencies above 1.3GHz.
3. ***Transformation***
    * I've converted lower (or proper) case to upper case for the State and County fields.  Ditto for the entity_name.
    This eliminates inadvertent mismatches due to case sensitivity.
4. ***Sizes***
    * As used here, the resulting SQLite database size--all tables--is about 29GB.
    The 'US' .csv file (all states) is ~15GB, and the 'CA' .csv file is ~23GB (due to the 'Emissions' column).  Therefore, these files are posted in 'zipped' format and 'parquet' format.
    Using ZIP, the .csv files will compress about 96%.
    Parquet files are compressed automatically as part of the file format.
    I've left the county-level files in non-zipped format; on the down side, this lengthens the download time, but on the up side, facilitates easier and more direct use by many users (albeit, zipped files need to be unzipped).

**Code**:

I've tried to use Base R functionality in most places.
  * The few libraries that are used are listed at the top of the R script. They need to installed into R from CRAN first.  This is easy to do and documented in the R script at the top.
  * Although unconventional in the general R community, I prefix my variables with the object type of the variable (e.g., "s." for single values and "df." for dataframes to aid in readability.
  * After downloading the R program from https://www.r-project.org/, you can use the `source` function to run the R script (e.g., `source( "uls-36.r" )`.
  * The code begins with the `main()` function at the bottom of the R script.
  * The code is very liberally commented.  This should help new users generally, and moreover, advanced users with various integration, customization, and extensions specifically.

**Timing**:

  * ***Network:*** It takes about 20 minutes to download all the necessary FCC ULS files.
    This rate appears to be limited on the FCC side either at the network-level or server-level.
  * ***DBMS:***  Locally, with respect to database management, the database joins generally take the most amount of time, even using indexes.
    More RAM (e.g., more than 16 MB) and faster hard drives (e.g., NVMe solid-state devices) help.
    I've successfully run the script on a laptop with 16GB or RAM, but frankly, more is recommended.
    And since a sophisticated database query JOIN will inevitably swap to disk, faster, solid-state drives are preferred to slower, hard-disk drives.
    I've used SQLite internally but I've kept the joins in SQL (rather than, say, `dplyr` or `data.table`) so that it's relatively easy to to switch DBMS back-ends.
    Like most analytical work, a columnar-oriented database such as DuckDB might be a better long-run choice than Sqlite..

**Tips**:

  * ***Searching:*** There are many spelling variants and misspellings, and frankly, overt errors and ommissions.  It would be a herculean effort to try to correct those.
    Although somewhat slower, I would encourage users to use wildcard searches or similiar search strategies, at least initially, for cities, addresses, and entity names.

I welcome your feedback.


Enjoy,

Wayne Smith, Ph.D.\
N6LHV\
[Southern California Monitoring Association (SCMA)](https://socalscanner.com/)\
<mailto:n6lhv@arrl.net>

