# Web Scraping with Python

```python
import os, shutil, glob
import time, requests
import pandas as pd
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
```
- ***Chức năng của các loại thư viện được dùng trong Web Scraping with Python***: <a href="https://github.com/onsra520/Electronic-Products-Price-Analytics/blob/main/Document/Library%20use%20for%20Web%20Scraping%20with%20Python.md"> Click Here!!!</a>

```python
URL = "https://cellphones.com.vn/"
Page = requests.get(URL).text 
Soup = BeautifulSoup(Page, "lxml")
```

- ***URL*** - Đây là địa chỉ URL của trang web mà bạn muốn truy cập, trong trường hợp này là trang chủ của Cellphones. 

- ***requests.get(URL)***: Sẽ gửi một yêu cầu HTTP GET tới trang web Cellphones để lấy nội dung của trang. Nếu thành công, nó sẽ nhận về mã nguồn HTML của trang.

- ***.text***: Lấy phần nội dung HTML của trang web dưới dạng chuỗi văn bản (string). Đây là mã nguồn HTML của toàn bộ trang mà bạn có thể xử lý sau đó.

- ***BeautifulSoup(Page, "lxml")***: Sử dụng BeautifulSoup để phân tích cú pháp HTML của trang chỉ định, ở đây **Page** được chỉ định là *Cellphones*   
  
- ***"lxml"***: là parser (trình phân tích) dùng để xử lý mã HTML.

```python
print(Soup.prettify()) 
```

- ***Soup.prettify()***: Phương thức này định dạng và làm đẹp mã nguồn HTML. Nó sẽ in ra một phiên bản của HTML đã được phân tích với các thụt lề và dòng mới để dễ đọc hơn.


```python
Links = Soup.find_all("a", class_="multiple-link")

for link in Links:
    href = link.get("href")
    text = link.get_text(strip=True).split(',')[0]
    print(f"{text}: {href}")
```

- ***Soup.find_all("a", class_="multiple-link")***: Để tìm cả các thẻ **\<a>** có **class: "multiple-link"**.

  -  Các loại Tags trong html: <a href="https://github.com/onsra520/Electronic-Products-Price-Analytics/blob/main/Document/Tags%20in%20HTML.md"> Click Here!!!</a>
  
- ***link.get('href')***: Lấy giá trị của thuộc tính href -  *Là URL của liên kết*.

- ***link.get_text(strip=True).split(',')[0]***: Lấy nội dung văn bản trong thẻ **\<a>**, loại bỏ khoảng trắng và dấu phẩy bị thừa.   

---
```python
import os
import requests
from bs4 import BeautifulSoup

def Scrape_Product_Type():
    URL = "https://cellphones.com.vn/" 
    Homepage = requests.get(URL).text
    Soup = BeautifulSoup(Homepage, "lxml")

    Links = Soup.find_all("a", class_="multiple-link")

    Pages = {}
    for link in Links:
        Page_Name = link.get_text(strip=True).split(',')[0]    
        Page_URL = link.get("href")
        Pages[Page_Name] = Page_URL  
```
