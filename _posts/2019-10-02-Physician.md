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

I will use the selenium package which you can install using pip either from your jupyter notebook or the command line.

```python
!pip install selenium
```
Now I will import all the necessary dependencies for selenium and *pandas* which I will use to create a dataframe.

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
You would also need to download and extract the latest [chromedriver](https://chromedriver.storage.googleapis.com/index.html?path=2.24/). Chromedriver allows python to fire up Google Chrome within your local machine which is then used to access any site you would like to scrape.   

```python
chrome_path = "C:/Users/snigd/Dropbox/IDS 594/chromedriver_win32/chromedriver.exe"
driver = webdriver.Chrome(chrome_path)
```
