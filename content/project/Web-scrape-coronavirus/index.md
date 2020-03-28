---
date: "2020-03-28"
external_link: ""
image:
  caption: 
  focal_point:
links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/asvn90
slides: 
summary: How to web scrape data from a website.
tags:
- Python
- Twitter
- COVID-19
- Coronavirus
title: Web scraping for daily COVID-19 stats
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---
{{% toc %}}

## Introduction

As part of my aim to learn Python, I decided to teach myself by completing a bunch of projects created in Python. I started off by creating a [Twitter bot](https://www.adriaansblog.com/project/twitter-bot-project/), and now I want to expand it to web scraping daily stats of a website and tweeting about it.
I am specifically interested in the daily stats of the COVID-19 virus (Coronavirus) and what the daily new cases are as well as the global growth factor. The idea to look at the daily stats of the Coronavirus was sparked after watching a video by [3Blue1Brown](https://www.youtube.com/watch?v=Kas0tIxDvrg&t) where he discusses exponential growth of epidemics.

**Right! Let's get started!**

## What is web scraping?

**Web scraping**, **web harvesting**, or **web data extraction** is used for extracting all kinds of data from websites. Web scraping can be done manually by the user, but web scraping typically includes automated processes implemented using a bot or web crawler. It is a form of copying, in which specific data is gathered and copied from the internet, typically into a database or spreadsheet, for later retrieval or analysis.

## The data source

We are interested in the data contained in a table at [Worldometer's](https://www.worldometers.info/coronavirus/#countries) website, where it lists all the countries together with their current reported coronavirus cases, new cases for the day, total deaths, new deaths for the day, etc. The idea is to get these daily figures, calculate the global growth factor and create a tweet that can be tweeted daily reporting the stats of:

- United Kingdom and
- South Africa.

## Coding the web scraper

### Get the HTML data

First thing you need to do is to get the HTML data of the site you are interested in, into your Python script. We can do this using the Python library, `requests`. You can install it using

    pip install requests
    
Once it is installed you can import it to your Python script:

    import requests
    
    URL = 'https://www.worldometers.info/coronavirus/#countries'
    page = requests(get.URL)
    
The code retrieves the HTML data that the server sends back and stores that data in a Python object.

We have scraped some HTML from this webpage, but when you look at it, it just seems like a huge mess. There are tons of HTML elements and thousands of attributes scattered around. We can now parse this lengthy code response with **Beautiful Soup** to make it more accessible and pick out the data that we are interested in.
    
## What is BeautifulSoup

Beautiful Soup is a Python library used for pulling data out of HTML and XML files. It  provides ways of navigating, searching, and modifying the parse tree of a website. It can save programmers hours or days of work when gatering data. 

### Installing BeautifulSoup

If you have not done so already, you need to install `BeautifulSoup` which is the Python library that you will use to scrape data from the  webpage. You can install it using the command prompt:

    pip install beautifulsoup4
    
Once that is done, we can start using it in our Python script.

    from bs4 import BeautifulSoup
    
    soup = BeautifulSoup(page.content, 'html.parser

## Extracting the data

### Find table ID

The Beautiful Soup object has been created in our Python script and the HTML data of the website has been scraped off of the page. Next we need to get the data that we are interested in, out of the HTML code. We first find the `id attribute` of the table by using the **inspect** fuctionality of the Chrome web browser. You right-click anywhere on the webpage and at the bottom of the dropdown list that appears, you select *inspect*.

The ID of the table is `main_table_countries_today`. We can use this to focus only on the table in the scraped HTML code:

    results = soup.find(id='main_table_countries_today')
    
>With a website like this, it is possible that the structure of the website can change in the future, and this can include changing the `id attribute` of the table which can cause an error in your code. You will just have to go back to the webpage and update your python code with the new table ID.

### Cleaning up the scraped data

If we go and print out the data contained in `results`, we get:

```cmd

<tr style="">
<td style="font-weight: bold; font-size:15px; text-align:left;"><a class="mt_a" href="country/uk/">UK</a></td>
<td style="font-weight: bold; text-align:right">14,543</td>
<td style="font-weight: bold; text-align:right;"></td>
<td style="font-weight: bold; text-align:right;">759 </td>
<td style="font-weight: bold; text-align:right;"></td>
<td style="font-weight: bold; text-align:right">135</td>
<td style="text-align:right;font-weight:bold;">13,649</td>
<td style="font-weight: bold; text-align:right">163</td>
<td style="font-weight: bold; text-align:right">214</td>
<td style="font-weight: bold; text-align:right">11</td>
<td style="text-align:right;font-size:13px;">
Jan 30 </td>
</tr>
```
The data is the entry for the **United Kingdom**, but there are still a lot of HTML code that we do not want. All the data entries of the table for the given country is wrapped in the HTML element `<td ... <\td>`. We can use that knowledge to further clean up the scraped data:

    content = results.find_all('td')

If we print out the data contained in `content` we get:

```cmd
<td style="font-weight: bold; font-size:15px; text-align:left;"><a class="mt_a" href="country/uk/">UK</a></td>, <td style="font-weight: bold; text-align:right">14,543</td>, <td style="font-weight: bold; text-align:right;"></td>, <td style="font-weight: bold; text-align:right;">759 </td>, <td style="font-weight: bold; text-align:right;"></td>, <td style="font-weight: bold; text-align:right">135</td>, <td style="text-align:right;font-weight:bold;">13,649</td>, <td style="font-weight: bold; text-align:right">163</td>, <td style="font-weight: bold; text-align:right">214</td>, <td style="font-weight: bold; text-align:right">11</td>, <td style="text-align:right;font-size:13px;">
```
This is still every chaotic. Luckily all the data that we need, it text and can be extracted from the above as:

    for data in content:
      print(data.text.strip())

This will contain the following data:
    
```cmd
UK
14,543

759

135
13,649
163
214
11
```

Each line above is a column entry for the **United Kingdom**. Next, this data needs to be entered into a dictionary so that we can use it to contruct a tweet.

### Creating a `dict` variable

#### Creating seperate lists

Before we can create a tweet reporting on the daily stats, we need to save the scraped data in some form that can be used effectively. For this project, all the data will be safed in a Python dictionary. To achieve this we will:

- Save each column in a list and
- Create a dictionary with all the populated lists.

First, we initialise empty lists for each column:

    countries = []
    total_cases = []
    new_cases = []
    total_deaths = []
    new_deaths = []
    total_recovered = []
    active_cases = []
    critical = []
    total_per_mil_pop = []

Then we iterate through the `content` variable and place each corresponding entry into the correct list. There are 10 columns and as such a new country's data is shown after every 10 iterations:

    i = 1
    for data in content:
        if i%10 == 1:
            countries.append(data.text.strip())
        if i%10 == 2:
            total_cases.append(data.text.strip())
        if i%10 == 3:
            new_cases.append(data.text.strip())
        if i%10 == 4:
            total_deaths.append(data.text.strip())
        if i%10 == 5:
            new_deaths.append(data.text.strip())
        if i%10 == 6:
            total_recovered.append(data.text.strip())
        if i%10 == 7:
            active_cases.append(data.text.strip())
        if i%10 == 8:
            critical.append(data.text.strip())
        if i%10 == 0:
            total_per_mil_pop.append(data.text.strip())
        i += 1

If we assume the first column on the left (*Country*) can be numbered as 1, then the mathematical operater `%10` returns the modulus which corresponds to each column. This can then be used to ensure the correct data is appended to the correct list.

#### Combining lists into a dictionary

After all the lists have been populated with the data scraped from the webpage, we can combine it into a dictionary:

    covid19_table = {
        "columns": column_names,
        "country": countries,
        "total_cases": total_cases,
        "new_cases": new_cases,
        "total_deaths": total_deaths,
        "new_deaths": new_deaths,
        "total_recovered": total_recovered,
        "active_cases": active_cases,
        "critical": critical,
        "total_1M_ pop": total_per_mil_pop}

The `column_names` have been generated seperately:

    column_names = ["Country", 
        "Total Cases", 
        "New Cases", 
        "Total Deaths", 
        "New Deaths", 
        "Total Recovered", 
        "Active Cases", 
        "Serious/Critical", 
        "Total Cases/1M pop"]
        
We are now ready to start using this data to contruct a tweet that will be sent out daily.

## Compiling the tweet

### Growth factor

In order to calculate the growth factor of the coronavirus, the following equation can be used:

$$Gf = \\frac{N_i}{N_{i-1}}$$

where $N_i$ is the amount of new cases for today and $N_{i-1}$ refers to the amount of new cases for the previous day.

A .csv file will be used to store each day's stats so that a new growth factor can be calculated each day:

```cmd
Date,Total_cases,New_cases,Growth_factor
2020-03-11,"126,007","7,059",0.0
2020-03-12,"134,098","7,899",1.12
2020-03-13,"145,336","10,759",1.36
```
A Python libary `pandas` will be used to read from, and write to the .csv file. You can install `pandas` using:

```cmd
pip install pandas
```

The following was added to the python script to read the current content of the .csv file and put in a dictionary `new_table`:

    import pandas
    
    df = pandas.read_csv('Total.csv', parse_dates = ["Date"], dayfirst = True)
    new_table = df.to_dict()

The current date is added to the `new_table` dictionary as it is the first entry required for the .csv file:

    today = str(date.today())
    new_table["Date"][len(new_table["Date"])] = today

Now we calculate the growth factor. We first search for the position in the *country* label of our `covid19_table` dictionary for the total numbers for the whole world for the current day. This is under the *country * entry *Total:*.

    search_position = covid19_table["country"].index("Total:")

Next, we retrieve yesterday's data from the `new_table` dictionary which was created from the .csv file:

    growth_yesterday = new_table["New_cases"][len(new_table["New_cases"])-1]
    if type(growth_yesterday) == str:
            growth_yesterday = new_table["New_cases"][len(new_table["New_cases"])-1].replace(',','')

The amount of new cases globally is retrieved from the `covid19_table` that we created from our scraped data using the `search_position` which indicates where the corresponding data is in the dictionary.

    growth_today = covid19_table["new_cases"][search_position].replace(',','')
    
The growth factor can now be calculated:

    Gf = round(float(growth_today)/float(growth_yesterday),2)

The data needed for the current day's entry for the .csv file is then added to the `new_table` dictionary:

    new_table["Total_cases"][len(new_table["Total_cases"])] = covid19_table["total_cases"][search_position]
    new_table["New_cases"][len(new_table["New_cases"])] = covid19_table["new_cases"][search_position]
    new_table["Growth_factor"][len(new_table["Growth_factor"])] = str(Gf)

Lastly, we convert the `new_table` dictionary back to a .csv file that can be used the next day again:

    df = pandas.DataFrame.from_dict(new_table)
    df.to_csv("Total.csv",index=False)

### Getting stats for the tweet

We create a dictionary that can be returned when the method is called with all the necessary data required for the tweet:

    position_UK = covid19_table["country"].index("UK")
    position_RSA = covid19_table["country"].index("South Africa")
    new_UK = covid19_table["new_cases"][position_UK]
    new_RSA = covid19_table["new_cases"][position_RSA]
    new_total = covid19_table["new_cases"][search_position].replace(',','')
    total_UK = covid19_table["total_cases"][position_UK]
    total_RSA = covid19_table["total_cases"][position_RSA]
    total_total =     covid19_table["total_cases"][search_position].replace(',','')

    tweet_data = {
        "UK": {
        "Total": total_UK,
        "New": new_UK
        },
        "RSA": {
        "Total":total_RSA,
        "New":new_RSA
        },
        "Total": {
        "Total": total_total,
        "New": new_total
        },
        "Gf": Gf
    }

The `tweet_data` dictionary can now be used to construct the tweet.

### Constructing the tweet

The best way to create a multi-line tweet is to use a .txt file that can be fed to the twitter API. The content of the .txt file for the tweet was written together with the data in `tweet_data`:

    with open('tweet.txt', 'w',encoding='utf-8') as f:
       f.write('#COVID19 stats '+str(date.today())+':\n\
       \nTotal cases for:\nUK: '+str(tweet_data["UK"]["Total"])+' ('+str(tweet_data["UK"]["New"])+')'+'\
       \nRSA: '+str(tweet_data["RSA"]["Total"])+' ('+str(tweet_data["RSA"]["New"])+')'+'\
       \nOverall: '+str(tweet_data["Total"]["Total"])+' (+'+str(tweet_data["Total"]["New"])+')'+'\
       \n\nGrowth factor: '+str(tweet_data["Gf"])+'\
       \n\n#CoronaVirusSA \n#CoronaVirusUK')
       
The last thing left is to tweet it!

    with open('tweet.txt','r') as f:
       api.update_status(f.read())

<div style="text-align: center;">    
<blockquote class="twitter-tweet"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/COVID19?src=hash&amp;ref_src=twsrc%5Etfw">#COVID19</a> stats 2020-03-27:<br> <br>Total cases for:<br>UK: 14,543 (+2,885) <br>RSA: 1,170 (+243) <br>Overall: 590628 (+58818) <br><br>Growth factor: 1.0 <a href="https://twitter.com/hashtag/CoronaVirusSA?src=hash&amp;ref_src=twsrc%5Etfw">#CoronaVirusSA</a> <a href="https://twitter.com/hashtag/CoronaVirusUK?src=hash&amp;ref_src=twsrc%5Etfw">#CoronaVirusUK</a></p>&mdash; COVID-19 stats tracker (@nog_nuus) <a href="https://twitter.com/nog_nuus/status/1243658693239017484?ref_src=twsrc%5Etfw">March 27, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>