
# Chapter1. 스크래퍼 이해 및 구현

## 1.1 연결
요즘에는 컴퓨터 인터페이스가 발전해서 http://google.com과 같은 페이지를 들어갈 때마다 네트워크에서 무슨 일이 일어나는지 생각할 필요가 없게 되었다.
하지만 웹 스크레이핑은 브라우저 수준에서(HTML, CSS, 자바스크립트) 어떻게 동작하는지 알아야 하고 때로는 네트워크 연결 수준에서 생각해봐야 할 때도 있다.

다음의 예를 보면서 브라우저로 정보를 가져오는 과정에 대해 감을 잡아보자
(밥은 데스크톱 컴퓨터를 가지고 있고 앨리스의 서버에 연결하려 하고 있다)

~~~
ㄱ. 밥의 컴퓨터는 1과 0으로 된 비트 스트림을 보낸다. 
각 비트는 전압으로 구별된다. 이들 비트는 정보를 구성하고, 헤더, 바디도 이런 비트들로 구성이 되어있다. **헤더에는 다음 목표인 밥의 라우터 MAC주소와 최종 목표인 엘리스의 IP주소**가 들어있다. **바디에는 밥이 앨리스의 서버 어플리케이션에 요청하는 내용**이 들어있다.

ㄴ. 밥의 라우터는 이들 비트를 받아 밥의 MAC주소에서 앨리스의 IP주소로 가는 패킷으로 해석한다.
밥의 라우터의 고유IP주소를 패킷에 **'발신자(from)주소'로 기록한** 다음 밥의 라우터는 **이 패킷을 인터넷으로 보낸다.**

ㄷ. 밥의 패킷은 여러 중간 서버를 거치며 이동한다.
중간 서버들은 정확한 물리적 경로 또는 유선 경로를 거쳐 앨리스의 서버를 향해 패킷을 보낸다.

ㄹ. 앨리스의 서버는 자신의 IP 주소에서 그 패킷을 받는다.

ㅁ. 앨리스의 서버는 패킷 헤더에서 포트번호를 찾고 적절한 어플리케이션, 즉 웹 서버 어플리케이션에 보낸다.
포트 번호는 웹 어플리케이션에서는 거의 80이다. IP주소가 몇번지를 의미하는 주소라면, 포트 번호는 패킷데이터의 아파트 동 번호라고 생각하면 된다.

ㅂ. 웹 서버 어플리케이션은 서버 프로세서에서 데이터 스트림을 받는다.  
이 데이터에는 다음과 같은 정보가 들어있다.
 - 이 요청은 GET 요청임
 - 요청하는 파일은 index.html임

ㅅ. 웹서버는 해당하는 HTML파일을 찾고 새 패킷으로 묶어서 자신의 라우터를 통해 밥의 컴퓨터로 전송한다.
웹 서버가 보낸 패킷은 밥이 보낸 패킷과 같은 과정을 거쳐 밥의 컴퓨터에 도달한다.
~~~  


**아래의 간단한 예제로 확인해보자**
~~~python
from urllib.request import urlopen
html = urlopen("http://pythonscrapping.com/pages/page1.html")
print(html.read())
~~~

출력 결과는 http://pythonscrapping.com/pages/page1.html 페이지의 HTML코드 전체이다.
더 정확히 말하자면, 이 출력 결과는 도메인 이름 http://pythonscrapping.com 에 있는 서버의 /pages디렉터리의 HTML파일 page1.html이다.

웹브라우저는 웹페이지를 해석하다가  
~~~html
<img src="cuteKitten.jpg">
~~~ 
위와 같은 태그를 만나면, 페이지를 완전히 렌더링하기 위해 서버에 다시 요청을 보내 cuteKitten.jpg파일의 데이터를 받는다. 위의 코드에는 서버로 돌아가 다른 파일을 요청하는 기능이 없다. 이 스크립트는 우리가 요청한 HTML 파일하나를 읽을 수 있을 뿐이다.

**이 스크립트가 어떻게 html파일을 가져오는 동작을 하는지 살펴보자**  
urllib은 파이썬 표준 라이브러리로 기본적으로 내장되어있고, 웹을 통해 데이터를 요청하는 함수, 쿠키를 처리하는 함수, 심지어 헤더나 유저 에이전트와 같은 메타데이터를 바꾸는 함수도 있다.  
urllib에 대해 자세한 내용은 http://docs.python.org/3/library/urllib.html 을 참고해보자.  
urlopen은 네트워크를 통해 원격 객체를 읽는다. urlopen은 HTML 파일이나 이미지 파일, 기타 파일 스트림을 쉽게 열 수 있는 범용적인 라이브러리이다.
앞으로도 자주 사용하게 될 것이다.  


# 1.2 BeutifulSoup 소개

BeautifulSoup는 HTML에서 원하는 내용을 가져올 수 있도록 도와주는 패키지인데, 이상하거나 잘못된 것들도 이해할 수 있다.
잘못된 HTML을 수정하여 쉽게 탐색할 수 있는 XML형식의 파이썬 객체로 변환해줘서 편리한 부분이 있다.

# 1.2.1 BeutifulSoup 설치

# 1.2.2 BeutifulSoup 실행(간단한 예제로 확인해보자)
BeutifulSoup 라이브러리에서 가장 널리 쓰이는 객체는 BeautifulSoup객체이다.
위에서 작성한 예제에서 가져온 HTML코드를 BeutifulSoup를 통해 파싱해보자.
~~~python
from urllib.request import urlopen
from bs4 import BeutifulSoup
html = urlopen("http://pythonscrapping.com/pages/page1.html")
bsObj = BeutifulSoup(html.read(), "html.parser")
print(bsObj.h1)
~~~

출력결과는 다음과 같다.
~~~html
<h1>An Interesting Title</h1>
~~~

코드는 다음과 같이 동작한다
urllib 라이브러리 임포트, html.read()를 통해서 웹페이지의 html콘텐츠를 얻어오고
이 HTML콘텐츠를 변형한 BeautifulSoup 객체를 가져온다.(앞에서 설명된 바와 같이 html을 쉽개 탐색할 수 있는 xml형식의 파이썬 객체로 변환된다)
변환된 BeutifulSoup객체의 구조는 다음과 같다.
~~~html
html <head>... </head><body>... </body></html>
  head <head><title>A Useful Page</title></head>
    title <title>A Useful Page</title>
  body <body><h1>An Int... </h1><div>Lorem ip... </div></body>
    h1 <h1>An Interesting Title</h1>
    div <div>Lorem Ipsum dolor...</div>
~~~

페이지에서 추출한 <h1\> 태그는 BeutifulSoup 객체 구조(html->body->h1)에서 **두 단계만큼 중첩되어 있다.**
하지만 **우리가 객체에서 가져올 때는 h1태그를 직접 가져올 수 있다.**
~~~python
bsObj.h1
bsObj.html.body.h1
bsObj.body.h1
bsObj.html.h1
~~~

위 코드와 같이 간단한 방법으로 태그에 대한 정보를 추출할 수 있게 도와주는 것이 바로 BeutifulSoup패키지이다.
이 패키지는 앞에서 설명된 바와 같이 HTML파일을 XML파일 형식의 객체로 변형시켜서 어떤 정보를 추출하는 것을 도와준다고 한다.



## 1.2.3 신뢰할 수 있는 연결

웹은 엉망진창이다. 데이터 형식은 제대로 지켜지지 않고 웹사이트는 자주 다운되며, 닫는 태그도 종종 빠져있다.
스크래퍼를 실행했는데, 예기치 못한 데이터 형식에 부딪혀 에러를 일으켜 멈춰있는 경우도 종종 있다.
이를 위해 예외처리를 추가해보자.
