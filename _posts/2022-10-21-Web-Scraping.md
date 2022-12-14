---
layout: post
title:  "Netflix company’s stock analysis: Using the Yahoo Finance API"
date:   2022-10-21
author: Katsuhiko Maeda
description: In this blog post, you will learn how to scrape and clean stock price data from Yahoo Finance with a quick tutorial. 
image: /assets/images/header_netflix_pic.png
---
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Top_pic_stock.jpg" alt="" style="width:800px;">

# Introduction
Netflix’s growth has been rapid over the last 10 years, and has become one of the most successful subscription companies in the world. Their business continued to grow during COVID-19 as many people spent the majority of their time at home. However, many investors have questioned the company’s lasting strength and stability. Three major threats to Netflix’s market share include their account sharing problems, the entry of Disney+, and the significant drop in subscribers after viewers returned to normal activities.
 
I decided to use this web scraping project as an opportunity to assess how the company has been performing based on its historical stock prices.
 
# How do I measure company performance?
There are many financial metrics to evaluate a company’s performance such as the number of subscribers, net worth, and stock prices. Stock prices in particular change based on a variety of factors, but stock prices of public companies typically rise over time as they become more financially successful. If you check the stock prices of most successful public companies like Amazon and Apple, you will find their stock prices have increased significantly compared to the time of IPO, Initial Public Offering.
 
# Data collection and cleaning
## Step 1: Ethical and legal considerations
For the first step, we need to check if scraping data off the website we am going to use is legal. The website I acquired Netflix’s stock prices from is called Yahoo Finance. They provide information like real-time market data and stock quotes for public companies. In my case, I determined that web scraping in the website is both ethical and legal for two reasons. First, the stock information on the website is open-source and publicly available. They also let you download a csv file of stock prices in your desired range of dates. Second, I was not able to locate information in their terms and conditions on whether or not scraping data is prohibited.

<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/yahoo_finance.png" alt="" style="width:300px;"/>

## Step2: Data collection
For data collection, I used requests.get and pd.read_html() functions. After downloading pandas, you will be able to read tables from a website by passing the URL to the read_html function.

```    
import pandas as pd</div>
<div>import requests</div>
<div>url_link='https://finance.yahoo.com/quote/NFLX/history?period1=1350777600&period2=1666310400&interval=1wk&filter=history&frequency=1wk&includeAdjustedClose=true'</div>
<div>r = requests.get(url_link,headers = {'User-Agent':'Mozilla/5.0'})</div>
<div>netflix_stock = pd.read_html(r.text)[0]</div>
<div>netflix_stock = netflix_stock.drop(netflix_stock.index[[100, 201, 302, 382, 403, 504, 528]])
```

<p>What is tricky about getting stock data from the Yahoo Finance website is that it does not allow us to retrieve the whole chart at one time. Instead, I had to adjust the data range to get a comprehensive list of stock quotes from a different webpage.
For this reason, I had to change the URL slightly each time I scraped a part of the whole list from the website. Luckily, the only part of the URL that I needed to change were the numbers highlighted below. However, the numbers were not in chronological order so I needed to manually type in the specific numbers that corresponded to the web page. For example, some of the URL number sequences would change by 60220800, 59875200 or 60480000. Everything else stays the same for each URL.</p>

<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/Page_URL_num.png" alt="" style="width:400px;"/>

Below is an excerpt of the for-loop I used to get data from each page. I specified the value of the number decrease from page to page and combined it with the rest of the URL. Then, I used the append function to add the new data to my dataframe by running the last line of code below:


```
url_bef_num = 'https://finance.yahoo.com/quote/NFLX/history?period1=1350777600&period2='</div>
url_aft_num = '&interval=1wk&filter=history&frequency=1wk&includeAdjustedClose=true'</div>
num = 1666310400</div>

for i in range(2,7):</div>
    # For the second page of stock quotes, the number before '&interval' in the URL increases by 60220800
    if i == 2:
        num = num - 60220800
        print("i is 2")
    # For the fifth page of stock quotes, the number before '&interval' in the URL increases by 59875200
    elif i == 5:
        num = num - 59875200
        print("i is 5")
    # For the other n-th page of stock quotes, the number before '&interval' in the URL increases by 60480000
    else:
        num = num - 60480000
        print("i is else")

    url_link = url_bef_num + str(num) + url_aft_num
    r = requests.get(url_link,headers = {'User-Agent':'Mozilla/5.0'})
    print(url_link)
    netflix_stock = netflix_stock.append(pd.read_html(r.text)[0], ignore_index=True)
```
 
Below are the first five entries of the resulting table:

<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/dataframe_head.png" alt="" style="width:400px;"/>
 
## Step 3: Data cleaning process
Fortunately, the table data is very organized and does not have to be modified much for the next data analysis process. Below are the two steps that I went through to prepare the scraped data to be ready for stock analysis.

**Step 3-1: Remove irrelevant rows**
<p>Irrelevant rows were added to the last row of netflix_stock dataframe everytime I attached new stock data in the for-loop. I removed them by referring to the index numbers:</p>

```
netflix_stock = netflix_stock.drop(netflix_stock.index[[100, 201, 302, 382, 403, 504, 528]])
netflix_stock = netflix_stock.drop(netflix_stock.index[[100, 201, 302, 382, 403, 504, 528]])
```

**Step 3-2: Change date format for analysis purpose**
<p>I modified the original date format to the following format and set the date column as an index. This helps us when you want to visualize movement of the stock prices in graphs.</p>

```
netflix_stock["Date2"] = [dt.strptime(i, "%b %d,  %Y") for i in netflix_stock["Date"]]
netflix_stock.set_index("Date2",inplace=True)
```
<img src="https://github.com/Kattsun2525/stat386-projects/raw/main/assets/images/date_column.png" alt="" style="width:100px;"/>
<br>(New data format)

# Conclusion
In this post, I explained how you can scrape Netflix’s stock data from Yahoo Finance and clean the data to analyze its stock performance. I am excited to conduct analysis on the cleaned data and tell you what we can say from the stock data in the next blog. Meanwhile, feel free to leave your comments below if you have any questions or suggestions.
 
 
If you are interested in seeing the full code and dataset, click [here](https://github.com/Kattsun2525/Web_scraping_project) to get access to the repository.

