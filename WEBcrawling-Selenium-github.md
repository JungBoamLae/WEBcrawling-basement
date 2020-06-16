# WEBcrawling-basement
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup

url = 'https://pjt3591oo.github.io/search'
test_keyword = 'python'

driver = webdriver.Chrome('C:/Users/정범래/Downloads/chromedriver_win32/chromedriver.exe')
driver.get(url)

selected_tag_a = driver.find_element_by_css_selector('input#search-box')
selected_tag_a.send_keys(test_keyword)
selected_tag_a.send_keys(Keys.ENTER)

try:
    soup = BeautifulSoup(driver.page_source, 'html.parser')
    items = soup.find('div', {'class':'search'}).find_all('h3')
    for temp in items:
        title = temp.get_text
        print(title)

except:
    print('error')

'''
1. 셀레니움을 이용해서 'python' 이라고 검색. 
2. beautifulSoup을 이용하여서 해당 결과 리스트들의 제목들만 출력해주세요.
'''
