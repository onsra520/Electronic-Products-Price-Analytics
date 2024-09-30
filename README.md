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

### Hàm **close_popus**
```python
def close_popus():
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








