# WEBcrawling-basement

from urllib.request import urlopen 
from bs4 import BeautifulSoup 

import re

temp_list = []

html = urlopen('http://en.wikipedia.org/wiki/Kevin_Bacon')
bs = BeautifulSoup(html, 'html.parser')

for links in bs.find('div', {'id':'bodyContent'}).find_all('a', href = re.compile('(^/wiki/).*(\.jpg)')):
    if 'href' in links.attrs:
        print(links.attrs)
        print('===============================')
    for temp in links.next_siblings:
        print('-------------------------------')
        try:
            print(temp.get_text())
        except AttributeError :
            print('Error')
        else : 
            temp = str('href: ') + links.attrs['href'] + str('/description') + temp.get_text()
            print (temp)
            temp_list.append(temp)
    
print('===============================')
print(temp_list[0])
print('===============================')
print(temp_list[1])
print('===============================')
print(temp_list[2])
    


"""
1.  BodyContent 안에 있는 a 태그를 가져오세요.  A태그의 href 속성의 이름들은 /wiki/ 로 시작하고 , .jpg 로 끝나는 속성들만 가져오세요. 
  
힌트 – find, findAll , 정규식 사용.



2.  가져온 확장자 jpg 의 내용 설명(description) 을 get_text() 매소드를 이용해서 가져오세요.
  
힌트 - .jpg의 설명은 .jpg 의 바로 아래의 div 태그안에 있어요 , next_siblings 를 사용하세요



3.  발생한 문제에 대해서 예외처리를 수행하여 프로그램이 정상적으로 돌도록 해주세요. 
  
힌트  - AttributeError 에러 
"""
