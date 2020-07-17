# Crawl-data-from-Zhihu
Use the Zhihu api to get the data and save it to your computer

#import packages
```
import requests
from fake_useragent import UserAgent
import json
import time
import pandas as pd
```
#Get the fake account
```
def get_page_json(url):
    try:
        ua = UserAgent(verify_ssl=False)
        headers = {"User-Agent": ua.random}
        json_comment = requests.get(url,headers=headers)
        json_comment.encoding = "Unicode"
        json_comment = json_comment.text
        return json_comment
    except:
        return None
```
#save the data
```
def save_to_csv(comments_list):
    data = pd.DataFrame(comments_list)
    #注意存储文件的编码为utf_8_sig，不然会乱码，后期会单独深入讲讲为何为这样（如果为utf-8）
    data.to_csv('article_user_7_1.csv', mode='a', index=False, sep=',',header=False,encoding='utf_8_sig')
```
#Get the data from the Zhihu API
```
def parse_page_json(json_comment):
    try:
        comments = json.loads(json_comment)
    except:
        return "error"
    
    comments_list = []
    num = len(comments['data'])
    for i in range(num):
        comment = comments['data'][i]
        comment_list = []
        
        author = comment['author']['member']['id']
        reply_to = comment['reply_to_author']['member']['id']
        comment = comment['content']
        
        comment_list.append(author)
        comment_list.append(reply_to)
        comment_list.append(comment)
        
        comments_list.append(comment_list)        
    save_to_csv(comments_list)
```



