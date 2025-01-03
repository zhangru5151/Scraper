
# Python Web Scraping: Extracting Jianshu's Weekly Hot Data (Asynchronous Loading Explained)

Recently, I've been working from home and haven’t updated much. My friend [Cheng](http://www.jianshu.com/subscriptions#/subscriptions/4069462/user) has also started writing on Jianshu. You should check it out—his content is quite detailed (unlike my lazy updates). Today, I'll explain how to scrape Jianshu's weekly hot data as an example of handling asynchronous loading.

---

## Understanding Asynchronous Loading

### Step 1: Analyzing the Web Page

When you first look at the Jianshu page, it might seem straightforward:

![Initial Page](http://upload-images.jianshu.io/upload_images/3629157-3db9042cee9807c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

But as you scroll down, you’ll notice something different:

![Asynchronous Loading Example](http://upload-images.jianshu.io/upload_images/3629157-4cbb86d2959ff2f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Clearly, this is asynchronous loading. Unlike traditional web pages where links to other pages are easily visible, here you need to identify the URLs yourself. Using tools like Chrome Developer Tools or Fiddler for packet capture, we can analyze how the data is loaded. Press `F12` to open the developer tools, refresh with `F5`, scroll down, and click on "Load More." You’ll see the following packet:

![Packet Capture](http://upload-images.jianshu.io/upload_images/3629157-28a9e9fcedd3e08c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

From here, we can construct the necessary URL for scraping.

---

### Step 2: Observing the Details Page

While extracting data from the details page, I found that metrics like views, comments, likes, rewards, and the associated collections are also asynchronously loaded.

![Detail Page 1](http://upload-images.jianshu.io/upload_images/3629157-20a03f27aa8ebf2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Detail Page 2](http://upload-images.jianshu.io/upload_images/3629157-c0baa424133b456a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

If you’re unfamiliar with how to identify asynchronous data, here’s a tip: If you don’t see the desired data in the HTML source code, it’s likely loaded asynchronously. For example:

- Metrics like views, comments, and likes can often be extracted from JavaScript variables in the source code using regular expressions:

![Code Example](http://upload-images.jianshu.io/upload_images/3629157-ccd39a4d7b10502f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- For rewards and collections, you’ll need to locate the API endpoints through packet capture. For instance, the rewards data is located in a specific API endpoint:

![Rewards Data](http://upload-images.jianshu.io/upload_images/3629157-570197d9ec8bacc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Similarly, collection data can be extracted by constructing URLs based on identified patterns.

---

## Code Implementation

Here’s a Python script to scrape Jianshu’s weekly hot data:

```python
from lxml import etree
import requests
import pymongo
import re
import json
import time

client = pymongo.MongoClient('localhost', 27017)
db = client['test']
sevenday = db['sevenday']
embody = db['embody']

header = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36'
}

urls = ['http://www.jianshu.com/trending/weekly?page={}'.format(i) for i in range(11)]

def get_url(url):
    html = requests.get(url, headers=header)
    selector = etree.HTML(html.text)
    infos = selector.xpath('//ul[@class="note-list"]/li')

    for info in infos:
        article_url_part = info.xpath('div/a/@href')[0]
        get_info(article_url_part)

def get_info(url):
    article_url = 'http://www.jianshu.com/' + url
    html = requests.get(article_url, headers=header)
    selector = etree.HTML(html.text)
    
    author = selector.xpath('//span[@class="name"]/a/text()')[0]
    article = selector.xpath('//h1[@class="title"]/text()')[0]
    date = selector.xpath('//span[@class="publish-time"]/text()')[0]
    word = selector.xpath('//span[@class="wordage"]/text()')[0]
    view = re.findall('"views_count":(.*?),', html.text, re.S)[0]
    comment = re.findall('"comments_count":(.*?)}', html.text, re.S)[0]
    like = re.findall('"likes_count":(.*?),', html.text, re.S)[0]
    id = re.findall('{"id":(.*?),', html.text, re.S)[0]
    
    gain_url = 'http://www.jianshu.com/notes/{}/rewards?count=20'.format(id)
    wb_data = requests.get(gain_url, headers=header)
    json_data = json.loads(wb_data.text)
    gain = json_data['rewards_count']
    
    info = {
        'author': author,
        'article': article,
        'date': date,
        'word': word,
        'view': view,
        'comment': comment,
        'like': like,
        'gain': gain
    }
    sevenday.insert_one(info)
    
    include_urls = ['http://www.jianshu.com/notes/{}/included_collections?page={}'.format(id, i) for i in range(1, 4)]
    for include_url in include_urls:
        include_data = requests.get(include_url, headers=header)
        json_data2 = json.loads(include_data.text)
        includes = json_data2['collections']
        for include in includes:
            include_title = include['title']
            embody.insert_one({'include_title': include_title})
    
    time.sleep(2)

for url in urls:
    get_url(url)
```

---

## Key Takeaways

1. If the data is missing from the source code, it’s likely loaded asynchronously.
2. Use developer tools or packet capture software to locate the necessary data.
3. Even basic English knowledge is enough to interpret API responses and endpoints.

---

### Recommended Solution for Web Scraping Challenges

Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

This guide should help you better understand asynchronous data scraping and streamline your web scraping efforts. Stay tuned for the next example where I’ll use Scrapy to build an optimized solution for this task.
