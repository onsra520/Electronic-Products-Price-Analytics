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

---

```python
os.makedirs(os.path.join("Cellphones Data","Dataset"),exist_ok = True)
```

- ***os.path.join("Cellphones Data","Dataset")*** - Tạo ra đường dẫn **/Cellphones Data/Dataset** 

- ***os.makedirs(os.path.join("Cellphones Data","Dataset"),exist_ok = True)***: Đầu tiên tạo Folder có tên là **Cellphones Data** nếu đã tồn tại thì bỏ qua nếu **exist_ok = True**, sau đó tạo thư mục con có tên là **"Dataset"** nếu có thì bỏ qua.

---

```python
def Scrape_Product(URLs, Name):
    driver = webdriver.Edge()

    try:
        URL = URLs
        driver.get(URL)
        
        def close_popups():
            while True:
                try:
                    close_button = WebDriverWait(driver, 2).until(
                        EC.element_to_be_clickable((By.CSS_SELECTOR, "button.modal-close.is-large"))
                    )
                    close_button.click()
                    time.sleep(0.5)
                except Exception:
                    break

            while True:
                try:
                    close_promo_button = WebDriverWait(driver, 2).until(
                        EC.element_to_be_clickable((By.CSS_SELECTOR, "button.cancel-button-top"))
                    )
                    close_promo_button.click()
                    time.sleep(0.5)
                except Exception:
                    break
        
        while True:
            try:
                close_popups()            
                show_more_button = WebDriverWait(driver, 2).until(
                    EC.element_to_be_clickable((By.CSS_SELECTOR, "a.button.btn-show-more.button__show-more-product"))
                )
                show_more_button.click()
                time.sleep(2)
            except Exception as e:
                break

        Page = driver.page_source
        Soup = BeautifulSoup(Page, "lxml")
        
        # Tìm tất cả tên và giá sản phẩm
        Product_Names = Soup.find_all("div", class_="product__name")
        Product_Prices = Soup.find_all("p", class_="product__price--show")

        Product_Information = {}

        # Kiểm tra xem số lượng tên sản phẩm và giá có khớp không
        if len(Product_Names) == len(Product_Prices):
            for name, price in zip(Product_Names, Product_Prices):
                product_name = name.get_text(strip=True)
                product_price = price.get_text(strip=True).replace("₫", "").replace(".", "").strip()
                Product_Information[product_name] = product_price
        else:
            print("Số lượng tên sản phẩm và giá sản phẩm không khớp!")

        for product, price in Product_Information.items():
            print(f"Tên sản phẩm: {product} - Giá: {price}")

    finally:
        # Đóng trình duyệt
        driver.quit()
    
    df = pd.DataFrame(list(Product_Information.items()), columns=["Tên sản phẩm", "Giá"])
    df.to_csv(f"{Name}.csv", header=True, index=False)
```

---

### Định nghĩa hàm
```python
def Scrape_Product(URLs, Name):
```
  - **URLs**: Đường dẫn của trang web cần thu thập dữ liệu.
  - **Name**: Tên cho tệp CSV sẽ được lưu, chứa thông tin sản phẩm.

### Khởi tạo WebDriver
```python
driver = webdriver.Edge()
```
  - Khởi tạo trình duyệt Microsoft Edge để thực hiện việc tự động hóa web.

### Thực thi mã trong khối try
```python
try:
  URL = URLs
  driver.get(URL)
```
  - Mở trang web tại URL đã cho.

### Hàm **close_popups**
```python
def close_popups():
    while True:
      try:
        close_button = WebDriverWait(driver, 2).until(
            EC.element_to_be_clickable((By.CSS_SELECTOR, "button.modal-close.is-large"))
        )
        close_button.click()
        time.sleep(0.5)
      except Exception:
        break 

    while True:
      try:
          close_promo_button = WebDriverWait(driver, 2).until(
            EC.element_to_be_clickable((By.CSS_SELECTOR, "button.cancel-button-top"))
        )
        close_promo_button.click()
      except Exception:
        break 
```

- **Mục đích**: Đóng các cửa sổ pop-up hoặc các thông báo khuyến mãi (promo) có thể xuất hiện khi trang web được tải. Đây là một phần quan trọng khi thu thập dữ liệu từ các trang web, bởi vì các pop-up có thể che khuất nội dung mà bạn cần lấy hoặc làm gián đoạn quá trình tự động hóa.
#### Giải thích chi tiết từng phần:
- Vòng lặp đầu tiên - Đóng cửa sổ Pop-up chính:

```python
while True:
    try:
        close_button = WebDriverWait(driver, 2).until(
            EC.element_to_be_clickable((By.CSS_SELECTOR, "button.modal-close.is-large"))
        )
        close_button.click()
        time.sleep(0.5)
    except Exception:
        break
```

- **while True**: Vòng lặp liên tục này sẽ chạy cho đến khi không còn pop-up nào để đóng.
  
- **WebDriverWait(driver, 2)**: Sử dụng Selenium để chờ tối đa 2 giây cho đến khi nút đóng pop-up có thể nhấn được. Đây là một chiến lược đợi để đảm bảo nút xuất hiện.
  
- **EC.element_to_be_clickable**: Kiểm tra xem phần tử có khả năng tương tác (clickable) hay không. Trong trường hợp này, nó kiểm tra nút có **CSS selector** là **"button.modal-close.is-large"**.

- **close_button.click()**: Nếu tìm thấy nút đóng, Selenium sẽ nhấn vào nút để đóng cửa sổ pop-up.

- **time.sleep(0.5)**: Tạm dừng chương trình 0.5 giây sau khi nhấn nút, nhằm đảm bảo quá trình đóng diễn ra mượt mà trước khi tiếp tục vòng lặp.

- **except Exception**: Nếu không thể tìm thấy nút hoặc có bất kỳ lỗi nào khác, chương trình sẽ bắt ngoại lệ và thoát khỏi vòng lặp (break). Điều này xảy ra khi không còn pop-up nào để đóng.

#### Vòng lặp thứ hai - Đóng cửa sổ khuyến mãi (promo):
```python
while True:
    try:
        close_promo_button = WebDriverWait(driver, 2).until(
            EC.element_to_be_clickable((By.CSS_SELECTOR, "button.cancel-button-top"))
        )
        close_promo_button.click()
        time.sleep(0.5)
    except Exception:
        break

```
- **Mục đích**: Tương tự như vòng lặp trước, nhưng vòng lặp này nhắm vào các cửa sổ khuyến mãi (promo) với nút đóng có **CSS selector** là **"button.cancel-button-top"**.
- Các bước xử lý cũng tương tự như vòng lặp đầu tiên:
  - Sử dụng *WebDriverWait* để chờ nút promo xuất hiện.
  - Nhấn nút để đóng cửa sổ promo.
  - Nếu không tìm thấy hoặc gặp lỗi, thoát khỏi vòng lặp.

### Vòng lập chính
```python
while True:
    try:
        close_popups()
        show_more_button = WebDriverWait(driver, 2).until(
            EC.element_to_be_clickable(
                (By.CSS_SELECTOR, "a.button.btn-show-more.button__show-more-product")
            )
        )
        show_more_button.click()
        time.sleep(2)
    except Exception as e:
        break
```
#### 1. Vòng lặp vô hạn **while True**

- Chạy cho đến khi bị dừng lại bởi một *điều kiện* (ở đây là thông qua câu lệnh break trong khối except). Vòng lặp này đảm bảo rằng mã sẽ liên tục *kiểm tra* và *nhấn* vào nút **"Hiển thị thêm"** cho đến khi nút đó không còn xuất hiện trên trang. 
  
#### 2. Đóng popup với hàm ***close_popups()***
   
- Mỗi khi vòng lặp bắt đầu, hàm **close_popups()** được gọi để kiểm tra và đóng bất kỳ popup quảng cáo hay thông báo nào có thể xuất hiện trong quá trình tải trang. Điều này cần thiết để tránh việc popup cản trở thao tác với các phần tử trên trang.
  
- Hàm **close_popups()** bao gồm hai vòng lặp lồng nhau, mỗi vòng kiểm tra một loại popup cụ thể (advertisement và promo). Các popup này sẽ được nhấn *"đóng"* nếu chúng xuất hiện.

#### 3. Xử lý nút **"Hiển thị thêm"**
```python
show_more_button = WebDriverWait(driver, 2).until(
    EC.element_to_be_clickable(
        (By.CSS_SELECTOR, "a.button.btn-show-more.button__show-more-product")
    )
)
show_more_button.click()
time.sleep(2)
```
- **Mục đích**: Tương tự như Hàm **close_popups()**, điều kiện mà Selenium sẽ đợi là nút có thể nhấn được. Nhắm vào nút có **CSS selector** là **"a.button.btn-show-more.button__show-more-product"**, đại diện cho nút *"Hiển thị thêm"* trên trang.

- **time.sleep(2)**: Tạm dừng chờ trang tải thêm dữ liệu.

#### 4. Khối **except**
```python
except Exception as e:
    break
```
- Nếu có bất kì lỗi nào trong quá trình *nhấn nút* ***"Hiển thị thêm"*** (ví dụ: nút không còn tồn tại trên trang, trang đã tải hết sản phẩm, hoặc xảy ra lỗi mạng), thì vòng lặp sẽ dừng lại bởi câu lệnh **break**. Đồng nghĩa với việc tất cả sản phẩm đã được tải.

### Phân tích HTML với BeautifulSoup và Tìm kiếm thông tin sản phẩm
```python 
Page = driver.page_source
Soup = BeautifulSoup(Page, "xlmx")

Product_Names = Soup.find_all("div", class_="product__name")
Product_Prices = Soup.find_all("p", class_="product__price--show")
```
- **driver.page_source**: là thuộc tính của đối tượng driver (được khởi tạo từ Selenium WebDriver). Nó trả về toàn bộ mã nguồn HTML của trang web hiện tại mà trình duyệt đang hiển thị.
  > Ở đây nó sẽ trả về **Source Code** của trang ***Cellphones***

- **BeautifulSoup(Page, "lxml")**: Đây là cách sử dụng thư viện *BeautifulSoup* từ gói *bs4* để phân tích (parse) mã HTML lấy từ **Page**
  >**HTML parsing** là quá trình đọc mã HTML từ trang web và trích xuất các thành phần cụ thể từ đó, như thẻ, thuộc tính, và nội dung của trang.

- **"lxml"**: Đây là trình phân tích cú pháp (parser) mà BeautifulSoup sử dụng để đọc và hiểu mã HTML. 
  ```python
  !pip install lxml # Cài đặt HTML parsing
  ```
- **Soup.find_all("div", class_="product__name")**
  
  - **Soup.find_all()**: Đây là một phương thức của BeautifulSoup giúp tìm tất cả các thẻ HTML khớp với tiêu chí tìm kiếm.

  - **"div"**: Tên của thẻ HTML cần tìm, trong trường hợp này là thẻ *\<div>*.

  - **class_="product__name"**: Đây là điều kiện để tìm thẻ *\<div>* có lớp CSS (class) là **product__name**.

- **Soup.find_all("p", class_="product__price--show")**
  - Tương tự như câu lệnh trên, nhưng ở đây bạn đang tìm các thẻ *\<p>* có class là **product__price--show**.

### Kiểm tra và lưu thông tin
```python 
Product_Information = {}

if len(Product_Names) == len(Product_Prices):
    for name, price in zip(Product_Names, Product_Prices):
        product_name = name.get_text(strip=True)
        product_price = (
            price.get_text(strip=True).replace("₫", "").replace(".", "").strip()
        )
        Product_Information[product_name] = product_price
else:
    print("Số lượng tên sản phẩm và giá sản phẩm không khớp!")

for product, price in Product_Information.items():
    print(f"Tên sản phẩm: {product} - Giá: {price}")

```
- Tạo một từ điển **Product_Information** để lưu tên sản phẩm và giá tương ứng.

- Nếu số lượng tên sản phẩm và giá không khớp, sẽ in ra thông báo lỗi.

- In ra danh sách tên sản phẩm và giá của chúng.

### Đóng trình duyệt
```python
finally:
    driver.quit()
```
- Đảm bảo trình duyệt sẽ được đóng lại sau khi hoàn thành việc thu thập dữ liệu, bất kể có lỗi xảy ra hay không.

### Xuất ra tệp CSV
```python
df = pd.DataFrame(list(Product_Information.items()), columns=["Tên sản phẩm", "Giá"])
df.to_csv(f"{Name}.csv", header=True, index=False)
```
- Tạo một DataFrame từ từ điển Product_Information và xuất ra tệp CSV với tên được cung cấp trong tham số Name.

---

```python
def Scrape_Product_URL():
    URL = "https://cellphones.com.vn/"
    Homepage = requests.get(URL).text
    Soup = BeautifulSoup(Homepage, "lxml")

    Links = Soup.find_all("a", class_="multiple-link")

    Pages = {}
    for link in Links:
        Page_Name = link.get_text(strip=True).replace(",", "")
        Page_URL = link.get("href")
        Pages[Page_Name] = Page_URL

    return Pages

def Scrape_Per_URL():
    Links = Scrape_Product_URL()
    for Num in Links:
        Scrape_Product(Links[Num], Num)
        shutil.move(f"{Num}.csv", os.path.join("Cellphones Data","Dataset"))
Scrape_Per_URL() 
```

### Hàm **Scrape_Product_URL()**
- Lấy URL của từng loại sản phẩm của trang chủ <a href=https://cellphones.com.vn/>Cellphones</a>
  >Điện thoại : https://cellphones.com.vn/mobile.html <p>
  >tablet : https://cellphones.com.vn/tablet.html ...etc...<p>

- Bằng cách tìm toàn bộ ***find_all*** các tags **\<a>** với *class* là **multiple-link** <p>

### Hàm **Scrape_Per_URL()**
- Với mỗi URl được gán trong cho mỗi đối tượng trong *Dict*
  >**Điện thoại** -> https://cellphones.com.vn/mobile.html <p>
  
- Sẽ được đưa vào hàm **Scrape_Product(URLs, Name)**
  
- Sau khi đã có được dữ liệu sẽ di chuyển file csv vào trong Folder có đường dẫn **\Cellphones Data\Dataset**