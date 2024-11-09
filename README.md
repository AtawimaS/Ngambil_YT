
# Ngambil_YT

This Python script extracts comments from a list of YouTube videos and stores them in a Pandas DataFrame for further analysis.

## Requirements

Before running the script, ensure you have the following dependencies installed:

- **Python 3.x**
- **Selenium** - for web scraping YouTube videos.
- **Pandas** - for handling and storing the extracted data.

To install the required libraries, run the following command:

```bash
pip install selenium pandas
```

You will also need to download the correct version of **ChromeDriver** compatible with your version of Chrome. Place it in the same directory as your script or specify the path to the driver in the script.

## How to Use

1. **Set up the list of YouTube links:**

   Define a list of YouTube video URLs from which you want to extract comments. You can add as many URLs as you like.

   Example:

   ```python
   link_youtube = [
       'https://www.youtube.com/watch?v=WWbyYFPHDH8&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=10',
       'https://www.youtube.com/watch?v=U30sF4m0bd0&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=11',
       'https://www.youtube.com/watch?v=f0a1XXmaQp8&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=12',
       'https://www.youtube.com/watch?v=oe7DW4rSH1o&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=13',
       'https://www.youtube.com/watch?v=Sj1ybuDDf9I&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=14'
   ]
   ```

2. **Define the DataFrame for storing comments:**

   Create an empty DataFrame where all comments will be stored. The DataFrame will have two columns: `URL` and `Comment`.

   ```python
   import pandas as pd

   all_comments = pd.DataFrame(columns=["URL", "Comment"])
   ```

3. **Extract Comments from Each Video:**

   Iterate through each URL in the list and extract the comments. The script will attempt to scrape 25 comments for each video and append them to the `all_comments` DataFrame. If an error occurs for a specific URL, it will print the error message and continue with the next URL.

   ```python
   from selenium import webdriver
   from selenium.webdriver.chrome.service import Service
   from selenium.webdriver.chrome.options import Options
   from selenium.webdriver.common.by import By
   from selenium.webdriver.common.action_chains import ActionChains
   import time

   def ngambil_youtube(url, num_comments):
       # Setup Selenium WebDriver options
       options = Options()
       options.add_argument("--headless")  # Running headless (without GUI)
       options.add_argument("--no-sandbox")
       options.add_argument("--disable-dev-shm-usage")
       
       # Path to ChromeDriver
       service = Service(r"C:\path\to\chromedriver.exe")
       driver = webdriver.Chrome(service=service, options=options)
       
       driver.get(url)
       time.sleep(2)
       
       driver.execute_script("window.scrollTo(0, 600);")
       time.sleep(3)
       
       comments = set()
       while len(comments) < num_comments:
           driver.execute_script("window.scrollTo(0, document.documentElement.scrollHeight);")
           time.sleep(2)
           comment_elements = driver.find_elements(By.XPATH, '//*[@id="content-text"]')
           for element in comment_elements[len(comments):]:
               comments.add(element.text)
           if len(comments) >= num_comments:
               break

       driver.quit()
       
       return list(comments)[:num_comments]

   # Define the list of URLs and store comments
   all_comments = pd.DataFrame(columns=["URL", "Comment"])
   link_youtube = [
       'https://www.youtube.com/watch?v=WWbyYFPHDH8&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=10',
       'https://www.youtube.com/watch?v=U30sF4m0bd0&list=PL2O3HdJI4voHNEv59SdXKRQVRZAFmwN9E&index=11'
   ]

   for url in link_youtube:
       try:
           comments = ngambil_youtube(url, 25)
           df_temp = pd.DataFrame({"URL": url, "Comment": comments})
           all_comments = pd.concat([all_comments, df_temp], ignore_index=True)
           print(url, " Done")
       except Exception as e:
           print(f"Error on URL {url}: {e}")
           continue

   # You can now process 'all_comments' DataFrame further as needed
   ```

## Notes

- Ensure that you have the correct ChromeDriver version for your installed Chrome browser. You can download it from [here](https://sites.google.com/chromium.org/driver/).
- The script uses **headless mode**, which means it won't open the browser's GUI. You can modify this if you want to visualize the browser.
- Comments are stored in a **Pandas DataFrame** for easy analysis or export to CSV/Excel.

