
# Practical Python Web Scraping: Using APIs for Data Collection

## Introduction to APIs

An **API (Application Programming Interface)** is a predefined set of functions that allows developers and applications to interact with software or hardware without needing to understand the underlying code or mechanisms. APIs provide access to specific routines, making them a powerful tool for collecting structured data.

For example, consider the following API call:

```
http://apis.juhe.cn/ip/ip2addr?ip=112.112.11.11&key=appkey
```

The returned data in JSON format looks like this:

```json
{
   "resultcode":"200",
   "reason":"Return Success!",
   "result":{
      "area":"江苏省苏州市",
      "location":"电信"
   }
}
```

And the XML version would look like this:

```xml
<?xml version="1.0" encoding="utf-8"?> 
<root>
    <resultcode>200</resultcode> 
    <reason>Return Success!</reason> 
    <result>
        <area>江苏省苏州市</area> 
        <location>电信</location> 
    </result>
</root>
```

APIs are incredibly useful for accessing structured data without dealing with web scraping complexities.

---

## Using Python to Call APIs

Python is a versatile tool for working with APIs. Here's how you can make API calls and process the returned data.

### Parsing JSON Data in Python

Here’s an example of how to parse JSON data using Python:

```python
import json

jsonString = '{"arrayOfNums":[{"number":0},{"number":1},{"number":2}],"arrayOfFruits":[{"fruit":"apple"},{"fruit":"banana"},{"fruit":"pear"}]}'
jsonObj = json.loads(jsonString)
print(jsonObj.get("arrayOfNums"))
print(jsonObj.get("arrayOfNums")[1])
print(jsonObj.get("arrayOfNums")[1].get("number") + jsonObj.get("arrayOfNums")[2].get("number"))
print(jsonObj.get("arrayOfFruits")[2].get("fruit"))
```

---

### Weather Forecast API Example

Here’s how you can use Python to call a weather forecast API:

```python
from urllib import urlencode
import urllib
import json

# Your API Key
appkey = "XXXXXXXXXXXXXXXXXXXXXXXX"

def queryWeather(appkey, method="GET", city="广州", dtype="json"):
    url = "http://v.juhe.cn/weather/index"
    params = {
        "cityname": city,
        "key": appkey,
        "dtype": dtype,
    }
    params = urlencode(params)
    if method == "GET":
        f = urllib.urlopen("%s?%s" % (url, params))
    else:
        f = urllib.urlopen(url, params)

    content = f.read()
    res = json.loads(content)
    if res:
        error_code = res["error_code"]
        if error_code == 0:
            return res["result"]
        else:
            print("%s: %s" % (res["error_code"], res["reason"]))
    else:
        print("API request failed.")

weather = queryWeather(appkey)
print(weather)
```

---

### IP Address Lookup API Example

Here’s an example of how to query the location of an IP address using an API:

```python
from urllib import urlopen
import json

def getCountry(ipAddress, appkey):
    response = urlopen("http://apis.juhe.cn/ip/ip2addr?ip=" + ipAddress + "&key=" + appkey).read().decode('utf-8')
    responseJson = json.loads(response)
    return responseJson.get("area")

appkey = "84bd1042092e7b0e3265483f46febc80"
print(getCountry("61.135.169.121", appkey))
```

---

## Using Social Media APIs

### Example: Weibo API with Python 2.x

To use the Weibo API with Python 2.x, install the `sinaweibopy` SDK:

```bash
pip install sinaweibopy
```

Then use the following script to authenticate and fetch user timelines:

```python
from weibo import APIClient
import webbrowser

APP_KEY = 'XXXXXX'
APP_SECRET = 'XXXXXXXXXXXXXXXXXXXXXXXX'
CALLBACK_URL = 'http://f.dataguru.cn'

client = APIClient(app_key=APP_KEY, app_secret=APP_SECRET, redirect_uri=CALLBACK_URL)
url = client.get_authorize_url()
webbrowser.open_new(url)

code = 'YOUR_AUTHORIZATION_CODE'
r = client.request_access_token(code)
access_token = r.access_token
expires_in = r.expires_in

client.set_access_token(access_token, expires_in)

statuses = client.statuses.home_timeline.get(count=10)['statuses']
for status in statuses:
    print("User:", status['user']['screen_name'])
    print("Text:", status['text'])
```

---

### Example: Weibo API with Python 3.x

To use the Weibo API with Python 3.x, install the `weibopy` SDK:

```bash
pip install weibopy
```

Here’s an example script for Python 3.x:

```python
from weibopy import WeiboOauth2, WeiboClient
import webbrowser

APP_KEY = 'XXXXXX'
APP_SECRET = 'XXXXXXXXXXXX'
CALLBACK_URL = 'http://f.dataguru.cn'

client = WeiboOauth2(APP_KEY, APP_SECRET, CALLBACK_URL)
authorize_url = client.authorize_url
webbrowser.open_new(authorize_url)

code = 'YOUR_AUTHORIZATION_CODE'
r = client.auth_access(code)
access_token = r.get("access_token")

client = WeiboClient(access_token)
statuses = client.get(suffix="statuses/home_timeline.json").get("statuses")
for status in statuses:
    print("User:", status['user']['screen_name'])
    print("Text:", status['text'])
```

---

## Why Use APIs for Data Collection?

APIs simplify data collection by providing structured data in formats like JSON or XML. Compared to web scraping, APIs are less prone to breaking due to website changes and often come with documentation to guide developers.

### Stop Wasting Time on Proxies and CAPTCHAs!

ScraperAPI’s simple API handles millions of web scraping requests so you can focus on analyzing data. Extract structured data from platforms like Amazon, Google, and Walmart without worrying about proxies or CAPTCHAs. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

By leveraging APIs and Python, you can streamline the process of collecting and analyzing data efficiently. Choose APIs whenever possible for a cleaner and more reliable data collection workflow.
