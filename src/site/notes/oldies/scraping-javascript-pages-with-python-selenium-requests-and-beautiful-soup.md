---
dg-show-inline-title: false
dg-publish: true
dg-home: false
dg-show-toc: false
dg-created: 12/03/2015
dg-updated: 
dg-show-tags: false
dg-link-preview: false
---

Title: Scraping JavaScript pages with Python: Selenium, Requests, and Beautiful Soup
Date: 2015-12-03 23:08
Author: Michael
Category: Python 
Tags: BeautifulSoup, Javascript, Requests, Scraping, Selenium
Slug: scraping-javascript-pages-with-python-selenium
Status: published

**Note: Imgur has an API. Please use the API if you intend to gather
images from Imgur.**

This post is a short demonstration of scraping JavaScript webpages that
are not possible to scrape with [Beautiful
Soup](http://www.crummy.com/software/BeautifulSoup/) or
[Requests](http://docs.python-requests.org/en/latest/) by themselves.
 While imgur has an API, the thought occurred that perhaps there are
many, many sites that do not have an API that need a good scraping. If
that is the case, how do you scrape them?
[Selenium](http://selenium-python.readthedocs.org/installation.html).
Lets dive in and I will show you:

First things first, import the modules that will be used. You will need
to install these I prefer to use the
[Anaconda](https://www.continuum.io/downloads) Python distribution. The
managing of Python environments is not in the scope of this demo, so I
will leave the management of that to you. Suffice to say that you can
install requests and beautiful soup with conda. Selenium can be created
as a package then uploaded to Anaconda Cloud with Binstar and... well,
you can simply install Selenium with pip.

    :::python
    import os
    from selenium import webdriver
    from bs4 import BeautifulSoup
    from requests import get
    

Next, define where you want to store the images. This will store the
image in the present working directory or the directory where the script
lives.

    :::python
    # Make a directory in the pwd, if one already exists. Cool.
    os.makedirs('imgur', exist_ok=True) # Make directory to store the images
    

Now we can instantiate the Selenium webdriver, and pass a url to fetch.
I used firefox because it seems like it is very well supported. I didn't
even try with Chrome which is my browser of choice.

    :::python
    # Imgur is not possible to scrape without Selenium. Because Javascript.
    browser = webdriver.Firefox() # Instantiate a webdriver object
    browser.get('http://imgur.com') # Go to Imgur
    

This is where your investigation begins. First, define an empty list to
hold the links of the pages. Turns out that imgur renders the home page
with thumbnails of images. The links to the pages where the main image
src resides is within the class image-list-link. Every website is
different and rendered JavaScript pages can use wildly different naming
conventions. Thus the need to dig with your browser before you start
coding.  To investigate with a OSX, tune your browser (chrome) to the
url you wish to scrape and press: option | command | j  Then start
inspecting elements.

    :::python
    # Makes list of links to get full image
    links = []
    # This is the container of images on the main page
    cards = browser.find_elements_by_class_name('image-list-link')
    for img_src in cards:
        # Now assemble list to pass to requests and beautifulsoup
        links.append(img_src.get_attribute('href'))
    

The rest of the story. This bit of code loops through the links list
created in the previous snippet. We call requests.get() to url stored in
links list. Then we pass the html from requests to beautiful soup where
we can dominate it with reckless abandon. We ask soup to select the
post-image class img tag. Then we check if it is empty. If the list is
not empty, we assign the actual src of the imageLink to imageUrl. This
variable is prefixed with http: and sent back to requests for retrieval.
 If the url is mangled or mistyped we handle the exception and move to
the next one. If not, we write the file to directory and using requests
iter_chunks method in 100,000 byte increments. Close the image file and
start the next round or finish the script.

    :::python
    # loop through the links list (I'm slicing to 5)
    for page in links[:5]:
        res = requests.get(page)
        res.raise_for_status()
        soup = BeautifulSoup(res.text, 'html.parser')
        imageLink = soup.select('.post-image img')
        if imageLink == []:
            print('Nothing here...')
        else:
            try:
                # assign imageUrl hold the actual file name of the image
                imageUrl = imageLink[0].get('src')
                #Download the image
                print('Downloading image %s...' %(imageUrl))
                res = requests.get('http:'+imageUrl)
                res.raise_for_status()
            except requests.exceptions.MissingSchema:
                continue
            imageFile = open(os.path.join('imgur', os.path.basename(imageUrl)), 'wb')
            for chunk in res.iter_content(100000):
                imageFile.write(chunk)
            imageFile.close()
   

 
