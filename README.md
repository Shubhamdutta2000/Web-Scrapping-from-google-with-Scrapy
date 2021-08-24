# Web Scrap from google with Scrapy

Python Scrapy spider that searches Google for a particular keyword and extracts all data from the SERP results. The spider will iterate through all pages returned by the keyword query. The following are the fields the spider scrapes for the Google SERP page:

* Title
* Link
* Snippet
* Date of Scan
* No. of Scans

# How it Works

This Google SERP spider uses Scraper API as the proxy solution. Scraper API has a free plan that allows you to make up to 1,000 requests per month which makes it ideal for the development phase, but can be easily scaled up to millions of pages per month if needs be.

Scraper API also offers autoparsing functionality free of charge for Google, so by adding `&autoparse=true` to the request the API returns all the SERP data in JSON format.

This spider can be easily customised for your particular search requirements. In this case, it allows you to refine your search queries by specifying a  keyword, the geographic region, the language, the number of results, results from a particular domain, or even to only return safe results.


# How To Use

Install pipenv for virtual environment

```
pip install pipenv
```

Launching subshell in virtual environment

```
pipenv shell
```

Install packages in virtual environment

```
pipenv install
```

Set the keywords you want to search in Google.

```
queries = ['scrapy', 'beautifulsoup']
```

Signup to [Scraper API](https://www.scraperapi.com/signup) and get your free API key that allows you to scrape 1,000 pages per month for free. Enter your API key into the API variable:

```
API_KEY = 'YOUR_API_KEY' 

def get_url(url):
    payload = {'api_key': API_KEY, 'url': url, 'autoparse': 'true', 'country_code': 'us'}
    proxy_url = 'http://api.scraperapi.com/?' + urlencode(payload)
    return proxy_url
```

By default, the spider is set to have a max concurrency of 5 concurrent requests as this the max concurrency allowed on Scraper APIs free plan. If you have a plan with higher concurrency then make sure to increase the max concurrency in `custom_settings` dictionary.

```
custom_settings = {'ROBOTSTXT_OBEY': False, 'LOG_LEVEL': 'INFO',
                       'CONCURRENT_REQUESTS_PER_DOMAIN': 5, 
                       'RETRY_TIMES': 5}
```

We should also set `RETRY_TIMES` to tell Scrapy to retry any failed requests (to 5 for example) and make sure that `DOWNLOAD_DELAY`  and `RANDOMIZE_DOWNLOAD_DELAY` aren’t enabled as these will lower your concurrency and are not needed with Scraper API.

```
## settings.py

# DOWNLOAD_DELAY
# RANDOMIZE_DOWNLOAD_DELAY
```

To run the spider, use:

```
scrapy crawl google -o result.csv
```




