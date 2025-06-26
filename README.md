# Python-program-for-searching-a-car-cover


from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
import time
import csv

# Setup Selenium ChromeDriver
service = Service('chromedriver')  # Ensure chromedriver is in your path
driver = webdriver.Chrome(service=service)

# Open OLX search page
url = "https://www.olx.in/items/q-car-cover"
driver.get(url)
time.sleep(5)  # Wait for page to load

# Find all listing elements
items = driver.find_elements(By.CSS_SELECTOR, "li.EIR5N")  # Class used for items

# Open file to save results
with open('olx_car_covers.csv', 'w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)
    writer.writerow(['Title', 'Price', 'Location', 'Link'])

    for item in items:
        try:
            title = item.find_element(By.TAG_NAME, 'h6').text
        except:
            title = "N/A"

        try:
            price = item.find_element(By.CLASS_NAME, '_89yzn').text
        except:
            price = "N/A"

        try:
            location = item.find_element(By.CLASS_NAME, '_2e0PD').text
        except:
            location = "N/A"

        try:
            link = item.find_element(By.TAG_NAME, 'a').get_attribute('href')
        except:
            link = "N/A"

        writer.writerow([title, price, location, link])

print("Scraping completed. Check 'olx_car_covers.csv' for results.")
driver.quit()
