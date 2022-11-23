---
layout: post
title:  "Exploring Netflix stock data over the past decade"
date:   2022-11-22
author: Katsuhiko Maeda
description: This blog shows how to conduct EDA on the Netflix’s stock data I gathered in my previous blog post.
image: /assets/images/header_netflix_pic.png
---
In my previous post, I explained how I gathered all the weekly Netflix stock quotes from October 22nd in 2012 to today using Yahoo Finance API and cleaned the data for following analysis procedures. Please follow this [link](https://kattsun2525.github.io/stat386-projects/2022/10/21/Web-Scraping.html) if you are interested in the tutorial. 

In this blog, we start exploring the data to observe if it exhibits any patterns and abnormalities. As a quick reminder, the goal of this stock data analysis project is to understand how Netflix has performed in the past based on its historical stock quotes. Despite the fact the company has seen a tremendous growth in the last 10 years, the company has struggled due to external factors such as declining leisure time at home after COVID-19 and internal problems like account sharing issues. I am looking forward to helping you understand Netflix’s stock situation based on my EDA in this blog.

 
# Quick recap of Netflix dataset
We have downloaded the daily stock prices data using the Yahoo finance API functionality. The dataset includes the following variables:
- Open: The price of the stock when the market opened
- High: Highest stock price traded during that day
- Low: Lowest stock price traded on that day
- Close: The price of the stock when the market closed 
- Volume: The total amount of stocks traded on that day
- Date (index column) : Stock trade date from October 22th in 2012 to October 17th in 2022.
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Picture0.png" alt="" style="width:800px;"/>

# Exploratory Data Analysis
The below is a basic summary of the dataset you can get by using describe() function:
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Picture1.png" alt="" style="width:800px;"/>

While this summary chart is helpful for us to get a general sense of Netflix stock data on the entire timeframe, it is not very informative as the behavior of the data changes from time to time depending on the company performance and its surrounding environments. So, we will have to dissect data in certain periods of time to draw specific insights.
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Picture2.png" alt="" style="width:800px;"/>

This stock price history chart shows that the stock price has steadily increased for almost 10 years since October in 2012, although it fluctuated quite a bit in 2018 and 2019. Their stock price even increased by roughly $300 from around March 2020 to November 2021 and peaked at $700. However, there was a steep decline in the stock quote soon after. 

Now I would like to zoom in and get a more detailed look of where the stock price started seeing a decline. 
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Picture3.png" alt="" style="width:800px;"/>

The stock was priced at around $700 in November 2021 but dropped to $200 (-71%) in a matter of six months. One of the largest factors that contributed to this steep decline would be that a substantial amount of people returned to pre-pandemic life and cancelled their Netflix membership because they had much less leisure time at home. 
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Picture4.png" alt="" style="width:800px;"/>

In this graph, you can tell that many investors most likely sold their Netflix stocks following the sudden drop. The fact that a large volume of stocks was traded when the stock price was dropping significantly is a clear sign that the investors were not optimistic. Furthermore, the increased selling activity likely contributed to even lower prices.

One might say that investors were too optimistic about Netflix’s future prospect during the pandemic and the stock price inflated as a result, but the drop was probably a lot steeper than many have speculated as it is now lower than the pre-pandemic price.


Next, I am going to find out the distribution of the percent changes in stock price compared to the previous week. The code I used to create the chart is as follows:

I simply divided the stock closing price by the previous one and subtracted the result by 1. So, if you have -0.2, that means the week’s stock closing price decreased by 20% from the previous week.

<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Picture5.png" alt="" style="width:800px;"/>
The histogram looks normal around 0.0 which means no change. However, we need to take into account that even a small percentage change could result in a big price difference. So, if the previous price is a very large number, the change is huge. For example, on July 13th, the percent change was -10% compared to the previous week, but the total change was almost $55 in a day because the previous closing price was $548.
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Picture6.png" alt="" style="width:800px;"/>

# Conclusion
In this blog, we have explored the Netflix stock price data to gain an understanding of the data and get insights into how we can proceed to the following analyses. Netflix has seen a tremendous growth, especially during COVID-19 but has been struggling following the pandemic. The potential causes of the declining stock price would be more increased outdoor  activities and threat of new entrants like Disney+. We also saw that investors reacted to the stock price negatively in the volume analysis.

As a next step, I will conduct more of qualitative analyses to understand the reasons behind  the changes in the stock prices and see how they compare against their competitors. I am also going to search for methods to predict future stock prices. Stay tuned for my next blog post.

Please let me know if you have any questions or thoughts on this blog content. I would really appreciate any feedback.

Here is my [repository](https://github.com/Kattsun2525/Web_scraping_project) for this exploratory data analysis.
