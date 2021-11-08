# youtube_python

# 유튜브 장르 중 어느 장르가 가장 구독자수 대비 조회수가 많은 장르인지를 알아봄
http://localhost:8888/notebooks/Desktop/bigdata_python/bigdata_python/k-project/music.ipynb


pip install lxml
pip install selenium
pip install bs4
pip install seaborn

from bs4 import BeautifulSoup
from urllib.request import urlopen

import request as rq
from selenium.webdriver import Chrome

driver = Chrome('../chromedriver.exe)

url = 'https://kr.noxinfluencer.com/youtube-channel-rank/top-100-all-film%20%26%20animation-youtuber-sorted-by-avgview-weekly'
driver = Chrome('../chromedriver.exe')
driver.get(url)
soup = BeautifulSoup(driver.page_source, 'lxml')
print(soup)


# 구독자수 구하기
people_data = []

people = soup.select('span.number')
# print(len(people))

print("구독자 수")

nu = 0

for j in people :
    if nu % 3 == 1 :
        print(j.text)
        people_data.append(j.text)
        nu += 1
    else :
        nu += 1
        
        
# 평균 조회수 구하기
view_data = []
people = soup.select('span.number')

print("평균 조회수")
num = 0

for i in people :
    if num % 3 == 2 :
        print(i.text)
        view_data.append(i.text)
        num += 1
    else :
        num += 1
        
        
        
import pandas as pd
import numpy as np     ##행렬, RANGE에 소수점도 가능
import matplotlib.pyplot as plt    #시각화
import seaborn as sns     #시각화 라이브러리, 그래프가 좀 더 이쁨, 통계적 그래프를 좀 더 많이 지원함

#scikit learn
from sklearn.model_selection import train_test_split   ## 훈련데이터 / 테스트형 데이터 랜덤하게 분리 75:25 or 80:20 (훈련데이터가 최대로 확보되는 것이 좋다)
from sklearn.linear_model import LinearRegression      ## 선형회귀 
from sklearn.metrics import mean_squared_error         ## 오차 감사


# 데이터 전처리

people_data2 = []

for i in people_data:
    
    if i[-1] == "만" :
        j = i.replace("만", "")
        j = float(j)
        
    elif i[-1] == "억" :
        j = i.replace("억", "")
        j = float(j)* 10000
    else :
        j = float(i)    
        
    people_data2.append(j)
    
print(people_data2)
#단위 : 만


# 데이터 전처리2
view_data2 = []

for i in view_data:
    
    if i[-1] == "만" :
        j = i.replace("만", "")
        j = float(j)
        
    elif i[-1] == "억":
        j = i.replace("억","")
        j = float(j)*10000
        
    else :
        j = float(i)
        print(j)
        
    view_data2.append(j)
print(view_data2)


df = pd.DataFrame({'People' : people_data2,
                  'View' : view_data2})
                  
df.plot(kind = 'scatter', x = 'People', y = 'View',
        c = 'coral', s= 10, figsize = (10,5)) 
plt.show()      # = print
plt.close()

sns.regplot(x = 'People', y = 'View', data = df) #회귀선 표시      
plt.show()
plt.close()

x = df[['People']] # 독립 변수 x
y = df[['View']] # 종속 변수 y

lr = LinearRegression()
lr.fit(x, y)
# 회귀식의 기울기
lr.coef_
# 회귀식의 y절편
lr.intercept_

