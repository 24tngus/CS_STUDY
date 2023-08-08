# Web Server vs Web Applicatin Server

## 1. 개념

Web = World Wide Web 의 약자로서, 인터넷을 기반으로 사용되는 서비스이다. WWW or W3 라고 많이 불려진다.

기본적으로 웹의 3대 요소에는 `URI`(주소), `HTML`(내용), `HTTP`(통신 규약)등 3개의 구성요소를 가지고 있다. 

URI : Uniform Resource Identifier 로서 인터넷에서 웹 페이지, 이미지, 비디오 등 리소스의 위치를 가리키는 문자열로서, 주소를 나타내고 있다.

HTML : Hypertext Markup Language 로서 웹 문서의 구조를 만들기 위한 언어로서, 정적인 컨텐츠를 담당하고 해당페이지의 내용을 담고 있다. **HTML 은 Markup Language 로서 Programing Language가 아니다.**

HTTP : Hyper text Transfer Protocol 로서 웹 상에서 정보를 주고 받을 수 있는 프로토콜로서 일종의 통신 규칙 이다. 

![Untitled](https://github.com/24tngus/CS_STUDY/assets/101094583/b0bf6d68-688c-4c4a-a222-bbfd37844f37)

server = 클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 시스템.

### `Web Server` = 클라이언트에게 인터넷을 통해 웹 서비스를 제공하는 컴퓨터로서 정적인 컨텐츠들만 사용자에게 제공할 수 있다.

### `Web Application Server` = Web Server + Web Application 이다. 
Web Application은 웹에서 실행되는 응용 프로그램으로서, 웹 애플리케이션이 서버환경을 만들어 동작하는 기능을 제공하는 소프트 웨어.
 필요한 동작을 수행하고 결과물을 웹 서버로 전달하는 역할을 한다. (간단히 프로그램 실행시, 데이터베이스에 접속하는 기능을 제공하고 비즈니스 로직 수행이 가능해 진다.)

`변화하는 정보를 사용자에게 제공한다. ⇒ WAS`

`정적인 이미지와 같은 데이터를 사용자에게 제공한다. ⇒ WS`

## 2. 정적 웹 페이지 (Static Web Page) vs 동적 웹 페이지(Dynamic Pages)

![Untitled1](https://github.com/24tngus/CS_STUDY/assets/101094583/38ece258-a779-4dde-bcc0-f3fd5a130c2c)

Web Server 에 미리 저장된 파일이 그대로 사용자에게 전달되는 웹 페이지 이다.

→ HTML, CSS, JS, IMG가 예시이다.

⇒ 클라이언트에 요청에 따라 저장된 파일을 응답하므로 빠르다는 `장점`이 있지만 `But` 저장된 데이터 이외의 데이터는 보여줄수 없어 서비스가 한정적 이라는 `단점`이 있다.

![Untitled2](https://github.com/24tngus/CS_STUDY/assets/101094583/b2059456-e780-4fa6-823e-e5c05e1d348c)


Web Server에 있는 데이터들을 스크립트에 의해 가공처리 한 뒤 생성되어 전달하는 웹 페이지 이다.

→ JSP, php, aps와 같은 페이지 생성 가능

⇒ 클라이언트의 요청에 따라 C, U, D가 가능하고, 동적으로 데이터를 처리하므로 여러 서비스를 제공할수 있다는 `장점` 이 있지만, `But` 처리하는 비즈니스 로직을 가지고 있어 이를 처리하는 Web Application Server가 필요하므로 비용적 `단점`이 있다.

# 3. Web Server 와 Web Application Server

![Untitled3](https://github.com/24tngus/CS_STUDY/assets/101094583/d73ee448-7172-4bab-b873-9b5b2a6af8d6)


## 3-1. Web Server

`하드웨어 적 의미의` WebServer : Web 서버가 설치되어 있는 컴퓨터.

`소프트웨어 적 의미의` WebServer : 웹 브라우저 클라이언트로부터 HTTP 요청을 받아 정적인 페이지를 제공하는 컴퓨터 프로그램

1. HTTP 프로토콜을 기반으로 클라이언트의 요청을 서비스하는 기능을 담당한다.
    1. 정적인 컨텐츠(이미지)의 요청이 들어온다면 WAS를 거치지 않고 바로 사용자에게 제공해준다.
    2. 동적인 컨텐츠의 요청이 들어 온다면, WAS에게 요청을 보내고 해당 응답을 받아 사용자에게 전달 해준다.
2. 예시
    1. Apache Server, Nginx, IIS등 이 있다.
    

## 3-2. Web Application Server

1. Web Server + Web Container 로서 DB조회나 다양한 비즈니스 로직을 처리하여 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server이다.
    
    자바 개발자들은 Web Application Container 라고도 불린다. ( 웹 애플리케이션이 배포되는 공간 이므로)
    

1. HTTP 를 통해 컴퓨터나 장치에 애플리케이션을 수행하는 `미들웨어` 이다.

1. WAS 는 `Web Container` or `Servlet Container` 라고도 부른다. 
    
    Container → JSP Servlet을 실행시킬 수 있는 소프트 웨어
    
2. WAS의 대표적인 기능으로는 DB 접속과 비즈니스 로직 수행 등이 있다.
3. 예시
    1. Tomcat, JBoss, Jeus, Web Sphere

## 3-3. 예시

- 사용자가 URI를 통해 서버에 이미지를 요청할때 웹 서버의 경우 어떠한 동작을 요구하는 것이 아닌 웹 서버에 저장되어 있는 이미지를 사용자에게 응답해준다면 끝이다. 이러한 과정에서는 WAS 까지 요구되지 않으므로 빠른 처리가 가능하다.
- 하지만 예를 들어 구구단을 사용자가 요청한다고 생각해보자, 정적인 페이지만 제공하는 Web Server의 경우 반복문을 돌릴수 없다. Why? 응답으로 보내는 HTML 의 경우 Programing language가 아니라, 구조를 잡는 Markup language 로서, 일반적인 For 문을 돌릴수없다. 만약 그렇게 되면 해당하는 응답에 대한 모든 페이지를 만들어놓고 처리를 해야하지만, 사실상 불가능 하다. 따라서 Web Application Server의 경우 Java와 같은 Programing language로 구성되어 있어, for 문을 돌려 나온 결과를 처리 할 수 있어 매우 효과적 이다.
- 따라서 정적인 페이지에 대한 요청이 들어올땐 Web Server 가 처리를, 동적인 요청이 들어 올땐 Web Application Server가 처리를 해야 한다.

# 4. Web Server Architecture

- Web Server 구조는 여러가지 구조를 가질수 있지만, 일반적인 구조의 경우
    - `Cient → Web Server → Web Application Server → DB` 구조를 가지고 있다.

![Untitled4](https://github.com/24tngus/CS_STUDY/assets/101094583/35e7a207-c66d-4609-ba4b-d00890e481f5)


Web Application Server 와 Web Server를 공부하면서 가장 궁금했던 부분이, 그렇다면 Web Application Server는 Web Server를 포함하고 있는데 그냥 WAS 만 사용하면 안되나? 단지, 속도 차이 때문에 그런것인가? 에대한 의문이였는데. 정확한 해답을 이해할수 있었다.

보통 일반적인 웹 서버의 구조의 경우 WAS 앞에 웹서버를 두어 분산 처리를 한다.

WAS 의 경우 DB 조회나, 다양한 비즈니스 로직을 처리하는 대에 집중을 해야하고, 웹 서버의 경우 정적인 컨텐츠에 대한 요청을 맡기며 서로 기능을 분리 해주어야 서버의 부하를 방지하여 효율적인 서버 운영이 가능해 진다. 만약 모든 요청을 WAS 가 관리하게 된다면, 일반적인 이미지 요청에도 데이터 처리가 지연되고 속도가 느려지며 여러 문제가 발생하게 된다.  

⇒ 따라서 `Cient → Web Server → Web Application Server → DB`  구조를 통해, 사용자의 요청이 정적인 컨텐츠라면 Web Server에서 바로 응답 해주며 1차적으로 걸러주고, 동적인 컨텐츠 라면 WAS에게 요청을 보내어 응답을 받는 구조를 가져 효율적인 분산 처리를 진행해 준다.

### *번외*

Apache  와 Apache Tomcat

Apache ⇒ Web Server 

Apache Tomcat ⇒ Web Application Server

옛날에는 웹 서버는 Apache, WAS 는 Tomcat 이었으나 Tomcat 자체가 업데이트 되어오며, 정적인 컨텐츠를 처리하는 기능이 추가되었고, 순수 Apache만을 사용하는 것과 성능 차이가 없어 Apache Tomcat으로 불리게 되었다. 

관련 공부
리버스 프록시
Nginx vs Apache (웹 서버의 동작방식)

### 참고

https://velog.io/@kimxdaxsom/%EC%9B%B9-3%EB%8C%80-%EC%9A%94%EC%86%8C

https://velog.io/@gillog/Web-Server%EC%99%80-Web-Application-Server%EC%9D%98-%EC%B0%A8%EC%9D%B4

https://titus94.tistory.com/4

https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html

https://codechasseur.tistory.com/25

https://www.youtube.com/watch?v=mcnJcjbfjrs

https://www.youtube.com/watch?v=NyhbNtOq0Bc

[원본 글](https://github.com/24tngus/CS_STUDY/discussions/17#discussion-5456888)