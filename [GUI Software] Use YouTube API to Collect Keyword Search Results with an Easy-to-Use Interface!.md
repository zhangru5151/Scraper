# [GUI Software] Use YouTube API to Collect Keyword Search Results with an Easy-to-Use Interface!

## Background Introduction

### Objective

Hello, I’m [@马哥python说](https://cloud.tencent.com/developer/user/5690870/articles?from_column=20421&from=20421), a software engineer with 10 years of experience. 

I developed a Python-based scraping tool that collects search results from YouTube by keywords. The tool extracts 14 critical fields:

- Keyword
- Page Number
- Video Title
- Video ID
- Video URL
- Publish Time
- Video Duration
- Channel Name
- Channel ID
- Channel URL
- View Count
- Like Count
- Comment Count
- Video Description

This software leverages YouTube's official API instead of web scraping, ensuring high stability. If you’re interested, check out this [step-by-step guide](https://cloud.tencent.com/developer/article/2414978?from_column=20421&from=20421) on enabling the YouTube API (YouTube Data API v3).

To make it accessible to non-coders, the tool is developed as a GUI software. No Python installation or code editing is required—just double-click and use!

Here’s a preview of the tool’s interface:

![Software Interface](https://developer.qcloudimg.com/http-save/yehe-5690870/2b3a2b40e429c6e28c06b301efeb383b.png)

### Key Features:

1. Intuitive GUI for ease of use.
2. Extracts structured data with YouTube’s official API.
3. Outputs directly to CSV, minimizing data loss during interruptions.

---

### Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on data analysis. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## Demonstration Video

The following demonstration shows the software in action. No coding knowledge is required—simply follow the visual instructions to understand its functionality.

![Results Preview](https://developer.qcloudimg.com/http-save/yehe-5690870/664058d9d2a44814f5d988ef93b4913d.png)

![Results Preview 2](https://developer.qcloudimg.com/http-save/yehe-5690870/ca2e2d75461cadd15a030f09c2a8cb56.png)

![Results Preview 3](https://developer.qcloudimg.com/http-save/yehe-5690870/3730a132239706be91eaf994e87b9b88.png)

---

## API Integration Explained

### Search API

The software retrieves data via the YouTube Search API. Below is an example of the JSON response:

![Search API JSON](https://developer.qcloudimg.com/http-save/yehe-5690870/0fa29793f792c3f2a8bd9e4b45ebe3d7.png)

**Steps:**

1. Define the API endpoint URL for requests.
2. Set up request headers to simulate a browser.
3. Add query parameters for search filters.

---

### Video Details API

The software also utilizes the Video Details API to retrieve more detailed information. Example JSON response:

![Details API JSON](https://developer.qcloudimg.com/http-save/yehe-5690870/d76b69ac11c18774bc15a168b6d4bc0d.png)

**Steps:**

1. Define the API endpoint for video details.
2. Simulate browser headers for requests.
3. Send the request, receive data, and parse fields like "View Count" or "Likes."

The results are saved incrementally to CSV files, preventing data loss due to unexpected interruptions.

---

### API Key Configuration

The **API_KEY** is required to access YouTube’s official API. Once obtained, it needs to be added to the `config.json` file:

![config.json Example](https://developer.qcloudimg.com/http-save/yehe-5690870/1e3aee1e47375eeb9cf063b71bd318e0.png)

⚠️ Important: Ensure proper VPN or proxy configurations for accessing YouTube services from restricted regions.

---

## Software Features Breakdown

### GUI Modules

1. **Input Controls**: Simple fields to input search keywords and filters.
2. **Real-Time Logs**: A log window to track progress and diagnose issues.
3. **Footer Section**: Displays software version and credits.

---

### Logging Functionality

A robust logging system ensures seamless debugging. If an error occurs during execution, the log file can help pinpoint the issue quickly.

![Log File](https://developer.qcloudimg.com/http-save/yehe-5690870/0228bde6895c7cee8fb15351184ce2a3.png)

---

## Conclusion

This GUI-based YouTube scraping tool simplifies data collection for both developers and non-coders. By leveraging YouTube’s official API, the tool ensures reliability and stability. Whether you're gathering insights for market analysis, content planning, or academic research, this tool provides an efficient solution.

👉 Start enhancing your scraping tasks today with [ScraperAPI](https://www.scraperapi.com/?fp_ref=coupons), your go-to tool for web scraping without CAPTCHAs or proxies.

---

## Author Notes

I’m [@马哥python说](https://cloud.tencent.com/developer/user/5690870/articles?from_column=20421&from=20421), a developer sharing Python insights and tools. Follow my WeChat blog **"老男孩的平凡之路"** for more tutorials and discussions. Let’s connect!
