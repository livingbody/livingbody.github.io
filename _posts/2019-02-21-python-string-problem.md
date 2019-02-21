---
layout: post
title:  "pymsql转移函数处理解决不了问题"
date:   2015-02-10 15:14:54
categories: python
tags: 爬虫
author:  livingbody
---

* content
{:toc}

## 代码信息
```python
from bs4 import BeautifulSoup;
import requests;
import pymysql
import re

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

db = pymysql.connect("localhost", "root", "root", "finddouban")
cursor = db.cursor()
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
    cursor.execute('INSERT INTO moive(title,content,) VALUES (%s, %s )', (title, pymysql.escape_string(content)))
    # cursor.execute('INSERT INTO moive(title,img,content,rate,person) VALUES (%s, %s, %s, %f, %d )',
    #                (title, pymysql.escape_string(img), pymysql.escape_string(content), float(rate), int(person)))
    db.commit()
db.close()

```

## 报错

```python

C:\Users\livingbody\PycharmProjects\run\venv\Scripts\python.exe C:/Users/livingbody/PycharmProjects/run/src/getdoubanfilm.py
Traceback (most recent call last):
{'title': 'Dear EX', 'img': 'https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2546961193.jpg', 'content': '2018-04-22(乌迪内远东电影节) / 2018-11-02(台湾) / 邱泽 / 谢盈萱 / 陈如山 / 黄圣球 / 周洺甫 / 梁正群 / 杨丽音 / 安哲 / 高隽雅 / 吴定谦 / 禾语辰 / 钟欣凌 / 万芳 / 高爱伦 / 台湾 / 徐誉庭 / 许智彦 / 100分钟 / 谁先爱上他的 / 剧情 / 喜剧...', 'rate': '8.6', 'person': '100795'}
  File "C:/Users/livingbody/PycharmProjects/run/src/getdoubanfilm.py", line 56, in <module>
    cursor.execute('INSERT INTO moive(title,content,) VALUES (%s, %s )', (title,  pymysql.escape_string(content)))
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\cursors.py", line 170, in execute
    result = self._query(query)
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\cursors.py", line 328, in _query
    conn.query(q)
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\connections.py", line 517, in query
    self._affected_rows = self._read_query_result(unbuffered=unbuffered)
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\connections.py", line 732, in _read_query_result
    result.read()
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\connections.py", line 1075, in read
    first_packet = self.connection._read_packet()
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\connections.py", line 684, in _read_packet
    packet.check_error()
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\protocol.py", line 220, in check_error
    err.raise_mysql_exception(self._data)
  File "C:\Users\livingbody\PycharmProjects\run\venv\lib\site-packages\pymysql\err.py", line 109, in raise_mysql_exception
    raise errorclass(errno, errval)
pymysql.err.ProgrammingError: (1064, "You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ') VALUES ('Dear EX', '2018-04-22(乌迪内远东电影节) / 2018-11-02(台湾) ' at line 1")

Process finished with exit code 1

```

## any good ideas?