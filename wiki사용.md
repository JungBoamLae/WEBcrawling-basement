# WEBcrawling-basement
from urllib.request import urlopen
from bs4 import BeautifulSoup
import re
import datetime
import random

random.seed(datetime.datetime.now())

pages = list()

def getLinks(articleUrl):
    html = urlopen('http://en.wikipedia.org{}'.format(articleUrl))
    bs = BeautifulSoup(html, 'html.parser')
    return bs.find('div', {'id':'bodyContent'}).find_all('a', href=re.compile('^(/wiki/)((?!Category).)*$'))

links = getLinks('/wiki/Kevin_Bacon') 

while len(links) > 0: 
    newArticle = links[random.randint(0, len(links)-1)].attrs['href']
    if newArticle not in pages:
        pages.append(newArticle)
        #print(newArticle)
        print(pages)
        links = getLinks(newArticle)
        
"""
1. 랜덤하게 태그를 타고 들어감 , 중복된 페이지는 또 다시 들어가면 안됨.
2. 랜덤함수의 시드는 오늘의 날짜값을 인풋 파라미터로 받아야함.
3. 위키 페이지에서 읽어오는 태그는 bodyContent 안에 있어야 하고 , 정규식을 이용하여서 ‘/wiki/’ 가 가장 앞에 오되 웹 페이지 안에 ‘Category’ 라는 문장이 들어가 있으면 안됨
/wiki/Kevin_Bacon
"""
