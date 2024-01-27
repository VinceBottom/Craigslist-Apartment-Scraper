from requests import get
import pandas as pd
import difflib
import os
import string
##declare user agent
user_agent = 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36'
headers = { 'User-Agent': 'python-craigslist/1.1.0' }
#get the first page of the san francisco ADU/in-law suite housing prices
response = get('https://sfbay.craigslist.org/search/apa?hasPic=1&housing_type=3&housing_type=7&postedToday=1#search=1~list~0~0', headers=headers)
from bs4 import BeautifulSoup
html_soup = BeautifulSoup(response.text, 'html.parser')
#get container for the housing posts
posts = html_soup.find_all('li', class_='cl-static-search-result')
post_one = posts[0]
##
length = len(posts)
count = 0
fulldf = pd.DataFrame(columns=['Title','Price','Location','Link'])
for x in range(length):
    post_current = posts[count]
    title = post_current.find("div", {"class": "title"}).get_text(strip=True)
    price = post_current.find("div", {"class": "price"}).get_text(strip=True)
    location = post_current.find("div", {"class": "location"}).get_text(strip=True)
    link = post_current.find("a", {"class": ""}).get("href")
    new_row = {'Title': title, 'Price': price, 'Location': location, 'Link': link}
    fulldf.loc[len(fulldf)] = new_row
    count += 1
os.chdir("C:\\Users\\Vince\\Desktop")
fulldf["Price"] = fulldf["Price"].replace("[$,]", "", regex=True).astype(int)##remove $ and , from Price
fulldf["Title"] = fulldf["Title"].replace("[$,]", "", regex=True)##remove $ and , from Price
from datetime import date
from datetime import datetime
today = datetime.now()
today_date_time = today.strftime("%m/%d/%Y")
##write to new or update existing csv file with append mode
for index, row in fulldf.iterrows():
       f = open('ADU Alert.csv', 'a')
       price_str = str(row['Price'])
       f.write(row['Title'][0:37]+ ',' + today_date_time + ',' + price_str + ',' + row['Link'] + ',' + '\n')
       f.close()
