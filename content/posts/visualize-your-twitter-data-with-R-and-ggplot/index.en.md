---
author: Cole
authorLink: https://colebaril.ca
categories:
- R
date: "2022-09-29T21:29:01+08:00"
description: A workflow for analyzing and visualizing your personal Twitter data. 
draft: false
images: []
lastmod: "2022-09-29T21:29:01+08:00"
lightgallery: true
resources:
- name: logo
  src: logo.png
tags:
- R
- RStudio
- Package
- Function
- Code
- Recipe
- Webscraping
title: Visualize your Twitter activity using R and ggplot2
toc: no
keep_md: yes
weight: 1
---

A hobby-project package containing functions to scrape recipes off the web. 

<!--more-->

## Premise <img src='logo-twitter-png-5860.png' align="right" height="139" />

Over the years I have tweeted *a lot*. One day, I was curious about my tweeting habits. 
What better way to put my newly-discovered interest in data science to use than to turn my 
Twitter data into a practice project? This analysis will walk you through the steps and provide 
a workflow to analyze your own Twitter data.

## Exporting your Twitter Data

To export your data from Twitter, you will need to request your Twitter data
from Twitter itself. To do so, login to your Twitter account on Desktop. On the left 
click More, then Settings then Your account. You should see a section called 
"Download an archive of your data". Click this and follow the instructions. 

For security reasons, it will take up to 24 hours to get your data back. When you
receive it, download and unpack the Zip folder. The folder will contain three files/folders:

1. assets folder
2. data folder - this is what we want!
3. "Your archive" - a HTML file containing your entire Twitter history. This file is good for searching for specific tweets if you need to find context.

Open the data folder and locate the JavaScript file called "tweet.js" (likely the largest file in the folder). 
For now, open this file in a simple text editor. Currently, the file is a JavaScript file, however, we want it 
to be a JSON (.json; JavaScript Object Notation) file so that we can read it into R. To convert it to a JSON file, simply strip the top line 
of code from the file: `window.YTD.tweet.part0 = [`. Once you do this, you can read the file into R! 

## Reading your Twitter Data into R

