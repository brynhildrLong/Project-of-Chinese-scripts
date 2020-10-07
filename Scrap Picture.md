## Oracle Script Scrap

### Pre-work
The url is (http://9610.com/jiagu/bian/)
What we want with the web-page code:
```html
<TABLE border=1>
<TBODY>
<TR bgColor=#cccccc>
<TD colSpan=10 height=24>
<DIV align=center><A name=D></A><FONT color=#0000ff size=3>D</FONT></DIV></TD></TR>
<TR>
<TD width=50 height=54><IMG height=50 src="20060209103815773.jpg" width=50>沓</TD>
<TD width=50 height=54><IMG height=50 src="20060209103815156.jpg" width=50>大</TD>
<TD width=50 height=54><IMG height=50 src="20060209103816767.jpg" width=50>大</TD>
<TD width=50 height=54><IMG height=50 src="20060209103816935.jpg" width=50>汏</TD>
<TD width=50 height=54><IMG height=50 src="20060209103816815.jpg" width=50>带</TD>
<TD width=50 height=54><IMG height=50 src="20060209103817315.jpg" width=50>带</TD>
<TD width=50 height=54><IMG height=50 src="20060209103817172.jpg" width=50>带</TD>
<TD width=50 height=54><IMG height=50 src="20060209103818737.jpg" width=50>丹</TD>
<TD width=50 height=54><IMG height=50 src="20060209103818438.jpg" width=50>单</TD>
<TD width=50 height=54><IMG height=50 src="20060209103818185.jpg" width=50>单</TD>
<TR>
```

The info we want is in tag **"TD"**, the text and the image.

### Python Code
Using lib **"BeatifulSoup"** and **"Request"**

Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work.

Requests is an open source Python HTTP library based on the Apache2 protocol.

Import the lib
```python
from bs4 import BeautifulSoup
import requests
import os
```



```python
class Orcale_Picture:
    def __init__(self):  # initialize the calss
        self.headers = {
            # setting a header like Chrome browser
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64)'
                          ' AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'}
        self.web_url = 'http://9610.com/jiagu/bian/def.htm#d'   # the scraping web url
        self.folder_path = 'C:\\Users\\brynh\\jiagu'  # saving path
        self.primary_url = 'http://9610.com/jiagu/bian/'

    def request(self,url):  
        r = requests.get(url, self.headers)  # sending a GET request to the web page
        r.encoding = 'gbk'
        return r

    def get_pic(self):
        r = self.request(self.web_url)
        all_item = BeautifulSoup(r.text, 'lxml').find_all(name="td")
        self.mkdir(self.folder_path)
        os.chdir(self.folder_path)
        label_list = ["沓"]
        i = 1
        for item in all_item:
            label_list.append(item.get_text())
            # del(word[0])
            for x in label_list:
                if "\n" in x:
                    label_list.remove(x)
        all_img = BeautifulSoup(r.text, 'lxml').select("td > img")
        for img in all_img:
            download_url = img['src']
            self.save_img(self.primary_url+download_url,label_list[i]+'.jpg')
            i += 1

    def mkdir(self,path):
        path = path.strip()
        is_exists = os.path.exists(path)
        if not is_exists:
            print('Creating a folder named', path)
            os.makedirs(path)
            print('Successfully!')
        else:
            print(path, 'The folder is existed')

    def save_img(self,url,name):
        img = self.request(url)
        f = open(name,'ab')
        f.write(img.content) 
        print(name,"saved successfully")
        f.close()

if __name__ == '__main__':
    orcale = Orcale_Picture()
    orcale.get_pic()
```
