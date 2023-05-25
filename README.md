![Python Version](https://img.shields.io/badge/Python-3.10-brightgreen) ![License](https://img.shields.io/pypi/l/app-store-scraper)

# Apple App Store Reviews Scraper



 Apple App Store reviews scraper. Reviews are fetched using Apple's API, as the webpage of each app only displays a few reviews. 
 
 It is adapted from 
 [app-store-scraper](https://github.com/cowboy-bebug/app-store-scraper). I converted the classes to functions for ease of debugging and removed some redundant elements such as the date filter as Apple does not allow reviews to be sorted by date. Generally, the larger the `offset`, the older the reviews tend to be.

 This port is motivated by the rate limiting that is quickly encountered when using the original package. To address this, I added a default delay and backoff strategy. With the current configuration, it has been tested to scrape ~15,000 reviews with no rate limiting.
 
Includes two simple functions `get_token` and `fetch_reviews`, which are used to retrieve an authentication token and fetch app reviews, respectively.


## Usage

First, set some some `user_agents`.
```python
user_agents = [
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 13_4) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.4 Safari/605.1.15',
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36',
]
```

### Obtaining a Bearer Token

To fetch app reviews from the Apple App Store API, you need to obtain an authentication token. The `get_token` function retrieves this token.

```python
from apple_app_reviews_scraper import get_token

# Provide the necessary parameters
country = 'sg'
app_name = 'your-app-name' # can be named anything, really
app_id = 'your-app-id'

# Get token
token = get_token(country, app_name, app_id, user_agents)

print(f"Authentication Token: {token}")
```

### Fetching App Reviews

Once you have obtained the authentication token, use the `fetch_reviews` function to fetch app reviews.

```python
from apple_app_reviews_scraper import fetch_reviews
import pandas as pd

country = 'sg'
app_name = 'your-app-name' # can be named anything, really
app_id = 'your-app-id'

# Call the function
reviews, offset, status_code = fetch_reviews(country, app_name, app_id, user_agents, token)

# Preview as a DataFrame
df = pd.json_normalize(reviews)
```

JSON structure of a single review: 
```
{'id': '2801236969',
 'type': 'user-reviews',
 'attributes': {'date': '2022-06-30T08:36:15Z',
  'review': "This is a great app!",
  'rating': 5,
  'isEdited': False,
  'userName': 'updog',
  'title': 'Nice'},
 'offset': '21',
 'n_batch': 20,
 'app_id': '324684580'}
```
If developer responded:
```
 {'id': '9700279414',
 'type': 'user-reviews',
 'attributes': {'date': '2023-03-11T01:06:51Z',
  'developerResponse': {'id': 35337720,
   'body': "Thanks for your feedback!",
   'modified': '2023-03-12T17:16:37Z'},
  'review': 'I love this app!',
  'rating': 4,
  'isEdited': False,
  'title': 'Great',
  'userName': 'Nice'},
 'offset': '21',
 'n_batch': 20,
 'app_id': '913943275'}
```
## Authors

- [@glennfang](https://www.github.com/glennfang)

## Credits
- [@cowboy-bebug](https://www.github.com/cowboy-bebug/app-store-scraper) (original Python implementation)
- [@facundoolano](https://github.com/facundoolano/app-store-scraper) (original Node.JS implementation)



