# data-512-a2
DATA-512-HUMAN CENTERED DATA SCIENCE - ASSIGNMENT 2 - BIAS IN DATA

# Goal of the Project
Goals of this project as stated in assignment link here - https://wiki.communitydata.cc/Human_Centered_Data_Science_(Fall_2018)/Assignments#A2:_Bias_in_data are:

- To explore the concept of bias through data on Wikipedia articles - specifically, articles on political figures from a variety of countries. For this assignment, I will have to combine a dataset of Wikipedia articles with a dataset of country populations, and use a machine learning service called ORES to estimate the quality of each article.
- To perform an analysis of how the coverage of politicians on Wikipedia and the quality of articles about politicians varies between countries.
- To create a table that shows the countries with the greatest and least coverage of politicians on Wikipedia compared to their population.
- To create a table that shows the countries with the highest and lowest proportion of high quality articles about politicians.
- To write a short reflection on the project, that describes how this assignment helps me understand the causes and consequences of bias on Wikipedia.

# Data Sources
Two data sources are used in this project:
## 1) Wikipedia Dataset
**Source:** https://figshare.com/articles/Untitled_Item/5513449

**Description:** The above link provides the following description (verbatim) about the dataset:

> The data was extracted via the Wikimedia API using the associated code. It is formatted as a CSV and saved as page_data.csv in the "data" directory. Columns are:
> 
> 1. "country", containing the sanitised country name, extracted from the category name;
> 2. "page", containing the unsanitised page title.
> 3. "last_edit", containing the edit ID of the last edit to the page.
> 
> Country codes are inconsistent. Where possible, they have been modified to match the country names found in http://www.prb.org/DataFinder/Topic/Rankings.aspx?ind=14 - but the PRB dataset contains nations not found in Wikipedia, and vice versa.
> 
> The actual recursion only went 2 levels deep into the category tree: someone listed as an Antiguan politician, say, is included - someone exclusively listed as an Antiguan politician who was assassinated is not.

From the Source link, I downloaded the file "country.zip" and unzipped it to find the csv file "page_data.csv" in the "data" directory. As part of submission to GitHub, I have included the csv file in "raw" folder and excluded all other files related to the code to generate the same.

**License:** As stated in Source link, this dataset is released under the CC-BY-SA 4.0 (https://creativecommons.org/licenses/by/4.0/) license.

**Code to Reproduce the Data:** In the Source link, the following details are mentioned (verbatim) about the code to reproduce the data.

> The code is written in the programming language R, and heavily commented; it can be found in the "code" directory, and is split into 3 files:
> 
> 1. utils.R, which contains utilities for operating the code in the other files;
> 2. retrieve.R, which contains functions for retrieving the category and page data from Wikipedia;
> 3. main.R, which executes the data retrieval code and performs sanitisation before writing it to file.

## 1) Population Dataset
**Source:** https://www.dropbox.com/s/5u7sy1xt7g0oi2c/WPDS_2018_data.csv?dl=0

**Description:**
There is no description provided in the Source link. However, it appears to have been sourced from World Population Data websites (http://www.worldpopdata.org) and Population Reference Bureau (https://www.prb.org/2018-world-population-data-sheet-with-focus-on-changing-age-structures/). 

More details about the enitre population data (of which this data is a subset) can be found in this link - http://www.worldpopdata.org (Click on Notes, Sources, & Definitions in the footer of the page).

The dataset is arranged in tabluar form with one row for each country. There are two columns in the table:
- Geography - This column has the country names.
- Population mid-2018 (millions) - This column has the population in millions.

From the Source link, I downloaded the csv file "WPDS_2018_data.csv". As part of submission to GitHub, I have included the csv file in the "raw" folder.

**License:** There is no information about Licensing in the Source link. However, since the actual source appears to be World Population Data website, referring that indicates that the data is copyrighted by Population Reference Bureau. However, there is no information available on this website on freedom of usage.

**Code to Reproduce the Data:** The data can be downloaded as csv file from the link http://www.worldpopdata.org/table by selecting all countries in the filters.
