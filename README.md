# WEBcrawling-basement
from selenium import webdriver
from urllib.request import urlopen
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup
from msvcrt import getch
import requests as rq
import re

count = 0
data_list = []
keyword_list = ['한일관계','코로나','경제지표','한일관계개선']

def init():
    del data_list[:]

url = 'https://naver.com'

driver = webdriver.Chrome('C:/Users/정범래/Downloads/chromedriver_win32/chromedriver.exe')

for keyword in keyword_list:
    
    init()
    count += 1
    print(count) 
    print("===================================") 
 
    driver.get(url)
    
    selected_tag = driver.find_element_by_css_selector('input#query.input_text')
    selected_tag.send_keys(keyword)
    selected_tag.send_keys(Keys.ENTER)
    
    html = driver.page_source
    bs = BeautifulSoup(html, 'html.parser')
    item = bs.find('li',{'class':'lnb4'})
    print("-----========================-----")
    
    selected_tag2 = driver.find_element_by_css_selector('li.lnb4 a.tab')
    selected_tag2.send_keys(Keys.ENTER)
    
    html2 = driver.page_source
    bs2 = BeautifulSoup(html2, 'html.parser')
    
    count = 0
    for temp in bs2.find('ul',{'class':'type01'}).find_all('li', id = re.compile('^(sp_nws).*')):
        count = count + 1
        print("-----========================-----")
        try:
            temp_title = temp.dl.find('a')
            temper = temp_title.attrs['title']
            #print(temper)
            data_list.append(temper)
            #print(data_list)
            print("------------------------------++++----")
        except Exception as e:
            print(e)
    for temper in data_list:
        print(temper)    
        print(data_list)
        print("-----------------------------------")
driver.close()
