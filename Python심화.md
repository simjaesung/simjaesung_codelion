# 1. 실시간 검색어 확인하기
## 크롤링?
- Crawler의 뜻 -> 기는것, 파충류

- Crawling은 기어다닌다~라고 해석하면 편할것같고

  - Web Crawler은 즉 웹페이지 내부를 기어다니면서 데이터를 모아주는 소프트웨어라고 생각하자!

**requsets.get(url)** -> GET 요청을 보내는 기능

이 기능을 사용하기 위해서는 외부모듈를 requests를 불러야함

<code>
<pre>
import requests
url = "http://www.daum.net"
print(requests.get(url)) #Client의 요청
</pre>
</code>

**출력결과는 <Response[200]>로 #Server의 응답**

- 200은 코딩에서 성공이라는 걸 의미하는 숫자로 '응답'이 성공했다는 뜻

<code>
<pre>
response = requests.get(url)
#응답 정보를 response 에 담음, 응답을 하나의 객체로 만들었다고 생각하자

response.text #html의 코드를 모두 가져온다
</pre>
</code>

---
## html에서 가져온 코드를 이쁘게 출력해야함 !
### BeautifulSoup을 사용하는데 이는 모듈이름이 아님 모듈안에 있는 기능임

<code>
<pre>
from bs4 import BeautifulSoup
print(type(response.text))
print(type(BeautifulSoup(response.text, 'html.parser')))
</pre>
</code>

### 출력결과는
class 'str'<br>
class 'bs4.BeautifulSoup'<br>
로 두 결과물의 타입이 완전 다르다

### 파라미터
BeautifulSoup(데이터,파싱방법)<br>
데이터 - HTML XML<br>
파싱 - 우리의 데이터를 의미있게 변경<br>

### 기능
BeautifulSoup(response.text, 'html.parser')는 문자열이였던 타입을 어떠한 다른 타입으로 변환시켜주는데,<br>
BeautifulSoup은 정보를 담는 통으로 보면 편하겠다.<br>
이 통안에는 칸막이가 있어 response.text를 가지런히 정리해준다
<code>
<pre>
soup = BeautifulSoup(response.text, 'html.parser')
soup.title #title태그 자체를 가져옴
soup.title.string #태그를 없앤 문자열만 가져옴
soup.span #상단 span태그 하나만 출력함
soup.findAll('span') #모든 span태그를 가져온다
</pre>
</code>

### 실시간 검색어 부분만 추출하기
원하는 부분의 데이터를 추출하기 위해서는 이들만의 공통점을 찾아야되는데
태그이름, 클래스이름으로 접근하면 좋다.<br>
**이 코드의 실시간 검색어들은 a태그와 link-favorsch라는 클래스명을 가지고 있다!!**

<code>
<pre>
soup.findAll("a") 
#html 문서에서 모든 a태그를 가져오는 코드

soup.findAll("a","link_favorsch") 
#html 문서에서 a태그이면서 클래스명이 link_favorsch인 태그들을 가져오기
</pre>
</code>

### findAll은 리스트형태로 리턴해주는데 이를 for문으로 접근하여 사용하면 편하겠다.

### .get_text()를 사용하여 text만 뽑아 출력해주면 실시간 검색어 크롤링 성공

---
## 날짜기능
<code>
<pre>
import datetime #외부 모듈을 사용해야함
datetime.today()

#초까지의 세세한 날짜가 필요하지 않으면 strftime 함수를 사용해준다!
datetime.today().strftime("%Y년 %m월 %d일의 실시간 검색어 순위입니다.\n")
#이와 같은 형식으로 필요한 날짜정보만 불러올수있다.
</pre>
</code>

---
## 파일오픈, 모드는 w,r,a 3가지
<code>
<pre>
search_rank_file = open("rankresult.txt","w")
search_rank_file.write #txt에 내용작성
</pre>
</code>

---
## 크롤링 방지 사이트에 대한 대처
headers = {'User-Agent':'Mozilla/5.0 (Windows NT 6.3; Win64; x64) 
AppleWebKit/537.36 (KHTML, like Gecko) 
Chrome/63.0.3239.132 Safari/537.36'}<br>
위와 같이 headers에 우리의 정보를 담아<br>
**requests.get(url,headers=headers)**,요청을 보낼때 같이 보내준다<br><br>

# 날씨 정보 받아오기
### API key 발급받기
- ### API?
    - Application Programming Interface
    - 프로그램과 프로그램을 이어주는 인터페이스라고 생각하자!

### 이미 만들어져있는 API를 이용해 실시간 날씨 정보를 출력 가능

### API key?
- 랜덤문자들로 구성된 API를 이욯할때 나를 명시할수있음

### API링크 만들기
- 사이트에서 링크를 가져와 API가 요구하는 파라미터를 채워준다

- 이때 사용하는건 fstring함수
    1. 사용할 문자열앞에 f라고 작성
    2. {}안에 바꾸고자하는 문자열이 담긴 변수를 넣어으면 그 형태로 바뀐다<br>
    
- api주소는 ?를 기준으로 나눈다 ?이전에는 공통url, 후는 파라미터

### requests모듈을 통해 api에 요청을 보낸다 !
 - requests.get(api)
 - 받은 응답을 text로 출력하면 dictionary형태의 문자열이 출력이된다!
    <br>이를 사전형태로 바꿔주면 좋을 것 같다. . . . . . 

### import json
 - json-> 자바스크립트 오브젝트 노테이션 , 데이터를 주고 받을때 사용하는 포맷이다 !

 - json 포맷으로 받아온 응답값을 쓰기 좋은 형태로 응답받음

 - 일반 문자열을 json형태로 바꾸는 방법 -> json.loads(result.text)<br>
    result.text는 <class 'dict'>으로 변경된다!

### 이제 dict의 key값으로 value를 얻어내어 원하는 정보를 출력하면된다
---

# 번역하기
## googletrans의 2기능

1. 언어감지
2. 번역
<code>
<pre>
from googletrans import Translator
translator = Translator()
</pre>
</code>

### 언어 감지<br>
1. 번역기 만들기
2. 언어감지를 원하는 문장 설정
3. 언어를 감지

### detect
 - 언어 감지, lang값과 confidence값을 돌려줌
 - detect.lang을 통해 lang의 정보만 출력할 수있음
<code>
<pre>
print(detected.lang,":",sentence)
#ko : sentence
</pre>
</code>

### 번역하기
 - translate(text, dest, src)
 - 파라미터는 각각 (번역을 원하는 문장, 어떤언어로?, 소스재료(생략가능))
- ex) translator,translate(sentence, ['ko','en'등등....])
<code>
<pre>
result = translator.translate(sentence,'en')

print(result.dest,":",result.text)
#en : 번역된 문장 출력 !
</pre>
</code>

# 메일 보내기