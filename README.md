# Python_crawler：使用三種Python套件
+ requests
+ beautifulsoup4(抓去html中的tag)
+ re(正規表式法)

## 1.requests
目的：request目標網址並拿到回傳的物件，主要分成二種方法(get & post)
+ get方法：</br>
    step1：宣告一個url(目標網址) url="http://www.cartoonmad.com/comic/1221.html"</br>
    step2：宣告一個head={"Connection":"keep-alive"}(此為一個dic)，為了讓程式像是一般的瀏覽器</br>
    step3：準備好url & headers即可正式requests url，拿回res(此為一物件)，語法為： res=r.get(url,headers=head)</br>
    step4：確認目標網址的編碼方式再encoding此編碼，語法為：res.encoding="big5</br>
    step4：最後將response的結果印出來，語法為：print(res.test)</br>

```python
#request 漫畫名稱，respon
import requests as r
url ="http://www.cartoonmad.com/comic/1221.html"
head={"Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8",
    "Accept-Encoding":"gzip, deflate",
    "Accept-Language":"zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7",
    "Cache-Control":"max-age=0",
    "Connection":"keep-alive",
    "Cookie":"ASPSESSIONIDCSRQBDAR=IIHNKLGBLAANGAGLGLAEMNFJ; __utma=42881207.392990975.1522250582.1522250582.1522250582.1; __utmc=42881207; __utmz=42881207.1522250582.1.1.utmcsr=google|utmccn=(organic)|utmcmd=organic|utmctr=(not%20provided); __utmt=1; chname=%7C%B7s%A5%7C%C5X%A4p%A4l%7C%B6i%C0%BB%AA%BA%A5%A8%A4H; chid=%7C07609%7C01221; __utmb=42881207.10.10.1522250582",
    "Host":"www.cartoonmad.com",
    "Referer":"http://www.cartoonmad.com/",
    "Upgrade-Insecure-Requests":"1",
    "User-Agent":"Mozilla/5.0 (iPhone; CPU iPhone OS 10_3 like Mac OS X) AppleWebKit/602.1.50 (KHTML, like Gecko) CriOS/56.0.2924.75 Mobile/14E5239e Safari/602.1"}

res=r.get(url,headers=head)
res.encoding="big5"
page=res.text
print(res.text)
print(res.status_code)
```
## 2.beautifulsoup
目的：截取html檔中想要的內容
+ 使用方法：</br>
    step1：先掃描需要的html檔案，語法為：soup = BeautifulSoup(page, 'lxml') 其中page即為html的擋案</br>
    step2：用chrome觀察想要截取內容的在哪一個tag中，本範例為找出每一話名稱，與每一話的url</br>
    step3：觀察結果為每一話名稱&url皆在 id為info中的td a裡面，且名稱直接是text，url則是其中的屬性href</br>
    step4：最後再塞進list中為了之後方便使用</br>
```python
from bs4 import BeautifulSoup
chapter_html=[]
soup = BeautifulSoup(page, 'lxml') # html5lib
for index in soup.select('#info td a'):
    chapter=index.text
    chapter_url="http://www.cartoonmad.com"+index["href"]
    #print(index.text)
    #print(index["href"])
    chapter_html.append([chapter,chapter_url])
chapter_html
```
