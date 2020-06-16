# WEBcrawling-basement
from selenium import webdriver
from urllib.request import urlopen
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup

url = 'https://www.naver.com/'
BookList = list()
keword_list = ['python web scraping','python web scraping cookbook','머신러닝 가이드']
#keword_list = ['python web scraping']

for temp in keword_list:
    driver = webdriver.Chrome('C:/Users/정범래/Downloads/chromedriver_win32/chromedriver.exe')
    driver.get(url)
    selected_tag = driver.find_element_by_css_selector('input#query.input_text')
    selected_tag.send_keys(temp)
    selected_tag.send_keys(Keys.ENTER)
    
    soup = BeautifulSoup(driver.page_source, 'html.parser')
    item = soup.find('p',{'class':'title_desc title_desc_v2'})
    
    try:
        html = urlopen(item.a.attrs['href'])
    except:
        print('url open error')
    else:
        soup_2 = BeautifulSoup(html, 'html.parser')
        
    for temp in soup_2.find('ul',{'class':'goods_list'}).children:
        try:
            if '_itemSection' in temp.attrs['class']:
                temp_title = temp.find('a',{'class' : 'link'}).attrs['title']
                temp_price = temp.find('span',{'class' : 'price'}).get_text()
                total_info = str('title : ')+ temp_title + str('\n price : ') + temp_price.lstrip('\n') 
                BookList.append(total_info)

        except Exception as e:
            print(e)
    
    driver.close()

print(BookList)
