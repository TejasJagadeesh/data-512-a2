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
Three data sources are used in this project:
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

In the above description, the third column name is mentioned as "last_edit". However, on inspection of the csv file, it is labeled as "revision_id" and not "last_edit".

From the Source link, I downloaded the file "country.zip" and unzipped it to find the csv file "page_data.csv" in the "data" directory. As part of submission to GitHub, I have included the csv file in "data/raw" folder and excluded all other files related to the code to generate the same.

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

From the Source link, I downloaded the csv file "WPDS_2018_data.csv". As part of submission to GitHub, I have included the csv file in the "data/raw" folder.

**License:** There is no information about Licensing in the Source link. However, since the actual source appears to be World Population Data website, referring that indicates that the data is copyrighted by Population Reference Bureau. However, there is no information available on this website on freedom of usage.

**Code to Reproduce the Data:** The data can be downloaded as csv file from the link http://www.worldpopdata.org/table by selecting all countries in the filters.

## 3) Page Quality Score
**Description:**
We use Wikimedia REST API to access ORES (Objective Revision Evaluation Service) web service which is based on machine learning. All details about ORES can be found here - https://www.mediawiki.org/wiki/ORES and https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model and in the embedded links in these pages.

ORES provides probability estimates for each of the following 6 categories. The category with the highest probability is returned as the score. The API also returns probabilities for all the categories. The following categories are ordered from best to worst.

- FA - Featured article
- GA - Good article
- B - B-class article
- C - C-class article
- Start - Start-class article
- Stub - Stub-class article

While using the API, we specify the project as 'enwiki' and model as 'wp10' which uses structural characteristics to provide predictions. The API takes as input a set of revision IDs seprated by "|" character. It is recommended to fetch in batches of 50 revision ids at a time.

**License:** There is not information provied about specific licensing for ORES Wikimedia API. Assumption is that it falls under the same generic license mentioned in the footer of ORES page - https://www.mediawiki.org/wiki/ORES. The license mentioned is Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0) - https://creativecommons.org/licenses/by-sa/3.0/

Sample request with two revision ids (34854345, 485104318), project as 'enwiki' and model as 'wp10' looks like below:

https://ores.wikimedia.org/v3/scores/enwiki/?models=wp10&revids=34854345|485104318

Sample response for the above request looks like below:

{
  "enwiki": {
    "models": {
      "wp10": {
        "version": "0.6.1"
      }
    },
    "scores": {
      "34854345": {
        "wp10": {
          "score": {
            "prediction": "FA",
            "probability": {
              "B": 0.22322826391917108,
              "C": 0.02086552236039792,
              "FA": 0.727835633467006,
              "GA": 0.010004753389648268,
              "Start": 0.0161698481518677,
              "Stub": 0.001895978711909182
            }
          }
        }
      },
      "485104318": {
        "wp10": {
          "score": {
            "prediction": "Stub",
            "probability": {
              "B": 0.006687915236839397,
              "C": 0.011853353376349855,
              "FA": 0.0007003464421344177,
              "GA": 0.0017530069896292625,
              "Start": 0.051243552848899025,
              "Stub": 0.927761825106148
            }
          }
        }
      }
    }
  }
}

# Output
As part of the analysis, page quality score is fetched for each of the revision ids mentioned in the page dataset. This dataset is then combined with the population dataset by using country names as the matching keys. Rows for which matching keys are not found are discarded. Rows for which a quality score was not found are also discarded. This data is saved in csv format with the following columns. Each row corresponds to a single article.

- country - names of countries as mentioned in the page dataset.
- article_name - names of the wikipedia articles as mentioned in the page dataset.
- revision_id - revision_ids as mentioned in the page dataset.
- article_quality - quality scores fetched using the ORES Wikimedia REST API for each of the revision ids.
- population - population data in millions upto mid 2018 as mentioned in the population dataset.

Code to reproduce this output document is available in the jupyter notebook "hcds-a2-bias.ipynb" along with comments.

# Directory Structure
```
data-512-a2/
    |- data/
        |- raw/
            |- page_data.csv 				 // page data
            |- WPDS_2018_data.csv    // population data
    |- hcds-a2-bias.ipynb 				 // source code (Jupyter Notebook)
    |- combined_data.csv   // Output file with combined data
    |- LICENSE 						 // standard MIT License
    |- table1.jpg          // Analysis output - table 1
    |- table2.jpg          // Analysis output - table 2
    |- table3.jpg          // Analysis output - table 3
    |- table4.jpg          // Analysis output - table 4
    |- table5.jpg          // Analysis output - table 5
    |- visualization.jpg   // Visuaization for analysis of countries with most articles
    |- README.md
 ```
 
 # Analysis
 
 Code for all the analysis performed below is documented in the jupyter notebook "hcds-a2-bias.ipynb" along with comments.
 
 **Table 1:** 10 highest-ranked countries in terms of no. of politician articles as a proportion of country population
 
 ![](https://github.com/TejasJagadeesh/data-512-a2/blob/master/table1.jpg)
 
 **Observations:**
 
- As expected, the countries with lower populations were going to be on top as having just a few articles would significantly boost the proportion which is computed as a percentage. Though I didn't expect to see countries like Tuvalu and Nauru which I had not heard of until now. Proportion based on overall population is probabaly not the best way to analyze this data.
- Iceland appears to pop out of this list above with highest no. of articles.

**Table 2:** 10 lowest-ranked countries in terms of no. of politician articles as a proportion of country population
 
 ![](https://github.com/TejasJagadeesh/data-512-a2/blob/master/table2.jpg)
 
 **Observations:**
 
- Once again, since we are ranking based on proportion computed on overall population, the countries with highest populations with English not as their primary language are expected to feature here.
- Another expectation is also to see some under-developed countries with relativley limited access to internet. For example the African countries in the list above.

**Table 3:** 10 highest-ranked countries in terms of no. of GA and FA-quality articles as a proportion of all articles about politicians from that country
 
 ![](https://github.com/TejasJagadeesh/data-512-a2/blob/master/table3.jpg)
 
 **Observations:**
 
- It is very interesting to find North Korea and Saudi Arabia on top of this list considering the tight control on freedom of overall speech in both countries.
- It is also interesting to see that USA has only 82 high quality articles out of a total of 1092. Though 82 is highest in the list, it is ranked 9 due to the ranking based on proportion.
- To dig deeper, we'll need to understand the ORES API Scoring algorithm and find out what attributes contribute towards a higher score.
 
 **Table 4:** All lowest-ranked countries in terms of no. of GA and FA-quality articles as a proportion of all articles about politicians from that country
 
 ![](https://github.com/TejasJagadeesh/data-512-a2/blob/master/table4.jpg)
 
 **Observations:**
 
- All the above countries have the zero high quality articles.
- The countries have been ranked based on the total no. of articles published. Country with highest no. of articles with not a single high quality article is ranked as 1.
- It is interesting to see developed countries like Finland, Belgium and Switzerland have a good count of articles, but zero high quality ones. It is hard to speculate without understanding the scoring mechanism. If I had to hazard a guess, I would attribute the lack of high quality to the fact that English is not the primary language of these countries.

**Analysis of Countries with most articles:**

![](https://github.com/TejasJagadeesh/data-512-a2/blob/master/visualization.jpg)

**Data Representation:**

- X-axis encodes the population (millions).
- Y-axis encodes the total no. of English articles related to politicians.
- Hue encodes countries.
- Size of the markers also encode population (millions) for better visual comparison

**Observations:**

- Though China and India have highest population, they don't have the highest no. of articles. The reason could be attributed to the fact that English is not the native language of these countries and they may have far more articles published about politicians in other medium for wider reach.
- What stands out is the fact the China has more English articles related to politics as compared to USA especially considering how tightly controlled political speech in China is. Though articles per person would tip the scale in favor of USA, I was still expecting USA to be on top in total count as well.
- Considering the fact that English is most widely spoken in the countries USA and UK and both have vibrant political systems, I was expecting most articles to be from one of them. To my surprise, France has the highest number even though English is not its native language.
- Even though Australia is primarily English speaking country, I didn't expect it to stand way above USA in total count of articles, considering the fact the USA has a population more than 10 times that of population of Australia.
- We don't see any country in the top 10 from the continents of Africa and South America. Brazil with a population of around 200 million is noticeable miss.

# Conclusion:
In each segment of anlaysis above the expected biases have been mentioned. Contrary findings and results have also been mentioned. One theory I have about some of the unexpected findings is that, while considering population in computation of proportion, we are using overall population. Instead we should fine tune it to include only the English speaking educated class with access to internet to level the playing field.
