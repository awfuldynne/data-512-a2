# data-512-a2 - Bias in Wikipedia Data

Sean Miller (<millsea0@u.washington.edu>) 10-30-2017

#### Overview

This repository outlines performing an analysis of bias in the coverage of politicians on the English Wikipedia by combining three data sets. We leverage a data set scraped from Wikipedia of collected articles tagged with "Category:Politicians by nationality" and their respective country, a data set of population per country as of mid-2015 and the Wikipedia [**ORES API**](https://www.mediawiki.org/wiki/ORES) to gather information on page quality for each of the articles in the first data set. The **ORES API** returns a class of article within a subset of the [Grade](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_assessment) chart.

We will evaluate two metrics to analyze how Wikipedia might show bias in the coverage of politicians.

1. The percent of articles-per-poulation for each country
2. The percent of high quality articles for each country

#### Analysis

First let us look at the metric for percent of articles-per-population for each country. From my own intuition, I had assumed that at least one country would have the equivalent of one percent of its population in articles about politicians, especially those with smaller populations but the largest percentage achieved was less than half of that (0.48% for Nauru - Population 10,860). Since we are looking at articles from the English Wikipedia, I would expect that this metric is quite biased against countries with large populations where the native language is not English. We can see that the top three countries with the lowest percent of articles-per-population match this criteria and are the countries with the three largest populations that are not the United States. This analysis would benefit from having an additional column for national language so we could see how this metric performs between countries that speak English and those that do not.

The second metric, the percent of high quality articles for each country measures what percent of articles for a given country received a grade of FA or GA. The first thing that jumps out at me is that while North Korea has one of the lowest percentages of articles-per-population, it also has the highest percent of high quality articles. I would have expected that an English speaking nation would have topped the list for high quality articles but I believe that a combination of having very few articles (39) and having those individuals be of note in the geopolitical scene increases the likelihood that more of the articles would be of a higher quality. North Korea has also been heavily in the news in recent years due to the threat of nuclear capability so it is possible that has provided users with more material to flesh out articles. 39 of the 187 countries we had a population for have no high quality articles on politicians. I was surprised to see that this list includes Belgium and Switzerland. For these nations, it would be interesting to see the percentage of high quality articles for their respective national language's Wikipedia.

While I've suggested additional data points that could improve our ability to detect bias within the English Wikipedia on the coverage of politicians, I think that expanding the number of categories we look for within the English Wikipedia would provide us with a larger dataset of articles to work with. It's possible that for countries that do not speak English as their national language that they are also less likely to interact with the English Wikipedia and learn the ins and outs of its category  system. Perhaps there is a group of categories that would perform better than the single category of "Category:Politicians by nationality". To have a better context of the steps necessary to create a Wikipedia article, I feel that it would be helpful to learn to edit Wikipedia articles myself.

#### Example Output

**Top 10 Countries - Percent of Articles that are High Quality**

| Country | Percent of High Quality Articles |
| ------------------------------------------ |
| Korea, North | 0.230769 |
| Romania | 0.129310 |
| Saudi Arabia | 0.126050 |
| Central African Republic | 0.117647 |
| Qatar | 0.098039 |
| Guinea-Bissau | 0.095238 |
| Vietnam | 0.094241 |
| Bhutan | 0.090909 |
| Ireland | 0.081365 |
| United States | 0.078324 |

#### License(s)

Data retrieved from the ORES webservice [(Wikimedia API)](https://wikimediafoundation.org/wiki/Terms_of_Use/en) is licensed under [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/).

[English Wikipedia article data](https://figshare.com/articles/Untitled_Item/5513449) is provided under the [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) license.

It is unclear as to the license for the [Country Population Mid-2015](http://www.prb.org/DataFinder/Topic/Rankings.aspx?ind=14) data set from the website itself.

#### Libraries

All of the following code was written and tested against the default packages present in **Anaconda3 v4.4.0**. You can find a download for Anaconda and its latest versions at <https://repo.continuum.io/archive/>.

#### APIs

To retrieve data on Wikipedia page quality, Wikimedia provides the **ORES API** which is machine learning as a service. From this service we are able to leverage the *wp10* model which "...predicts the assessment class of an article"[1]. As part of the [Wikimedia Foundation Terms of Use](https://www.mediawiki.org/wiki/REST_API) you are asked to limit requests to the API to fewer than 200/sec and to set a User-Agent field in the header.

For example, setting the header value in Python looks like the following. This headers variable is passed into your get request.
```python
headers = {"User-Agent" : "https://github.com/awfuldynne", "From" : "millsea0@uw.edu"}
```

###### Links to API Documentation

| API | URL | Documentation | License |
|--------------------------------|
| Wikipedia ORES | [ORES](https://www.mediawiki.org/wiki/ORES) | [ORES Swagger](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model) | [CC-BY-SA 3.0](https://wikimediafoundation.org/wiki/Terms_of_Use/en#7._Licensing_of_Content) |

###### Structure of Repo
```
data-512-a2
| -clean_data (Contains any cleaned and processed data sets)
| - | - en-wikipedia_politician_article_quality.csv
| - raw_data (Contains any raw data such as responses from the ORES API)
| - | - article_quality_data.csv
| - | - page_data.csv
| - | - Population Mid-2015.csv
| - hcds-a2-bias.ipynb (The Jupyter Notebook)
| - LICENSE
| - READEME.md
```

**Raw Data Files**

| API | File Name | URL | Documentation | License |
|--------------------------------|
| EN-Wikipedia Articles On Politicians | page_data.csv | [Figshare](https://figshare.com/articles/Untitled_Item/5513449) | Same as URL | [CC-BY-SA 4.0](https://figshare.com/articles/Untitled_Item/5513449) |
| Country Population Data (Mid-2015) | Population Mid-2015.csv | [Population Research Bureau website](http://www.prb.org/DataFinder/Topic/Rankings.aspx?ind=14) | Same as URL | No reference |

*Structure of raw_data/article_quality_data.csv*

| Column | Value |
|-----------------------|
| rev_id | Integer ID that maps to the given Wikipedia page's last edit |
| article_quality | Quality of the Article as determined by ORES |

*Structure of raw_data/page_data.csv*

| Column | Value |
|-----------------------|
| page | Name of the Wikipedia article |
| country | Name of the country the article belongs to |
| rev_id | Integer ID that maps to the given Wikipedia page's last edit |

*Structure of raw_data/Population Mid-2015.csv*

| Column | Value |
|-----------------------|
| Location | Name of the country |
| Location Type | Type of Location the Location column maps to |
| TimeFrame | Label of when this data was snapshot |
| Data Type | Type of data in the Data column |
| Data | Number of people living in the country in mid-2015 |
| Footnotes | Any additional information |

**Cleaned Data Files**

*Structure of clean_data/en-wikipedia_politician_article_quality.csv*

| Column | Value |
|-----------------------|
| country | Name of the country the article belongs to |
| article_name | Name of the Wikipedia article |
| revision_id | Integer ID that maps to the given Wikipedia page's last edit |
| article_quality | Quality of the Article as determined by ORES |
| population | Number of people living in the country in mid-2015 |

#### References

1. <https://www.mediawiki.org/wiki/ORES#Assessment_scale_support>