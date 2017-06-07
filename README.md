# Webscraping NBA Stats on ESPN

Contributers:
+ Moritz Droste
+ Jiali Huang
+ Russell Kim
+ Chris Bell

## This is a beginner webscraper taking data from NBA players on ESPN.

To-Do List
+ All done


### Code and Walkthrough

We went through this project using python and the BeautifulSoup library. Therefore, we first had to import all the necessary libraries into python.

```
from bs4 import BeautifulSoup
import requests
import pandas 
```

Requests is a library used to request the html documents from webpages. Pandas is a library for creating and manipulating dataframes, which is how we store the data we get before putting it into a mySQL table.

Then, we dive right in and use requests to get the webpages. 

```
players = []
pageList = ["http://www.espn.com/nba//statistics/player/_/stat/scoring/sort/points"]
done = False

while done == False:
    for link in pageList:
        isNew = False
        newUrl = requests.get(link)
        newSoup = BeautifulSoup(newUrl.text, "lxml")
        for i in newSoup.find_all("a"):
            url = i.get("href")
            if "www.espn.com/nba/statistics/player/_/stat/scoring/sort/points/qualified/false/count/" in url and url not in pageList and ("http:" + url) not in pageList:
                if "http" in url:   
                    pageList.append(url)
                else: pageList.append("http:" + url)
                isNew = True
    if isNew == 0:
        done = True
            
    
for link in pageList:
    url = requests.get(link)
    soup = BeautifulSoup(url.text,"lxml")
    for i in soup.find_all("a"):
        if "http://www.espn.com/nba/player/_/id/" in i.get('href'):
            statPage = i.get('href')[:30] + "/stats" + i.get('href')[30:]
            players.append(statPage)
```

