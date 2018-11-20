---
layout : post
title : Introduction to NLP Analyzing NY Times Topics
---

This is a presentation I gave for prospective students at Flatiron School. It was meant as an engaging introductory example that demonstrated some of the potentially cool and interesting insights students could draw after only a month or two of intesive work at the bootcamp. As such, this talk covered a wide variety of topics from the basic data science workflow, APIs, http requests, natural language processing and visualization. Despite the wide range of topics (including some fairly complex ones with Latent Dirchlet Allocation), the talk is meant to be accessible to a wide audience and demonstrate just how powerful newbie data scientists can be with the proper guidance and modularization of knowledge. With that, I hope you enjoy and start to get a glimpse at both the power and accessability of many modern day data science workflows!  


# Analyzing NY Times Articles

![]({{matthew-mitchell.github.io}}/images/nytimes_nlp/new-york-times-logo.jpg)

## General Data Science Outline

![  blank ]({{matthew-mitchell.github.io}}/images/nytimes_nlp/ds_cycle.png)

## Our Outline
![  blank ]({{matthew-mitchell.github.io}}/images/nytimes_nlp/News_Analysis_Outline.jpg)

# Acquire Articles - The NY Times API

https://developers.nytimes.com/

![  blank ]({{matthew-mitchell.github.io}}/images/nytimes_nlp/nytimes_api.png)



### Reading Documentation

https://developer.nytimes.com/article_search_v2.json

![  blank ]({{matthew-mitchell.github.io}}/images/nytimes_nlp/nyt_docs.png)

![  blank ]({{matthew-mitchell.github.io}}/images/nytimes_nlp/nyt_ex.png)

#### HTTP Requests <a id="http"></a>
HTTP stands for Hyper Text Transfer Protocol. This protocol (like many) was proposed by the Internet Engineering Task Force (IETF) through a request for comments (RFC). We're going to start with a very simple HTTP method: the get method.  

![  blank ]({{matthew-mitchell.github.io}}/images/nytimes_nlp/http_requests.png)

To learn more about HTTP methods see:  
https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods

#### Python's Requests Package <a id="req"></a>
The first thing to understand when dealing with APIs is how to make get requests in general.
To do this, we'll use the Python requests package.

http://docs.python-requests.org/en/master/

![  blank ]({{matthew-mitchell.github.io}}/images/nytimes_nlp/requests_homepage.png)

### Making a  get request <a id="get_request"></a>


```python
import requests
```


```python
response = requests.get('https://flatironschool.com')
print('Type:', type(response), '\n')
print('Response:', response, '\n')
print('Response text:\n', response.text)
```

    Type: <class 'requests.models.Response'> 
    
    Response: <Response [403]> 
    
    Response text:
     <html>
    <head><title>403 Forbidden</title></head>
    <body bgcolor="white">
    <center><h1>403 Forbidden</h1></center>
    <hr><center>nginx</center>
    </body>
    </html>
    


Hmmm, well that was only partially helpful. You can see that our request was denied. (This is shown by the response itself, which has the code 403, meaning forbidden.) Most likely, this is caused by permissioning from Flatiron School's servers, which may be blocking requests that appear to be from an automated platform.

### HTTP Response Codes <a id="http_codes"></a>
In general, here's some common HTTP response codes you might come across:
![](./images/http_response_codes.gif)

Let's try another get request in the hopes of getting a successful (200) response.


```python
#The Electronic Frontier Foundation (EFF) website; advocating for data privacy and an open internet
response = requests.get('https://www.eff.org')
print(response)
print(response.text[:2500])
```

    <Response [200]>
    <!DOCTYPE html>
      <!--[if IEMobile 7]><html class="no-js ie iem7" lang="en" dir="ltr"><![endif]-->
      <!--[if lte IE 6]><html class="no-js ie lt-ie9 lt-ie8 lt-ie7" lang="en" dir="ltr"><![endif]-->
      <!--[if (IE 7)&(!IEMobile)]><html class="no-js ie lt-ie9 lt-ie8" lang="en" dir="ltr"><![endif]-->
      <!--[if IE 8]><html class="no-js ie lt-ie9" lang="en" dir="ltr"><![endif]-->
      <!--[if (gte IE 9)|(gt IEMobile 7)]><html class="no-js ie" lang="en" dir="ltr" prefix="fb: http://ogp.me/ns/fb# og: http://ogp.me/ns# article: http://ogp.me/ns/article# book: http://ogp.me/ns/book# profile: http://ogp.me/ns/profile# video: http://ogp.me/ns/video# product: http://ogp.me/ns/product#"><![endif]-->
      <!--[if !IE]><!--><html class="no-js" lang="en" dir="ltr" prefix="fb: http://ogp.me/ns/fb# og: http://ogp.me/ns# article: http://ogp.me/ns/article# book: http://ogp.me/ns/book# profile: http://ogp.me/ns/profile# video: http://ogp.me/ns/video# product: http://ogp.me/ns/product#"><!--<![endif]-->
    <head>
      <meta charset="utf-8" />
    <link href="https://www.eff.org/vi" rel="alternate" hreflang="vi" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed-114x114.png" sizes="114x114" />
    <link href="https://www.eff.org/ur" rel="alternate" hreflang="ur" />
    <link href="https://www.eff.org/tr" rel="alternate" hreflang="tr" />
    <link href="https://www.eff.org/sh" rel="alternate" hreflang="sh" />
    <link href="https://www.eff.org/sv" rel="alternate" hreflang="sv" />
    <link href="https://www.eff.org/th" rel="alternate" hreflang="th" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed-72x72.png" sizes="72x72" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed-144x144.png" sizes="144x144" />
    <link rel="profile" href="http://www.w3.org/1999/xhtml/vocab" />
    <link rel="shortcut icon" href="https://www.eff.org/sites/all/themes/frontier/favicon.ico" type="image/vnd.microsoft.icon" />
    <meta name="HandheldFriendly" content="true" />
    <meta name="MobileOptimized" content="width" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed.png" />
    <meta http-equiv="cleartype" content="on" />
    <link href="https://www.eff.org/ru" rel="alternate" hreflang="ru" />
    <link href="https://www.eff.org/es" rel="alternate" hreflang="es" />
    <link href


Success! As you can see, the response.text is the html code for the given url that we requested. In the background, this forms the basis for web browsers themselves. Every time you put in a new url or click on a link your computer makes a get request for that particular page and then the browser itself renders that page into a visual display on screen.

### OAuth  <a id="oauth"></a>
Some requests are a bit more complicated. Often, websites require identity verification such as logins. This helps a variety of issues such as privacy concerns, limiting access to content and tracking users history. Going forward, OAuth has furthered this idea by allowing third parties such as apps access to user information without providing the underlying password itself.

In the words of the Internet Engineering Task Force, "The OAuth 2.0 authorization framework enables a third-party
application to obtain limited access to an HTTP service, either on
behalf of a resource owner by orchestrating an approval interaction
between the resource owner and the HTTP service, or by allowing the
third-party application to obtain access on its own behalf.  This
specification replaces and obsoletes the OAuth 1.0 protocol described
in RFC 5849."

See https://oauth.net/2/ or https://tools.ietf.org/html/rfc6749 for more details.

## Access Tokens

In order to make requests to many APIs, you are required to *login* via an access token. As a result, the first step is to sign up through the web interface using your browser. Once you have an API key, you can then use it to make requests to the API. As with login passwords for your computer, these access tokens should be kept secret! For example, rather then including the passwords directly in this file, I have saved them to a seperate file called **'ny_times_api_keys.py'**. The file would look something like this:

```api_key = 'blah_blah_blah_YOUR_KEY_HERE'```

Now it's time to start making some api calls!


```python
from ny_times_api_keys import *
```


```python
import requests
```


```python
url = "https://api.nytimes.com/svc/search/v2/articlesearch.json"
url_params = {"api-key" : api_key,
             'fq' : 'The New York Times',
             'sort' : "newest"}
response = requests.get(url, params=url_params)

```


```python
response
```




    <Response [200]>



# JSON Files

While you can see that we received an HTTP code of 200, indicating success, the actual data from the request is stored in a json file format. JSON stands for **Javascript Object Notation** and is the standard format for most data requests from the web these days. You can read more about json [here](https://www.json.org/). With that, let's take a quick look at our data:


```python
response.json()
```




    {'status': 'OK',
     'copyright': 'Copyright (c) 2018 The New York Times Company. All Rights Reserved.',
     'response': {'docs': [{'web_url': 'https://www.nytimes.com/interactive/2018/upshot/elections-poll-ny22.html',
        'snippet': 'The district stretches from Lake Ontario to the Pennsylvania border.',
        'blog': {},
        'source': 'The New York Times',
        'multimedia': [{'rank': 0,
          'subtype': 'xlarge',
          'caption': None,
          'credit': None,
          'type': 'image',
          'url': 'images/2018/11/01/upshot/elections-poll-ny22-1541083888183/elections-poll-ny22-1541083888183-articleLarge.png',
          'height': 368,
          'width': 600,
          'legacy': {'xlarge': 'images/2018/11/01/upshot/elections-poll-ny22-1541083888183/elections-poll-ny22-1541083888183-articleLarge.png',
           'xlargewidth': 600,
           'xlargeheight': 368},
          'subType': 'xlarge',
          'crop_name': 'articleLarge'},
         {'rank': 0,
          'subtype': 'wide',
          'caption': None,
          'credit': None,
          'type': 'image',
          'url': 'images/2018/11/01/upshot/elections-poll-ny22-1541083888183/elections-poll-ny22-1541083888183-thumbWide.png',
          'height': 126,
          'width': 190,
          'legacy': {'wide': 'images/2018/11/01/upshot/elections-poll-ny22-1541083888183/elections-poll-ny22-1541083888183-thumbWide.png',
           'widewidth': 190,
           'wideheight': 126},
          'subType': 'wide',
          'crop_name': 'thumbWide'}],
        'headline': {'main': 'Trump’s Nationalism Is Breaking Point for Some Suburban Voters, Risking G.O.P. Coalition',
         'kicker': None,
         'content_kicker': None,
         'print_headline': 'Trump’s Nationalism Is Breaking Point for Some Suburban Voters, Risking G.O.P. Coalition',
         'name': None,
         'seo': None,
         'sub': None},
        'keywords': [{'name': 'subject',
          'value': 'Midterm Elections (2018)',
          'rank': 1,
          'major': 'N'},
         {'name': 'persons', 'value': 'Trump, Donald J', 'rank': 2, 'major': 'N'},
         {'name': 'subject',
          'value': 'Elections, House of Representatives',
          'rank': 3,
          'major': 'N'},
         {'name': 'subject',
          'value': 'Presidential Election of 2016',
          'rank': 4,
          'major': 'N'},
         {'name': 'subject',
          'value': 'Voting and Voters',
          'rank': 5,
          'major': 'N'},
         {'name': 'persons', 'value': 'Culberson, John', 'rank': 6, 'major': 'N'},
         {'name': 'persons',
          'value': 'Fletcher, Lizzie Pannill',
          'rank': 7,
          'major': 'N'},
         {'name': 'persons',
          'value': 'Fitzpatrick, Brian K (1973- )',
          'rank': 8,
          'major': 'N'},
         {'name': 'persons',
          'value': 'Wallace, H Scott',
          'rank': 9,
          'major': 'N'}],
        'pub_date': '2018-11-01T16:24:38+0000',
        'document_type': 'article',
        'news_desk': 'Politics',
        'section_name': 'Politics',
        'byline': {'original': 'By JONATHAN MARTIN and ALEXANDER BURNS',
         'person': [{'firstname': 'Jonathan',
           'middlename': None,
           'lastname': 'MARTIN',
           'qualifier': None,
           'title': None,
           'role': 'reported',
           'organization': '',
           'rank': 1},
          {'firstname': 'Alexander',
           'middlename': None,
           'lastname': 'BURNS',
           'qualifier': None,
           'title': None,
           'role': 'reported',
           'organization': '',
           'rank': 2}],
         'organization': None},
        'type_of_material': 'News',
        '_id': '5bdb28d100a1bc2872e91b0b',
        'word_count': 1657,
        'score': 1.0,
        'uri': 'nyt://article/29a6a865-9cd4-5b3a-bb92-c78fe1d6d954'},
       {'web_url': 'https://www.nytimes.com/aponline/2018/11/01/world/africa/ap-the-missing-lost-migrants-abridged.html',
        'snippet': 'As migration rises worldwide, so has its toll: The tens of thousands of people who die or simply disappear during their journeys. Barely counted in life, these migrants rarely register in death &#8212; almost as if they never lived at all.',
        'blog': {},
        'source': 'AP',
        'multimedia': [],
        'headline': {'main': '56,800 Dead and Missing: The Hidden Toll of Migration',
         'kicker': None,
         'content_kicker': None,
         'print_headline': '56,800 Dead and Missing: The Hidden Toll of Migration',
         'name': None,
         'seo': None,
         'sub': None},
        'keywords': [],
        'pub_date': '2018-11-01T16:23:39+0000',
        'document_type': 'article',
        'news_desk': 'None',
        'section_name': 'Africa',
        'byline': {'original': 'By THE ASSOCIATED PRESS',
         'person': [],
         'organization': 'THE ASSOCIATED PRESS'},
        'type_of_material': 'News',
        '_id': '5bdb289800a1bc2872e91b0a',
        'word_count': 858,
        'score': 1.0,
        'uri': 'nyt://article/caa668b3-9ab1-56fc-a73d-b74d50e70b83'}
        ],
      'meta': {'hits': 10621507, 'offset': 0, 'time': 475}}}




```python
type(response.json())
```




    dict




```python
response.json()['response']['docs'][0]['web_url']
```




    'https://www.nytimes.com/1989/10/21/world/poland-s-premier-in-rome-seeks-aid.html'




```python
response.json()['response']['docs'][0]['headline']
```




    {'main': "POLAND'S PREMIER, IN ROME, SEEKS AID",
     'kicker': None,
     'content_kicker': None,
     'print_headline': None,
     'name': None,
     'seo': None,
     'sub': None}



## Transforming Our Data


```python
import pandas as pd
```


```python
pd.DataFrame(response.json()['response']['docs'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>_id</th>
      <th>abstract</th>
      <th>blog</th>
      <th>byline</th>
      <th>document_type</th>
      <th>headline</th>
      <th>keywords</th>
      <th>multimedia</th>
      <th>news_desk</th>
      <th>print_page</th>
      <th>pub_date</th>
      <th>score</th>
      <th>section_name</th>
      <th>snippet</th>
      <th>source</th>
      <th>type_of_material</th>
      <th>web_url</th>
      <th>word_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4fd1aa418eb7c8105d6c7bc7</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By ALAN RIDING, Special to The N...</td>
      <td>article</td>
      <td>{'main': 'POLAND'S PREMIER, IN ROME, SEEKS AID...</td>
      <td>[{'name': 'persons', 'value': 'POPE', 'rank': ...</td>
      <td>[]</td>
      <td>Foreign Desk</td>
      <td>4</td>
      <td>1989-10-21T00:00:00Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>LEAD: On his first trip abroad since taking of...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>https://www.nytimes.com/1989/10/21/world/polan...</td>
      <td>694</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4fc04afd45c1498b0d22d8e2</td>
      <td>Cable opened</td>
      <td>{}</td>
      <td>NaN</td>
      <td>article</td>
      <td>{'main': 'PRESIDENT OPENS NEW MANILA CABLE; Me...</td>
      <td>[{'name': 'persons', 'value': 'ROOSEVELT, THEO...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>1</td>
      <td>1903-07-05T00:00:00Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>With the completion of the Commercial Pacific ...</td>
      <td>The New York Times</td>
      <td>Front Page</td>
      <td>https://query.nytimes.com/gst/abstract.html?re...</td>
      <td>1140</td>
    </tr>
    <tr>
      <th>2</th>
      <td>50193d971c22dfde670b384a</td>
      <td>The N.H.L. gave the players' union thousands o...</td>
      <td>{}</td>
      <td>{'original': 'By JEFF Z. KLEIN', 'person': [{'...</td>
      <td>blogpost</td>
      <td>{'main': 'In N.H.L. Negotiation, 76,000 Pages,...</td>
      <td>[{'name': 'persons', 'value': 'Bettman, Gary',...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2012-08-01T10:24:31Z</td>
      <td>1.0</td>
      <td>Hockey</td>
      <td>The N.H.L. gave the players' union thousands o...</td>
      <td>The New York Times</td>
      <td>Blog</td>
      <td>https://slapshot.blogs.nytimes.com/2012/08/01/...</td>
      <td>306</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4fc0b42c45c1498b0d417411</td>
      <td>Leaves for Portsmouth, Me</td>
      <td>{}</td>
      <td>NaN</td>
      <td>article</td>
      <td>{'main': 'Birth Notice 1 -- No Title', 'kicker...</td>
      <td>[{'name': 'persons', 'value': 'MEADOWCROFT, WM...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>19</td>
      <td>1931-07-18T00:00:00Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>Leaves for Portsmouth, Me</td>
      <td>The New York Times</td>
      <td>Birth Notice</td>
      <td>https://query.nytimes.com/gst/abstract.html?re...</td>
      <td>139</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5360131038f0d87ca6edfc10</td>
      <td>chrysanthemum show, NY Botanical Garden; illus</td>
      <td>{}</td>
      <td>NaN</td>
      <td>article</td>
      <td>{'main': 'MUMS DISPLAYED IN MOTIF OF JAPAN', '...</td>
      <td>[{'name': 'subject', 'value': 'HORTICULTURE', ...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1964-11-09T00:00:00Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>chrysanthemum show, NY Botanical Garden; illus</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>https://www.nytimes.com/1964/11/09/mums-displa...</td>
      <td>198</td>
    </tr>
    <tr>
      <th>5</th>
      <td>4fc04afd45c1498b0d22d8e7</td>
      <td>Warner, J. De W., protest water fee tales</td>
      <td>{}</td>
      <td>{'original': 'JOHN DE WITT WARNER', 'person': ...</td>
      <td>article</td>
      <td>{'main': 'John De Witt Warner's Compensation.'...</td>
      <td>[{'name': 'persons', 'value': 'WARNER, J. DE W...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>16</td>
      <td>1903-12-15T00:00:00Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>Warner, J. De W., protest water fee tales</td>
      <td>The New York Times</td>
      <td>Letter</td>
      <td>https://query.nytimes.com/gst/abstract.html?re...</td>
      <td>212</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4fbfdf8145c1498b0d03fa63</td>
      <td>NaN</td>
      <td>{}</td>
      <td>NaN</td>
      <td>article</td>
      <td>{'main': 'Amusements this Evening.', 'kicker':...</td>
      <td>[]</td>
      <td>[]</td>
      <td>NaN</td>
      <td>4</td>
      <td>1863-09-23T00:03:58Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td></td>
      <td>The New York Times</td>
      <td>Article</td>
      <td>https://www.nytimes.com/1863/09/23/news/amusem...</td>
      <td>74</td>
    </tr>
    <tr>
      <th>7</th>
      <td>501945961c22dfde670b3882</td>
      <td>A new study shows less mislabeling of sturgeon...</td>
      <td>{}</td>
      <td>{'original': 'By FLORENCE FABRICANT', 'person'...</td>
      <td>blogpost</td>
      <td>{'main': 'A Caviar Crackdown Has Worked, Resea...</td>
      <td>[{'name': 'glocations', 'value': 'New York Cit...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2012-08-01T11:02:20Z</td>
      <td>1.0</td>
      <td>Dining &amp;amp; Wine</td>
      <td>A new study shows less mislabeling of sturgeon...</td>
      <td>The New York Times</td>
      <td>Blog</td>
      <td>https://dinersjournal.blogs.nytimes.com/2012/0...</td>
      <td>412</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4fc0b42c45c1498b0d417415</td>
      <td>Wilson E; kidnapping feared</td>
      <td>{}</td>
      <td>NaN</td>
      <td>article</td>
      <td>{'main': 'ACTRESS VANISHES; KIDNAPPING FEARED;...</td>
      <td>[{'name': 'persons', 'value': 'WILSON, EVELYN'...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>15</td>
      <td>1931-07-06T00:00:00Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>With a black silk purse as their most tangible...</td>
      <td>The New York Times</td>
      <td>Article</td>
      <td>https://query.nytimes.com/gst/abstract.html?re...</td>
      <td>713</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4fc4782845c1498b0d9f334b</td>
      <td>State Farm Mutual Auto Ins Co announces it wil...</td>
      <td>{}</td>
      <td>NaN</td>
      <td>article</td>
      <td>{'main': 'STATE FARM PLANS DIVIDEND PAYMENTS',...</td>
      <td>[{'name': 'subject', 'value': 'AUTOMOBILE INSU...</td>
      <td>[]</td>
      <td>NaN</td>
      <td>52</td>
      <td>1971-08-17T00:00:00Z</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>The State Farm Mutual Automobile Insurance Com...</td>
      <td>The New York Times</td>
      <td>Article</td>
      <td>https://query.nytimes.com/gst/abstract.html?re...</td>
      <td>171</td>
    </tr>
  </tbody>
</table>
</div>



## Repeating the process progromatically


```python
import time
```


```python
responses = []
for i in range(10**3):
    url = "https://api.nytimes.com/svc/search/v2/articlesearch.json"
    url_params = {"api-key" : api_key,
                 'fq' : 'The New York Times',
                  'q' : 'politics',
                  'sort' : "newest",
                 'page': i}
    response = requests.get(url, params=url_params)
    if response.status_code == 200:
        responses.append(response)
    else:
        print('Request Failed.')
        print(response)
        print('Pausing for 60 seconds.')
        time.sleep(60)
    time.sleep(2) #Always include a 2 second pause
print(len(responses))
```

## Pulling Out Headline Text


```python
dfs = []
for r in responses:
    dfs.append(pd.DataFrame(r.json()['response']['docs']))
df = pd.concat(dfs, ignore_index=True)
print(len(df))
df.head()
```

    2010





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>_id</th>
      <th>abstract</th>
      <th>blog</th>
      <th>byline</th>
      <th>document_type</th>
      <th>headline</th>
      <th>keywords</th>
      <th>multimedia</th>
      <th>news_desk</th>
      <th>print_page</th>
      <th>pub_date</th>
      <th>score</th>
      <th>section_name</th>
      <th>snippet</th>
      <th>source</th>
      <th>type_of_material</th>
      <th>uri</th>
      <th>web_url</th>
      <th>word_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5bdb3ba600a1bc2872e91b3c</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By MICHAEL TACKETT', 'person': [...</td>
      <td>article</td>
      <td>{'main': 'Writing Postcards Brings Voters Back...</td>
      <td>[{'name': 'subject', 'value': 'Politics and Go...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:45:04+0000</td>
      <td>16.240780</td>
      <td>Politics</td>
      <td>A grass roots army of almost 40,000 is hand wr...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/5de9ee53-d584-5ef8-bbb4-7ee60a67...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>1001</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5bdb39f600a1bc2872e91b39</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By MICHAEL S. SCHMIDT, MARK MAZZ...</td>
      <td>article</td>
      <td>{'main': 'Read the Emails: The Trump Campaign ...</td>
      <td>[{'name': 'subject', 'value': 'Presidential El...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:37:56+0000</td>
      <td>15.122690</td>
      <td>Politics</td>
      <td>Newly revealed messages show how the political...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/02569d9a-d5b6-5e19-89c6-d74fc500...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>964</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5bdb39b100a1bc2872e91b38</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By SHARON LaFRANIERE, MICHAEL S....</td>
      <td>article</td>
      <td>{'main': 'Roger Stone Sold Himself to Trump’s ...</td>
      <td>[{'name': 'subject', 'value': 'Presidential El...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:36:36+0000</td>
      <td>13.804007</td>
      <td>Politics</td>
      <td>The special counsel is investigating whether M...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/4ee43518-63b8-5f83-9077-c59eed29...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>1762</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5bdb391e00a1bc2872e91b32</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By JASON FARAGO', 'person': [{'f...</td>
      <td>article</td>
      <td>{'main': 'How Conspiracy Theories Shape Art', ...</td>
      <td>[{'name': 'subject', 'value': 'Art', 'rank': 1...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Weekend</td>
      <td>NaN</td>
      <td>2018-11-01T17:34:14+0000</td>
      <td>9.288733</td>
      <td>Art &amp; Design</td>
      <td>At the Met Breuer, the crackpot exhibition “Ev...</td>
      <td>The New York Times</td>
      <td>Review</td>
      <td>nyt://article/1deb977a-def7-5508-999d-9394c566...</td>
      <td>https://www.nytimes.com/2018/11/01/arts/design...</td>
      <td>1436</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5bdb391f00a1bc2872e91b33</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By ALAN RAPPEPORT', 'person': [{...</td>
      <td>article</td>
      <td>{'main': 'Democrats Eye Trump’s Tax Returns, W...</td>
      <td>[{'name': 'persons', 'value': 'Trump, Donald J...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:34:12+0000</td>
      <td>16.596770</td>
      <td>Politics</td>
      <td>Democrats intend to request the president’s ta...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/08b8183d-f48a-5027-8d1b-286cf30a...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>1016</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['main_headline'] = df.headline.map(lambda x: x['main'])
```


```python
text = ''
for h in df.main_headline:
    text += str(h)
print(len(text), text[:50], text[-50:])
```

    117759 Writing Postcards Brings Voters Back From the Edge ard Channing Is a Mother to Remember in ‘Apologia’



```python
df.to_csv('Pulls_Nov1_2018_recent.csv', index=False)
```


```python
import pandas as pd
df = pd.read_csv('Pulls_Nov1_2018_recent.csv')
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>_id</th>
      <th>abstract</th>
      <th>blog</th>
      <th>byline</th>
      <th>document_type</th>
      <th>headline</th>
      <th>keywords</th>
      <th>multimedia</th>
      <th>news_desk</th>
      <th>print_page</th>
      <th>pub_date</th>
      <th>score</th>
      <th>section_name</th>
      <th>snippet</th>
      <th>source</th>
      <th>type_of_material</th>
      <th>uri</th>
      <th>web_url</th>
      <th>word_count</th>
      <th>main_headline</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5bdb3ba600a1bc2872e91b3c</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By MICHAEL TACKETT', 'person': [...</td>
      <td>article</td>
      <td>{'main': 'Writing Postcards Brings Voters Back...</td>
      <td>[{'name': 'subject', 'value': 'Politics and Go...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:45:04+0000</td>
      <td>16.240780</td>
      <td>Politics</td>
      <td>A grass roots army of almost 40,000 is hand wr...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/5de9ee53-d584-5ef8-bbb4-7ee60a67...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>1001</td>
      <td>Writing Postcards Brings Voters Back From the ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5bdb39f600a1bc2872e91b39</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By MICHAEL S. SCHMIDT, MARK MAZZ...</td>
      <td>article</td>
      <td>{'main': 'Read the Emails: The Trump Campaign ...</td>
      <td>[{'name': 'subject', 'value': 'Presidential El...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:37:56+0000</td>
      <td>15.122690</td>
      <td>Politics</td>
      <td>Newly revealed messages show how the political...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/02569d9a-d5b6-5e19-89c6-d74fc500...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>964</td>
      <td>Read the Emails: The Trump Campaign and Roger ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5bdb39b100a1bc2872e91b38</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By SHARON LaFRANIERE, MICHAEL S....</td>
      <td>article</td>
      <td>{'main': 'Roger Stone Sold Himself to Trump’s ...</td>
      <td>[{'name': 'subject', 'value': 'Presidential El...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:36:36+0000</td>
      <td>13.804007</td>
      <td>Politics</td>
      <td>The special counsel is investigating whether M...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/4ee43518-63b8-5f83-9077-c59eed29...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>1762</td>
      <td>Roger Stone Sold Himself to Trump’s Campaign a...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5bdb391e00a1bc2872e91b32</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By JASON FARAGO', 'person': [{'f...</td>
      <td>article</td>
      <td>{'main': 'How Conspiracy Theories Shape Art', ...</td>
      <td>[{'name': 'subject', 'value': 'Art', 'rank': 1...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Weekend</td>
      <td>NaN</td>
      <td>2018-11-01T17:34:14+0000</td>
      <td>9.288734</td>
      <td>Art &amp; Design</td>
      <td>At the Met Breuer, the crackpot exhibition “Ev...</td>
      <td>The New York Times</td>
      <td>Review</td>
      <td>nyt://article/1deb977a-def7-5508-999d-9394c566...</td>
      <td>https://www.nytimes.com/2018/11/01/arts/design...</td>
      <td>1436</td>
      <td>How Conspiracy Theories Shape Art</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5bdb391f00a1bc2872e91b33</td>
      <td>NaN</td>
      <td>{}</td>
      <td>{'original': 'By ALAN RAPPEPORT', 'person': [{...</td>
      <td>article</td>
      <td>{'main': 'Democrats Eye Trump’s Tax Returns, W...</td>
      <td>[{'name': 'persons', 'value': 'Trump, Donald J...</td>
      <td>[{'rank': 0, 'subtype': 'xlarge', 'caption': N...</td>
      <td>Washington</td>
      <td>NaN</td>
      <td>2018-11-01T17:34:12+0000</td>
      <td>16.596770</td>
      <td>Politics</td>
      <td>Democrats intend to request the president’s ta...</td>
      <td>The New York Times</td>
      <td>News</td>
      <td>nyt://article/08b8183d-f48a-5027-8d1b-286cf30a...</td>
      <td>https://www.nytimes.com/2018/11/01/us/politics...</td>
      <td>1016</td>
      <td>Democrats Eye Trump’s Tax Returns, With Mnuchi...</td>
    </tr>
  </tbody>
</table>
</div>



## Simple Visualizations


```python
import matplotlib.pyplot as plt
import seaborn as sns
# sns.set_style('darkgrid')
%matplotlib inline
```


```python
df.word_count.hist(figsize=(10,10))
plt.title('Distribution of Words Per Article')
plt.xlabel('Number of Words in Articles')
plt.ylabel('Number of Articles')
```




    Text(0,0.5,'Number of Articles')




![png](Analyzing_NYTimes_Articles_files/Analyzing_NYTimes_Articles_38_1.png)



```python
word_counts = {}
for h in df.main_headline:
    for word in h.split():
        word = word.lower()
        word_counts[word] = word_counts.get(word, 0) + 1
word_counts = pd.DataFrame.from_dict(word_counts, orient='index')
word_counts.columns = ['count']
word_counts = word_counts.sort_values(by='count', ascending=False)
word_counts.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>to</th>
      <td>500</td>
    </tr>
    <tr>
      <th>in</th>
      <td>445</td>
    </tr>
    <tr>
      <th>the</th>
      <td>443</td>
    </tr>
    <tr>
      <th>of</th>
      <td>301</td>
    </tr>
    <tr>
      <th>a</th>
      <td>283</td>
    </tr>
    <tr>
      <th>trump</th>
      <td>227</td>
    </tr>
    <tr>
      <th>for</th>
      <td>218</td>
    </tr>
    <tr>
      <th>on</th>
      <td>185</td>
    </tr>
    <tr>
      <th>and</th>
      <td>161</td>
    </tr>
    <tr>
      <th>is</th>
      <td>127</td>
    </tr>
  </tbody>
</table>
</div>




```python
word_counts.head(25).plot(kind='barh', figsize=(12,10))
plt.title('Most Frequent Headline Words',  fontsize=18)
plt.xlabel('Frequency', fontsize=14)
plt.ylabel('Word',  fontsize=14)
plt.xticks( fontsize=14)
plt.yticks( fontsize=14)
```




    (array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
            17, 18, 19, 20, 21, 22, 23, 24]),
     <a list of 25 Text yticklabel objects>)




![png](Analyzing_NYTimes_Articles_files/Analyzing_NYTimes_Articles_40_1.png)


## Topic Modelling

#### Brief Background

In order to perform topic modelling on our data we will use two primary tools. The first is to turn our text into a vector of word frequency counts; each possible word will be a feature and the number of times that word occurs will be represented by a number. From there, we can then apply mathematical operations to this numerical representation. In our case, we will be applying a common Natural Language Processing Algorithm known as Latent Dirichlet Allocation (LDA). For more technical details, start [here](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation).

## Count Vectorizer


```python
from sklearn.feature_extraction.text import CountVectorizer
corpus = [
        'This is the first document.',
        'This document is the second document.',
        'And this is the third one.',
        'Is this the first document?',
        ]
vectorizer = CountVectorizer()
X = vectorizer.fit_transform(corpus)
print(vectorizer.get_feature_names())
print(X.toarray())  
```

    ['and', 'document', 'first', 'is', 'one', 'second', 'the', 'third', 'this']
    [[0 1 1 1 0 0 1 0 1]
     [0 2 0 1 0 1 1 0 1]
     [1 0 0 1 1 0 1 1 1]
     [0 1 1 1 0 0 1 0 1]]



```python
#Installing a new python package on the fly
!pip install lda
```

## LDA: Latent Dirichlet Allocation

https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation

http://jmlr.org/papers/volume3/blei03a/blei03a.pdf

Latent dirichlet allocation is a probabilistic model for classifying documents. It works by viewing documents as mixtures of topics. In turn, topics can be viewed of as probability distributions of words. As we'll see, this allows us to model topics of a corpus and then visualize these topics by the top words associated with the topics as word clouds. While the mathematics behind LDA is fairly complex and outside the scope of this presentation, you can easily implement this powerful concept using prebuilt tools based on this academic research.



```python
from sklearn.feature_extraction.text import CountVectorizer
import lda
import numpy as np

tf_vectorizer = CountVectorizer(max_df=0.95, min_df=2, max_features=10000,
                                stop_words='english');
tf = tf_vectorizer.fit_transform(df.main_headline);


model = lda.LDA(n_topics=6, n_iter=1500, random_state=1);
model.fit(tf);

topic_word = model.topic_word_  # model.components_ also works
vocab = tf_vectorizer.get_feature_names();
```

    INFO:lda:n_documents: 2010
    INFO:lda:vocab_size: 1943
    INFO:lda:n_words: 10657
    INFO:lda:n_topics: 6
    INFO:lda:n_iter: 1500
    WARNING:lda:all zero row in document-term matrix found
    /Users/matthew.mitchell/anaconda3/lib/python3.6/site-packages/lda/utils.py:55: FutureWarning: Conversion of the second argument of issubdtype from `int` to `np.signedinteger` is deprecated. In future, it will be treated as `np.int64 == np.dtype(int).type`.
      if sparse and not np.issubdtype(doc_word.dtype, int):
    INFO:lda:<0> log likelihood: -123726
    INFO:lda:<10> log likelihood: -86737
    INFO:lda:<20> log likelihood: -85052
    INFO:lda:<30> log likelihood: -84339
    INFO:lda:<40> log likelihood: -83882
    INFO:lda:<50> log likelihood: -83550
    INFO:lda:<1499> log likelihood: -81516



```python
n_top_words = 10
for i, topic_dist in enumerate(topic_word):
    topic_words = np.array(vocab)[np.argsort(topic_dist)][:-(n_top_words):-1]
    print('Topic {}: {}'.format(i, ' '.join(topic_words)))
```

    Topic 0: trump bombs court bomb suspect ex political pipe charged
    Topic 1: says trump china caravan migrant border vote mexico eu
    Topic 2: new brazil president pm election pittsburgh right political sri
    Topic 3: saudi khashoggi election midterm poll briefing vs district elections
    Topic 4: trump democrats house race latest senate republicans governor campaign
    Topic 5: trump new york debate tax america media white plan


## Visualization


```python
from wordcloud import WordCloud

topic = "new soviet russia national talks party music world minister"
# Generate a word cloud image
wordcloud = WordCloud().generate(topic)

# Display the generated image:
# the matplotlib way:
import matplotlib.pyplot as plt
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")

# lower max_font_size
wordcloud = WordCloud(max_font_size=40).generate(topic)
plt.figure()
plt.imshow(wordcloud, interpolation="bilinear")
plt.axis("off")
plt.show()
```


![png](Analyzing_NYTimes_Articles_files/Analyzing_NYTimes_Articles_49_0.png)



![png](Analyzing_NYTimes_Articles_files/Analyzing_NYTimes_Articles_49_1.png)



```python
sns.set_style(None)
```


```python
fig, axes = plt.subplots(3,2, figsize=(15,15))
fig.tight_layout()
for i, topic_dist in enumerate(topic_word):
    topic_words = list(np.array(vocab)[np.argsort(topic_dist)][:-(n_top_words):-1])
    topic_words = ' '.join(topic_words)
    row = i//2
    col = i%2
    ax = axes[row, col]
    wordcloud = WordCloud().generate(topic_words)
    ax.imshow(wordcloud, interpolation='bilinear')
    ax.set_title('Topic {}'.format(i))
plt.tight_layout()
```


![png](Analyzing_NYTimes_Articles_files/Analyzing_NYTimes_Articles_51_0.png)


## Summary

Congratulations! We've covered a lot here! We started with HTTP requests, one of the fundamental protocols underlying the internet that we know and love. From there, we further investigated OAuth and saw how to get an access token to use in an API such as yelp. Then we made some requests to retrieve information that came back as a json format. We then transformed this data into a dataframe using the Pandas package. Finally, we created an initial visualization of the data that we retrieved using matplotlib!

## Appendix Extensions

## Scraping Full Articles


```python
from bs4 import BeautifulSoup
```


```python
def scrape_full_article_text(url):
    response = requests.get(url)
    page = response.text
    soup = BeautifulSoup(page, 'html.parser')
    paragraphs = soup.find_all('p', attrs={'class': 'story-body-text'})
    full_text=str()
    for paragraph in paragraphs:
        raw_paragraph = paragraph.contents
        cleaned_paragraph=str()
        for piece in raw_paragraph:
            if piece.string:
                cleaned_paragraph += piece.string
                cleaned_paragraph = cleaned_paragraph.replace(r"<.*?>","")
                cleaned_paragraph = cleaned_paragraph.encode('ascii','ignore')
                print(cleaned_paragraph, type(cleaned_paragraph))
        full_text += str(cleaned_paragraph)
    return full_text
```
