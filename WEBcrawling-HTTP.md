# WEBcrawling-basement

from urllib.request import urlopen, urlparse
from urllib.error import HTTPError
from bs4 import BeautifulSoup
import re

allExtLinks = set()
allIntLinks = set()


def getInternalLinks(bs, includeUrl):
    includeUrl = '{}://{}'.format(urlparse(includeUrl).scheme, urlparse(includeUrl).netloc)
    #print('--------------------------------------')
    internalLinks = []
    #Finds all links that begin with a "/"
    for link in bs.find_all('a', href=re.compile('^(/|.*'+includeUrl+')')):
        if link.attrs['href'] is not None:
            if link.attrs['href'] not in internalLinks:
                if(link.attrs['href'].startswith('/')):
                    internalLinks.append(includeUrl+link.attrs['href'])
                else:
                    internalLinks.append(link.attrs['href'])
    return internalLinks
        
        
#Retrieves a list of all external links found on a page
def getExternalLinks(bs, excludeUrl):
    externalLinks = []
    #Finds all links that start with "http" that do
    #not contain the current URL
    print(re.compile('^(http|www)((?!'+excludeUrl+').)*$'))
    for link in bs.find_all('a', href=re.compile('^(http|www)((?!'+excludeUrl+').)*$')):
        if link.attrs['href'] is not None:
            if link.attrs['href'] not in externalLinks:
                externalLinks.append(link.attrs['href'])
    return externalLinks

def getAllExternalLinks(siteUrl):
    try:
        html = urlopen(siteUrl)
        
    except HTTPError as e:
        print('page no found',e)
    
    else:
    
        domain = '{}://{}'.format(urlparse(siteUrl).scheme,
                                  urlparse(siteUrl).netloc)
    
        print(urlparse(siteUrl).scheme)
        print(urlparse(siteUrl).netloc)
    
        bs = BeautifulSoup(html, 'html.parser')
        internalLinks = getInternalLinks(bs, domain)
        externalLinks = getExternalLinks(bs, urlparse(siteUrl).netloc)
        
        for link in externalLinks:
            if link not in allExtLinks:
                allExtLinks.add(link)
                print(link)
        for link in internalLinks:
            if link not in allIntLinks:
                allIntLinks.add(link)
                getAllExternalLinks(link)
   
allIntLinks.add('http://oreilly.com')
getAllExternalLinks('http://oreilly.com')

#https://velog.io/@city7310/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9C%BC%EB%A1%9C-URL-%EA%B0%80%EC%A7%80%EA%B3%A0-%EB%86%80%EA%B8%B0
