---
title: "Text Mining of Physician Reviews: Gathering Data"
date: 2019-10-02
tags: [machine learning, web scraping, text mining, pandas]
excerpt: "What components of a review actually affect the overall rating of a physician?"
---
This is going to be a two-part post, in this post I describe the problem and walk through the *Python* scraper I built to gather data. The next post talks about the techniques I use to analyse textual data.  

## Problem Statement
Online physician reviews have become increasingly important over the past few years, with more and more patients turning to them in order to select their physician. A 2019 survey indicated that 94% of respondents use online reviews to evaluate physicians at least sometimes (*left*), with 72% of them using the reviews as the first step to finding a new provider (*right*).

<figure class="half">
    <img src="/images/PhyscianReviews/figure1.jpg">
    <img src="/images/PhyscianReviews/figure2.jpg">
</figure>

In the competitive healthcare marketplace, it is thus of crucial importance that physicians act to optimize their practice to yield positive reviews on these websites. In order to do so, physicians, and their respective practice managers, must understand what exactly patients consider when constructing their online review. Thus, the primary objective of this project was to answer the following question: What do patients really care about when they are rating their physicians? Is it a physician's bedside manner, staff behaviour, communication? Can we identify broad themes from reviews and do these themes affect the overall rating of a physician?

## Data Gathering: Web Scraping

This is an unique and interesting problem to solve but the data to solve this problem is not readily available. In this section I will discuss the python scraper I wrote that collects data from a popular physician website.

I will use the *selenium* package which you can install using pip either from your jupyter notebook or the command line.

```python
!pip install selenium
```
Now I will import all the necessary dependencies for *selenium* and *pandas* to create a dataframe.

```python
import selenium
import pandas as pd
from selenium.webdriver.common.keys import Keys
from selenium import webdriver
import re
from selenium.common.exceptions import NoSuchElementException
from selenium.common.exceptions import WebDriverException
import time
import csv
```  
You would also need to download and extract the latest [chromedriver](https://chromedriver.storage.googleapis.com/index.html?path=2.24/). Chromedriver allows python to fire up Google Chrome within your local machine which is then used to access the site you would like to scrape.   

```python
chrome_path = "yourpathtochromedriver/chromedriver.exe"
driver = webdriver.Chrome(chrome_path)
```
Lets first understand the scope of this process. I would like my scraper to automatically scrape demographic information and reviews for individual doctors from [Healthgrades](https://www.healthgrades.com). The following screenshot shows the search results for all doctors in Obstetrics and Gynaecology in Illinois. I want my scraper to loop over all the search results which spans across multiple pages and for each page collect the required information for all the doctors listed.

<img src="{{ site.url }}{{ site.baseurl }}/images/PhyscianReviews/figure3.png">

The individual page of a doctor presents another level of complexity, for some doctors all of their reviews are not present on the landing page. The following screenshot shows an example, the scraper would need to click on *show more reviews* until all reviews are shown on the page. Even with individual reviews there are cases where the scraper would have to click on the *read more* option to see the full length review.  

<img src="{{ site.url }}{{ site.baseurl }}/images/PhyscianReviews/figure4.png">

Getting the demographic information from a Doctor's page is relatively straight-forward and thus we will first tackle this problem by creating an user defined function(udf). Later on we will create another UDF to scrape the reviews.

First we create empty lists where we will store our scraped data.

```python
name=[]
gender=[]
age=[]
score=[]
metaReview=[]
```
Next I use selenium's '*.get*' command to navigate to the page I am scraping. To loop over multiple pages, I use the xpath of the element that redirects me to the next page of the website. Note this xpath will differ by website and can be found by right clicking on target element and inspecting the elements tab.  The following code clicks on the xpath (in this case on page 2) and stores the url for each page in a list.

```python
url=driver.get("pageurl")

pageLinks=[]

for i in range(11):
    driver.find_element_by_xpath("//a[@data-hgoname='next page']").click()
    source=driver.current_url
    pageLinks.append(source)
    time.sleep(2)
```
Now for each page, I store the urls of the individual doctor's page in another list. I use a regular expression to remove all unwanted characters from this list of links. I will use this list to iterate through each Physician's page to collect the demographic information and reviews.

```python

#Getting all the individual doctor page links
    list_of_hrefs = []

    doctor_list = driver.find_elements_by_class_name("uCard")

    for block in doctor_list:
        elements = block.find_elements_by_tag_name("a")
        for el in elements:
            list_of_hrefs.append(el.get_attribute("href"))

    print (list_of_hrefs)

    #Removing all unwanted characters in the link list
    try:
        regex = re.compile(r'^tel:')
        href=[x for x in list_of_hrefs if not regex.match(x)]
    except:
        list_of_hrefs=list(filter(None,list_of_hrefs))
        regex = re.compile(r'^tel:')
        href=[x for x in list_of_hrefs if not regex.match(x)]
    finally:
        print (len(href))
```    
<img src="{{ site.url }}{{ site.baseurl }}/images/PhyscianReviews/figure4.png">
