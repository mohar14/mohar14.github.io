---
title: "Text Mining of Physician Reviews: Gathering Data"
date: 2019-10-02
tags: [machine learning, web scraping, text mining, pandas]
excerpt: "What components of a review actually affect the overall rating of a physician?"
---
This is going to be a two-part post, in this post I describe the problem and walk through the *Python* scraper I built to gather data. The next post talks about the techniques I use to analyse textual data.  

## Problem Statement
Online physician reviews have become increasingly important over the past few years, with more and more patients turning to them in order to select their physician. A 2019 survey indicated that 94% of respondents use online reviews to evaluate physicians at least sometimes **Figure 1**, with 72% of them using the reviews as the first step to finding an new provider **Figure 2**.


<figure class="half">
    <img src="/images/PhysicianReviews/figure1.png">
    <img src="/images/PhysicianReviews/figure2.png">
    <figcaption>Caption describing these two images.</figcaption>
</figure>
