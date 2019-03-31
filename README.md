# avito_parser_03_2019
Первый парсер авито
import requests
from bs4 import BeautifulSoup as bs

headers = {'accept':'*\*', 'user-agent':'User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36'}

base_url = 'https://www.avito.ru/pavlovskaya/doma_dachi_kottedzhi?s_trg=11'

def avito_parse(base_url, headers):
    jobs = []
    sesssion = requests.Session()
    request = sesssion.get(base_url, headers=headers)
    if request.status_code == 200:
        print('ok')
        soup = bs(request.content, 'html.parser')
        divs = soup.find_all('div', attrs={'data-marker':'item'})
        for div in divs:
           photo = div.find('img', attrs={'src':'//76.img.avito.st/208x156/5099303376.jpg', 'class':'large-picture-img', 'alt':'Дом 67.7 м² на участке 15 сот.'})
           href2 = div.find('a', attrs={'itemprop':'url'} )['href']
           href = 'https://www.avito.ru' + href2
              # print('https://www.avito.ru' + 'href')
           title = div.find('a', attrs={'itemprop':'url'}).text
           price = div.find('span', attrs={'data-marker':'item-price'}).text
           announcement = div.find('p', attrs={'data-marker':'item-address'}).text
           time = div.find('div', attrs={'data-marker':'item-date'})
           jobs.append({
               'photo': photo,
               'href': href,
               'title': title,
               'price': price,
               'announcement': announcement,
               'time': time
           })
        print(len(jobs))

    else:
        print('error ')
avito_parse(base_url, headers)

print()
