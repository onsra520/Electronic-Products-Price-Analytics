## Thư viện ***os***
```python
import os
```
Thư viện chuẩn của Python giúp tương tác với hệ điều hành. Các chức năng chính bao gồm xử lý các thao tác liên quan đến tệp và thư mục như:

    - os.listdir(): liệt kê các tệp trong một thư mục.

    - os.mkdir(): tạo thư mục mới.

    - os.makedirs(): tạo một hoặc nhiều thư mục
  
    - os.remove(): xóa tệp.

    - os.path.join(): kết hợp đường dẫn.

## Thư viện ***shutil***
```python
import shutil
```
Thư viện hỗ trợ các thao tác cấp cao hơn với tệp và thư mục, thường dùng để sao chép, di chuyển hoặc xóa thư mục.

    - shutil.copy(): sao chép tệp từ vị trí này sang vị trí khác.

    - shutil.move(): di chuyển tệp hoặc thư mục.

    - shutil.rmtree(): xóa toàn bộ thư mục và tất cả nội dung bên trong.

## Thư viện ***glob***
```python
import glob
```
Thư viện dùng để tìm kiếm các tệp và thư mục theo mẫu tên tệp (pattern). Rất hữu ích khi bạn muốn liệt kê các tệp phù hợp với một mẫu cụ thể (ví dụ: tất cả các tệp .txt trong một thư mục).

    - glob.glob('*.txt'): tìm tất cả các tệp có phần mở rộng .txt trong thư mục hiện tại.

## Thư viện ***pandas***
```python
import pandas
```
Thư viện mạnh mẽ dùng cho phân tích dữ liệu, chủ yếu là xử lý dữ liệu dạng bảng (DataFrame). Một số chức năng chính:

    - pd.read_csv(): đọc dữ liệu từ tệp CSV và chuyển thành DataFrame.

    - DataFrame.head(): hiển thị các dòng đầu tiên của DataFrame.

    - DataFrame.describe(): cung cấp các thống kê tổng hợp cho các cột số học trong DataFrame.


## Thư viện ***requests***
```python
import requests
```
Thư viện phổ biến dùng để gửi các yêu cầu HTTP và nhận phản hồi từ các trang web hoặc API. Hỗ trợ các phương thức như GET, POST, PUT, DELETE,... Một số phương thức phổ biến:

    - requests.get(url): gửi yêu cầu HTTP GET đến url và nhận phản hồi.
  
    - requests.post(url, data): gửi dữ liệu tới url thông qua HTTP POST.
  
    - requests.status_code: lấy mã trạng thái của phản hồi .

## Thư viện ***BeautifulSoup*** (từ thư viện ***bs4***)
```python
from bs4 import BeautifulSoup
```
Công cụ phân tích và xử lý HTML/XML. Dùng để trích xuất dữ liệu từ trang web bằng cách phân tích cú pháp HTML. Một số chức năng phổ biến:

    - BeautifulSoup(html_content, 'lxml'): tạo một đối tượng BeautifulSoup từ nội dung HTML.
 
    - soup.find(): tìm thẻ HTML đầu tiên phù hợp với điều kiện.
 
    - soup.find_all(): tìm tất cả các thẻ HTML phù hợp với điều kiện.

## Thư viện ***time***
```python
import time
```
Thư viện xử lý thời gian, hỗ trợ các chức năng như:
time.sleep(seconds): tạm dừng chương trình trong một khoảng thời gian xác định.

    - time.time(): trả về thời gian hiện tại tính bằng giây từ mốc thời gian "epoch" (1/1/1970).
  
    - time.strftime(): định dạng thời gian thành chuỗi với các định dạng khác nhau.

## Thư viện ***selenium***
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait 
```
Thư viện tự động hóa các trình duyệt web, hỗ trợ thao tác với các trang web giống như con người làm. Cấu trúc cơ bản bao gồm các lớp webdriver để điều khiển trình duyệt.

    - webdriver.Edge(): mở trình duyệt Edge.
  
    - WebDriverWait(driver, timeout): chờ đợi một điều kiện xảy ra (ví dụ, một thẻ HTML xuất hiện).
  
    - driver.get(url): truy cập trang web.
  
    - find_element(By.CLASS_NAME, 'class_name'): tìm phần tử HTML dựa trên tiêu chí.
***By*** và ***EC***

    - By là một lớp trong selenium.webdriver.common.by, cung cấp các cách định vị phần tử HTML dựa trên ID, tên lớp, tên thẻ, v.v.

    - EC (Expected Conditions) giúp kiểm tra các điều kiện, như kiểm tra xem phần tử có hiển thị trên trang hay không, hay là điều kiện trang đã tải xong.





