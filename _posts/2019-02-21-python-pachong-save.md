---
layout: post
title:  "爬取豆瓣电影排行榜并保存"
categories: python
tags: python
author: livingbody
---

* content
{:toc}

## 代码
```python
from bs4 import BeautifulSoup;
import requests;
import re
import json

info = []
titles = []
imgs = []
contents = []
rates = []
persons = []
url = 'https://movie.douban.com/chart'
f = requests.get(url)
soup = BeautifulSoup(f.content, 'lxml')

for k in soup.find_all('div', class_='pl2'):  # ,找到div并且class为pl2的标签
    a = k.find('span')  # 在每个对应div标签下找span标签，会发现，一个a里面有四组span
    titles.append(a.string)
for i in soup.find_all('img', attrs={'width': '75'}):
    imgs.append(i['src'])
for c in soup.find_all('p', class_='pl'):
    contents.append(c.string)
for r in soup.find_all('span', class_='rating_nums'):
    rates.append(r.string)
for p in soup.find_all('span', class_='pl'):
    pp = totalCount = re.sub("\D", "", p.string.replace("(", "").replace(")", ""))
    persons.append(pp)


for title, img, content, rate, person in zip(titles, imgs, contents, rates, persons):
    data = {
        'title': title,
        'img': img,
        'content': content,
        'rate': rate,
        'person': person
    }
    info.append(data)
    print(data)

file = open(r'moive.json', 'w')
jsObj = json.dumps(info)
file.write(jsObj)
file.close()
file.close()

```

