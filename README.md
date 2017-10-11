# Ud-DataAnalyst-DataWrangling

## Project Overview
### Introduction
Real-world data rarely comes clean. Using Python, you will gather data from a variety of sources and in a variety of formats, assess its quality and tidiness, then clean it. This is called data wrangling. You will document your wrangling efforts in a Jupyter Notebook, plus showcase them through analyses and visualizations using Python and/or SQL.

The dataset that you'll be wrangling, analyzing, and visualizing is the tweet archive of Twitter user [@dog_rates](https://twitter.com/dog_rates) also known as [WeRateDogs](https://en.wikipedia.org/wiki/WeRateDogs).

WeRateDogs is a Twitter account that rates people's dogs with a humorous comment about the dog. It was started in 2015 by college student Matt Nelson, and has received international media coverage. As of October 2017, it has over 3.7 million followers. These ratings almost always have a denominator of 10. The numerators, though? Almost always greater than 10. 11/10, 12/10, 13/10, etc. Why? Because "[they're good dogs Brent](http://knowyourmeme.com/memes/theyre-good-dogs-brent)."

WeRateDogs gave Udacity exclusive access to their tweet archive for you to wrangle, analyze, and visualize. This archive contains basic tweet data for all 5000+ of their tweets. More on this soon.

![WeRateDogs Image via Boston Magazine](https://d17h27t6h515a5.cloudfront.net/topher/2017/October/59dd378f_dog-rates-social/dog-rates-social.jpg)
Image via [Boston Magazine](http://www.bostonmagazine.com/arts-entertainment/blog/2017/04/18/dog-rates-mit/)

### What Software Do I Need?
You need to be able to work in a Jupyter Notebook on your computer. Revisit our Jupyter Notebook and Anaconda tutorials earlier in the Nanodegree program for installation instructions.

The following libraries (also known as packages) are required to complete this project:

* pandas
* numpy
* requests
* tweepy
* json

You can install these packages via conda or pip. Please revisit our Anaconda tutorial earlier in the Nanodegree program for package installation instructions.

You also need to be able to create written documents with images that can be exported as PDF files. Example: [Google Docs](https://www.google.com/docs/about/), which is free.

A text editor, like [Sublime](https://www.sublimetext.com/) (which is free), will be useful but is not required.

## Project Motivation
### Goal
Your goal: wrangle WeRateDogs tweet data so you can create interesting analyses and visualizations that can be trusted. Yes, WeRateDogs gave Udacity exclusive access to their tweet archive, but it contains very basic information. Additional gathering, assessing, and cleaning is required for "Wow!"-worthy analyses and visualizations.

### Context
This archive contains basic tweet data for all 5000+ of their tweets, but not everything. One column the archive does contain though: each tweet's text, which I used to extract rating, dog name, and dog "stage" (i.e. doggo, floofer, pupper, and puppo).

![The extracted data from each tweet's text](https://d17h27t6h515a5.cloudfront.net/topher/2017/October/59dd4791_screenshot-2017-10-10-18.19.36/screenshot-2017-10-10-18.19.36.png)
The extracted data from each tweet's text

I extracted this data programmatically, but I didn't do a very good job. The ratings probably aren't all correct. Same goes for the dog names and probably dog stages too. You'll need to assess and clean these columns if you want to use them for analysis and visualization.

Back to the basic-ness of this dataset: retweet count and favorite count are a two of the notable column omissions. Fortunately, this additional data can be gathered from Twitter's API, but only because you have access to the tweet IDs in the tweet archive (without these IDs, the public only has access to the last ~3000 tweets via Twitter API). Guess what? You're going to gather this additional data.

One more cool thing: using a [neural network](https://classroom.udacity.com/nanodegrees/nd002/parts/20a33e0b-0f64-41a3-a9d9-c6f1207265c6/modules/14d9f5f1-9e7b-4bfb-97f3-bcdbf4a3699c/lessons/a8085857-3e28-4fc7-aeb8-da64ccbc2e20/concepts/28d4643b-3785-4700-bdee-4e5fc9963576) that can classify breeds of dogs*, I ran every image in the WeRateDogs Twitter archive. The results: a table full of image predictions alongside each tweet ID, image URL, and the image number that corresponded to the most confident prediction (1-4 since tweets can have four images).

![Tweet image prediction data](https://d17h27t6h515a5.cloudfront.net/topher/2017/October/59dd4d2c_screenshot-2017-10-10-18.43.41/screenshot-2017-10-10-18.43.41.png)

Tweet image prediction data

So for the last row in that table:

* tweet_id is the last part of the tweet URL after "status/" → **https://twitter.com/dog_rates/status/889531135344209921**
* p1 is the algorithm's #1 prediction for the image in the tweet → **golden retriever**
* p1_conf is how confident the algorithm is in its #1 prediction → **95%**
* p1_dog is whether or not the #1 prediction is a breed of dog → **TRUE**
* p2 is the algorithm's second most likely prediction → **Labrador retriever**
* p2_conf is how confident the algorithm is in its #2 prediction → **1%**
* p2_dog is whether or not the #2 prediction is a breed of dog → **TRUE**
* etc.

And the #1 prediction for the image in that tweet was spot on...

![A golden retriever named Stuart](https://d17h27t6h515a5.cloudfront.net/topher/2017/October/59dd4e05_dog-pred/dog-pred.png)

A golden retriever named Stuart

So that's all fun and good. But all of this additional data, after being gathered, will need to be assessed and cleaned. This is where you come in.

### Key Points
Key points to keep in mind when data wrangling for this project:

* We only want original ratings (no retweets) that have images.
* Fully assessing and cleaning the entire dataset requires exceptional effort so only a subset of its issues (eight quality issues and two tidiness issues at minimum) need to be assessed and cleaned.
* Cleaning includes merging according to the rules of [tidy data](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html) to facilitate analysis and visualization.
*Fun fact: creating this neural network is one of the projects in [Udacity's Artificial Intelligence Nanodegree program](https://www.udacity.com/course/artificial-intelligence-nanodegree--nd889).

## Project Details
* Data wrangling, which consists of:
  * Gathering data
  * Assessing data
  * Cleaning data
* Storing, analyzing, and visualizing your wrangled data
* Reporting on 1) your wrangling efforts and 2) your data analyses and visualizations

### Gathering Data for this Project
Gather each of the three pieces of data as described below in a Jupyter Notebook titled `wrangle_act.ipynb`:

1. The WeRateDogs Twitter archive. I am giving this file to you, so imagine it as a file on hand. Download this file manually by clicking the following link: `twitter_archive_enhanced.csv`

2. The tweet image predictions, i.e., what breed of dog or inanimate object is present in each tweet. This file (`image_predictions.tsv`) hosted on Udacity's servers and should be downloaded programmatically using the following URL: https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv

3. Each tweet's retweet count and favorite (i.e. "like") count at minimum, and any additional data you find interesting. Query Twitter's API (using the tweet IDs in the WeRateDogs Twitter archive) for each tweet's full set of data using Python's [Tweepy](http://www.tweepy.org/) library and store this data in a file called `tweet_json.txt` file, where each tweet's JSON data stored should be written to its own line. Then read this .txt file line by line into a pandas DataFrame with (at minimum) tweet ID, retweet count, and favorite count. Note: do not include your Twitter API keys and access tokens in your project submission.

### Assessing Data for this Project
After gathering each of the above pieces of data, assess it visually and programmatically for quality and tidiness issues. Detect and document at least **eight (8) quality issues** and **two (2) tidiness issues** in your Jupyter Notebook. To meet specifications, the issues that satisfy the Project Motivation (see previous page) must be assessed.

### Cleaning Data for this Project
Clean each of the issues you documented while assessing. The result should be a high quality and tidy master pandas DataFrame (or DataFrames if appropriate). Again, the issues that satisfy the Project Motivation must be cleaned.

### Storing, Analyzing, and Visualizing Data for this Project
Store the clean DataFrame(s) in a CSV file with the main one named `twitter_archive_master.csv`. If additional ones exist, name them appropriately. Additionally, you may store the cleaned data in a SQLite database (which is to be submitted as well if you do).

Analyze and visualize your wrangled data in your Jupyter Notebook. At least **three (3) insights and one (1) visualization** must be produced.

### Reporting for this Project
Create a 300-600 word written report called `wrangle_report.pdf` that briefly describes your wrangling efforts. This is to be framed as an internal document.

Create a >250 word written report called `act_report.pdf` that communicates the insights and displays the visualization(s) produced from your wrangled data. This is to be framed as an external document.

## Twitter API
### How to Query Twitter Data
In this project, you'll be using [Tweepy](http://www.tweepy.org/) to query Twitter's API for additional data beyond the data included in the WeRateDogs Twitter archive. This additional data will include retweet count and favorite count.

Tweet data is stored in JSON format by Twitter.

Getting tweet JSON data via tweet ID using Tweepy is described well in this [StackOverflow answer](https://stackoverflow.com/questions/28384588/twitter-api-get-tweets-with-specific-id).

Before you can run your API querying code, you need to set up your own Twitter application. And before that, you must sign up for a Twitter account. [This guide](https://www.slickremix.com/docs/how-to-get-api-keys-and-tokens-for-twitter/) describes the setup process well. (Note: if you can't set up an application because of mobile verification issues, email david.venturi@udacity.com and I will help you.)

### Do Not Include Your API Keys and Access Tokens in Your Submission
Do not include your API keys and access tokens in your project submission. This is standard practice for APIs and public code.

### Twitter's Rate Limit
Twitter's API has a rate limit. Rate limiting is used to control the rate of traffic sent or received by a network interface controller and is used to prevent DoS attacks.

As per Twitter's [rate limiting](https://developer.twitter.com/en/docs/basics/rate-limiting) page:

> Rate limits are divided into 15 minute intervals

To query all of the tweet IDs in the WeRateDogs Twitter archive, it took me 20-30 minutes usually. Printing out each tweet ID after it was queried as well as [using a timer](https://stackoverflow.com/questions/7370801/measure-time-elapsed-in-python) was helpful for sanity reasons.

## Project Submission
### Wrangle and Analyze Data
In this project, you'll gather, assess, and clean data then act on it through analysis, visualization and/or modeling.

### Submission
1. Ensure you pass all points on [the rubric](https://review.udacity.com/#!/rubrics/1136/view).
2. Ensure you haven't included your API keys and access tokens in your project files.
3. Package the following files into a zip archive, or push them from a GitHub repo:
 1. wrangle_act.ipynb
 2. wrangle_report.pdf
 3. act_report.pdf
 4. twitter_archive_enhanced.csv
 5. image_predictions.tsv
 6. tweet_json.txt
 7. twitter_archive_master.csv
