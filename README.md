# About
The following project uses the operational structure of a website and the Selenium library to scrape data from Foody & ShopeeFood at Ho Chi Minh City
The data Crawled include: Food Store/Restaurant Data, Comment Data, User's Network data.
Data is collected for research and for no other purpose.

Website: ShopeeFood/Foody

Technique: Selenium, Multithreading programming
# Crawl data Restaurant/Food store
There 2 step to crawl data Restaurant/Food store:
- Step 1: Crawl all the link and name of that food store
- Step 2: Crawl details each one
## Step 1: Get the list of Food Store:
The food Store of ShopeeFood is demonstrated like this:
![shopeefood](https://github.com/ToanToan110/WebCrawling/assets/64849001/0af08dbd-ae51-4a83-9316-b3471d55e6f3)

We use this function to crawl:
```python
def CrawlFoodData():
    all_web_item = driver.find_elements(By.CSS_SELECTOR,".item-restaurant .item-content")
    result = pd.DataFrame(columns=['name_res', 'img_link', 'food_link','favorite_tag', 'is_quality_mer','address','promotion'])
    for i in all_web_item:
        try:
            
            wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '.info-restaurant .name-res')))
            name_res = i.find_element(By.CSS_SELECTOR, '.info-restaurant .name-res').text
            
            wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '.img-restaurant img')))
            link_img = i.find_element(By.CSS_SELECTOR, '.img-restaurant img').get_attribute("src")
            
            food_link = i.get_attribute("href")
            
            wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '.img-restaurant')))
            favorite_tag = i.find_element(By.CSS_SELECTOR, '.img-restaurant').text
            
            wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '.info-restaurant .icon.icon-quality-merchant')))
            try:
                is_quality_mer = i.find_element(By.CSS_SELECTOR, '.info-restaurant .icon.icon-quality-merchant').get_attribute('title')
            except NoSuchElementException:
                is_quality_mer = np.nan
            
            wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '.info-restaurant .address-res')))
            address_res = i.find_element(By.CSS_SELECTOR, '.info-restaurant .address-res').text

            wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, '.info-restaurant .content-promotion')))
            promotion = i.find_element(By.CSS_SELECTOR, '.info-restaurant .content-promotion').text
        except:
            print("Error at item: ", all_web_item.index(i+1))

        record = pd.DataFrame([[name_res, link_img, food_link, favorite_tag, is_quality_mer, address_res, promotion]]
                              ,columns=['name_res', 'img_link', 'food_link','favorite_tag', 'is_quality_mer','address','promotion'])
        result = pd.concat([result, record])
    return result
```
The output contain:
- name_res: name of restaurant/food store
- img_link: the image url food store
- favorite_tag: whether the this place is liked or not
- is_quality_mer: whether the this place is quality merchant or not
- promotion: typicaly promotion of this food store
## Step 2:
With each food store we crawled from step 1, we will crawl detail each of that in Foody

The structure of each food store in foody looks like:
![foody](https://github.com/ToanToan110/WebCrawling/assets/64849001/ef85ed2d-a7b5-4778-acb3-5c45a50015dd)

The ouput:
![output2](https://github.com/ToanToan110/WebCrawling/assets/64849001/8d6a9bac-e581-431b-acf4-4d377a0af567)

# Crawl data Comment
With each food store we crawled, we will crawl comment each of that.

The structure of 1 comment looks like:
![comment](https://github.com/ToanToan110/WebCrawling/assets/64849001/467a1f13-86cf-4e10-8471-1800260e492b)

And the ouput looks like:

![output3](https://github.com/ToanToan110/WebCrawling/assets/64849001/6c188eea-a955-44ea-a2ef-0b90986d8957)

# Crawl data User

The structure of 1 user's network looks like:
![user](https://github.com/ToanToan110/WebCrawling/assets/64849001/0d9c01c3-ca9b-4e25-bbd5-58eaa6e69d02)

And the ouput looks like:

![output4](https://github.com/ToanToan110/WebCrawling/assets/64849001/57cc0e41-147d-4c06-87e9-5ea9d77692a0)

