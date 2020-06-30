---
date: "2020-04-06"
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
summary: How to web scrape data from Facebook.
tags:
- Python
- Facebook
- Promotional posts
title: Web scraping Facebook
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---
{{% toc %}}

## Introduction

This coding project is building on [my first web scraping project](https://www.adriaansblog.com/project/web-scrape-coronavirus/) and uses some of the code I developed in that project to get the data from Facebook.
I am sure you have seen posts from a company's or small business's Facebook page where they ask you to like, share and follow in order to win a prize. 
But, how do these businesses then determine who have won? They will have to go and compare the names of all the people who:

- liked the post,
- shared the post and
- followed their page

and then randomly select a winner from that group of people. If there are about 10 people who engaged with that promotional post, then it won't take too long, but imagine if hundreds of people participated? It will take quite a lot of time to determine the winner then...

To help save a bit of time, we are going to construct a program that will scrape all the names of the people who engaged with the promotion post and randomly select a winner from the eligible participants.

**Right! Let's get started!**

## The data source

We are interested in the names of people who follow the business's page and who liked and shared the post in question. All of this information is available on Facebook. In the end we will have three lists:

- a list containing the names of people who follow the business's Facebook page,
- a list of people who have liked the promotional post and
- a list of people who have shared the post.

The names that appear in all three lists, will be put in a final list and a random winner will be selected from the list of eligible names.

## Coding the web scraper

### Requirements

For this program to work, you will need to install:

- Python 3.7
- Requests library
- Beautifulsoup library
- Selenium library

### Get the HTML data

First thing we need to do is to get the HTML data of the Facebook post we are interested in, into our Python script. We can do this using the Python library, `requests`. 

Web scraping Facebook is a bit different, as the content is behind a login page. We will need to first have a look at the HTML code of the login page. For this project, we are going to log into https://mbasic.facebook.com:

- start working your way through the HTML until you find the `<form>` HTML tag.
- within the `<form>` tag look for the `method="post"` argument.
- go down further into the form tag and look for the `<input>` tags. There should be at least two (`username` and `password`). The username input tag is generally of `type=email` and the password, `type=password`.
- look within these input tags for a name argument. This is the name of this input field. This is also how requests is going to know where to “enter” your credentials. For this it is `name="email"` and `name="pass"`.

#### Log into Facebook

The following code can be used to log into your Facebook account in order to scrape the necessary data that we need:


    LOGIN_URL = 'https://www.facebook.com/login'

    payload = {
      'email': 'your username',
      'pass': 'your password'
    }

    with requests.Session() as session:
      post = session.post(LOGIN_URL, data=payload)

#### Scrape the data
The URL is standard for most posts. You will need the following:

- ID of post
- amount of people who engaged with the post

With the two variables known, the URL where we will scrape the data is:

    REQUEST_URL = f'https://mbasic.facebook.com/ufi/reaction/profile/
    browser/fetch/?limit={limit}&total_count=17&ft_ent_identifier={post_ID}'
Now we can scrape the data by adding the following line:

    with requests.Session() as session:
      post = session.post(POST_LOGIN_URL, data=payload)
      
      r = session.get(REQUEST_URL) #Add this line
      #^^^ Add this line... ^^^
      
### Zooming in

With the HTML data stored in the variable `r` we can get the names of the people who liked the post with the following:

    soup = BeautifulSoup(r.content, "html.parser")
    names = soup.find_all('h3', class_='be')
    people_who_liked = []
    for name in names:
      people_who_liked.append(name.text)

The list `people_who_liked` has been created can will be used later.

## People who shared

The same can be done for getting the list of people who shared the post, with a few changes to the code for `BeautifulSoup`:

    with requests.Session() as session:
      post = session.post(POST_LOGIN_URL, data=payload)
      r = session.get(REQUEST_URL)
      
    soup = BeautifulSoup(r.content, "html.parser")
    names = soup.find_all('span')
    
    people_who_shared = []
    for name in names:
      people_who_shared.append(name.text)

## People who follows your page

This one is a bit more tricky. We can only access the list of people who follows a page, if you have administrator rights to that pages. Also, the list is not shown when you access it through the website address https://mbasic.facebook.com and https://m.facebook.com and we will thus need another approach to get the HTML data through the use of the Selenium library:

- Log in to Facebook
- Navigate to page settings listing the people who liked our page
- Scroll down until all the names are displayed on the screen
- Scrape the list of names

#### Load the page of names

Selenium can take control of the browser and log in:

    from selenium import webdriver
    from selenium.webdriver.common.keys import Keys
    from selenium.webdriver.chrome.options import Options

    webdriver.driver.get("https://www.facebook.com/login")

    sleep(2)

    email_in = webdriver.driver.find_element_by_xpath('//*[@id="email"]')
    email_in.send_keys(username)

    password_in = webdriver.driver.find_element_by_xpath('//*[@id="pass"]')
    password_in.send_keys(password)

    login_btn = webdriver.driver.find_element_by_xpath('//*[@id="loginbutton"]')
    login_btn.click()

The `sleep()` command gives the page time to load before entering the login details. Next we navigate to the page settings listing all the names:

    page_name = your-page-name
    REQUEST_URL = f'https://www.facebook.com/{page_name}/settings/
    ?tab=people_and_other_pages&ref=page_edit'

    webdriver.driver.get(REQUEST_URL)

    sleep(2)
    scrolls = 15
    
    for i in range(1,scrolls):
      webdriver.driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
      sleep(3)

      page = webdriver.driver.page_source
      soup = BeautifulSoup(page, "html.parser")
      names = soup.find_all('a', class_='_3cb8')
      people_who_liked_page = []
      for name in names:
        people_who_liked_page.append(name.text)

For the above code we assume that 15 page scrolls will show all the names of the people who follow the page, but you can increase it depending on the amount of people who follow your page.

## Determine the winner

We now have three lists:

- People who liked the page
- People who liked the post
- People who shared the post

We need to create a new list that has all the names of the people who appear in all three of the above lists:

    eligible_to_win = []
    for name in list_A:
      if name in list_B and name in list_C:
        eligible_to_win.append(name)
          
The last thing to do is to select a winner randomly:

    winner = random.choice(eligible)
    
You can also select more than one winner:

    winners = random.sample(items, n)

where `n` is the amount of winners you want to select.

## Conclusion

This code will ensure that you can select a winner to the promotional post in only a few minutes, rather than doing it manually. 