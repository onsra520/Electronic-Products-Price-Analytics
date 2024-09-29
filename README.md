# Web Scraping with Python

```python
import os
import requests
from bs4 import BeautifulSoup
```
- ***os***: Thư viện này dùng để tương tác với hệ điều hành. Trong đoạn code này chưa sử dụng thư viện này.

- ***requests***: Thư viện dùng để gửi yêu cầu HTTP và nhận phản hồi từ một trang web. Nó thường được sử dụng để lấy mã nguồn HTML của trang.

- ***BeautifulSoup***: Được sử dụng để phân tích cú pháp HTML và XML, giúp dễ dàng trích xuất thông tin từ trang web.

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
    print(f"{href}")
```

- ***Soup.find_all("a", class_="multiple-link")***: Để tìm cả các thẻ **\<a>** có **class: "multiple-link"**.

  -  Các loại Tags trong html: <a href=""> Click Here!!!
- 