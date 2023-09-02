## 📒 Servlet & Spring MVC

- 발전 순서 : 순수 JAVA 웹 처리 구현 ➡️ Servlet ➡️ JSP ➡️ MVC1 ➡️ MVC2

### 📕 Web Server & WAS

- Web Server : 정적 페이지 처리하는 서버  
- WAS : 동적 컨텐츠 생성해주는 서버 

### 📕 Servlet

#### 1) Servlet 개념

- Java 기반의 웹 프로그래밍 기술 (.java 클래스로 자바의 모든 기능 가능)
- Java 코드 안에 HTML 코드 넣음
- 특징
  - 웹 서버 응답/요청을 직접 처리의 불편함 해소 ➡️ Data processing(`Controller`)에 좋음
  - 동적 HTML 생성의 불편함 해소
- 장점 
  - 스레드 기반의 빠른 처리 속도
- 단점
  - Java코드를 컴파일한 후 동적 페이지 처리 ➡️ Servlet이 수정된 경우, 재컴파일 및 재배포 필요
  - 프로그램 내에서 HTML 작성하면, 화면 인터페이스 구현에 너무 많은 코드 필요함 ➡️ HTML 가독성 나쁨 (유지보수 문제)

#### 2) [Servlet 구현](https://hyunki99.tistory.com/47)

![](https://velog.velcdn.com/images/24tngus/post/809d6593-7640-4e5d-a04f-a470261e08be/image.png)
  - `Servlet` 인터페이스 안에 `service` 메소드 존재
  - `ServletRequest`를 비즈니스 로직으로 처리해서 `ServletResponse`로 응답
![](https://velog.velcdn.com/images/24tngus/post/9ed93a43-f2d9-49b4-9506-992ae2c6b18e/image.png)
  - Java 내장 라이브러리에 `HTTPServlet`이라는 추상 클래스 구현
  - `HTTPServlet`은 `GenericServlet(서블릿 인터페이스)`를 상속받음
  - 웹에서 오는 요청과 응답을 다루는 표준 프로토콜 
  - doGet(), doPost() 등 메서드를 오버라이딩해서 원하는 로직 구현 가능 (오버라이딩)

- 🗒️ 예시
```java
package hello.servlet.web;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "memberSaveServlet", urlPatterns = "/servlet/members/save")
public class MemberSaveServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberSaveServlet.service");

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");

        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        PrintWriter w = response.getWriter();
        w.write("<html>\n" +
                "<head>\n" +
                " <meta charset=\"UTF-8\">\n" + "</head>\n" +
                "<body>\n" +
                "성공\n" +
                "<ul>\n" +
                "    <li>id="+member.getId()+"</li>\n" +
                "    <li>username="+member.getUsername()+"</li>\n" +
                " <li>age="+member.getAge()+"</li>\n" + "</ul>\n" +
                "<a href=\"/index.html\">메인</a>\n" + "</body>\n" +
                "</html>");
    }
}
```
 
#### 3) Servlet 동작 과정

![](https://velog.velcdn.com/images/24tngus/post/498b8dee-dd8b-4ee4-85a5-bac6911feac8/image.png)   
- 사용자 요청이 들어옴 ➡ 웹 서버가 WAS에게 요청 위임 ➡ Servlet Container가 요청을 해결할 수 있는 Servlet을 찾아 응답 

### 📕 JSP (Java Server Pages)

- 서블릿 기반의 서버 스크립트 언어 (동적 웹페이지 생성)
- HTML 코드 안에 Java 코드 넣음
- 장점 
  - HTML 작성이 편리하여 View 구현하기 좋음
  - JSP가 수정된 경우 ➡️ 재배포 필요 없이 WAS가 알아서 처리 
  - 요청이 메모리에서 치리되어 빠른 처리속도

- 🗒️ 예시
```java
<%@ page import="hello.servlet.domain.member.Member" %>
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    MemberRepository memberRepository = MemberRepository.getInstance();
    System.out.println("MemberSaveServlet.service");

    String username = request.getParameter("username");
    int age = Integer.parseInt(request.getParameter("age"));

    Member member = new Member(username, age);
    memberRepository.save(member);
%>
<html>
<head>
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id=<%=member.getId()%></li>
    <li>username=<%=member.getUsername()%></li>
    <li>age=<%=member.getAge()%></li>
</ul>
<a href="/index.html">메인</a>
</body>
</html>
```
  
### 📕 MVC (Model-View-Container) 패턴

#### 1) MVC 개념

- `Servlet` + `JSP` 형태
- **M**odel
  - View에 출력할 데이터 담음
  - 뷰에서 모델만 보고 꺼내서 써서, 렌더링에 집중함
- **V**iew : 사용자의 요청을 화면으로 출력 (`JSP`)
- **C**ontroller : 사용자 요청을 처리, 요청에 따른 전체적인 흐름 제어 (`Servlet`)
![](https://velog.velcdn.com/images/24tngus/post/f007a648-1b0c-44c5-9007-6b7da8314006/image.png)

- FrontController 패턴을 접목한 웹 어플리케이션 프레임워크 
- 서블릿이 호출되면 `HttpServlet`이 제공하는 `service()` 호출 
➡️ 스프링 MVC `DispatcherServlet`의 부모인 `FrameworkServlet`에서 `service()` 오버라이드 
➡️ `FrameworkServlet.service()`를 시작하고, 여러 메서드가 호출되면서 `DispatcherServlet.doDispatcher()` 호출됨

사용자가 정보를 `Controller`에게 요청 ➡️ 비즈니스 로직 수행 중 필요시 `Model`호출하여 데이터 요청 ➡️ 완료시 `View`를 통해 화면에 출력

#### 2) MVC 요소

![](https://velog.velcdn.com/images/24tngus/post/ca543918-b823-41e2-804f-412f4d0dc47f/image.png)
- Handler Mapping
  - 요청을 처리할 Handler 찾는 역할 (Controller)
  - 요청된 URL에 매핑되는 핸들러(컨트롤러)를 찾아서 반환하는 메서드
![](https://velog.velcdn.com/images/24tngus/post/94d515e1-96c0-4c3e-b056-d8946d7ce06d/image.png)

- Handler Adaptor
  - Handler의 실제 로직을 수행하는 역할
  - 핸들러의 종류에 관계없이 해당 핸들러 실행할 수 있도록 함
  - support ➡️ true이면, handle 메서드로 핸들러 로직 수행
  ![](https://velog.velcdn.com/images/24tngus/post/7182ac09-499c-4d76-9299-b03baeae68f3/image.png)![](https://velog.velcdn.com/images/24tngus/post/16349a09-cf58-47b4-a516-8b2ba9868091/image.png)

- View Resolver
  - 반환된 View 이름을 실제 View 객체로 변환하는 역할
  - `ModelAndView` : 컨트롤러에서 처리한 데이터(모델)과 해당 데이터를 표시할 View의 정보를 가진 객체 
  ![](https://velog.velcdn.com/images/24tngus/post/bcb172b3-0e82-433f-b483-a91234c4257e/image.png)
![](https://velog.velcdn.com/images/24tngus/post/40798beb-ca6e-4e58-8646-33b4c63938eb/image.png)

- Dispatcher Servlet 
  - Handler 매핑
  - 인터페이스로 구현된 `HandlerMapping`, `HandlerAdapter`, `ViewResolver`를 리스트로 가짐
  ![](https://velog.velcdn.com/images/24tngus/post/f053ed5e-fd62-4cec-a6c6-b9b5955048c8/image.png)

> 📖 참고 📖  [doDispatch 동작 순서](https://devpoong.tistory.com/55)
- 처음 FrontController 역할
```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
        HttpServletRequest processedRequest = request;
        HandlerExecutionChain mappedHandler = null;
        ModelAndView mv = null;
        // 1. 핸들러 조회
        mappedHandler = getHandler(processedRequest);
        if (mappedHandler == null) {
            noHandleFound(processedRequest, response);
            return;
        }
        // 2.핸들러 어댑터 조회-핸들러를 처리할 수 있는 어댑터
        HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
        // 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
                mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }
    private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, 
                                       HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {
        // 뷰 렌더링 호출
        render(mv, request, response);
    }
    protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
        View view;
        String viewName = mv.getViewName();
        // 6. 뷰 리졸버를 통해서 뷰 찾기, 7.View 반환
        view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
        // 8. 뷰 렌더링
        view.render(mv.getModelInternal(), request, response);
    }
```
#### 3) MVC 종류

- MVC1 
  - JSP가 뷰 + 컨트롤러 역할 
  - JSP에 Java코드와 Html, css 코드 섞여 있어 소스 복잡
  - 장점 : 설계 간단, 개발 속도 빠름 (작은 프로젝트에 적합)
  - 단점 : 읽기가 어렵고, 유지보수 힘듦
  ![](https://velog.velcdn.com/images/24tngus/post/9c2d3419-594e-467a-9f19-0f492d9d7ffb/image.png)

- MVC2
  - JSP가 뷰, Servlet이 컨트롤러 역할
  - Servlet이 요청 받고, 비즈니스 로직 수행하며 모델 호출 ➡️ 뷰 역할인 JSP 제어하여 화면 출력
  - JSP는 Java코드 안쓰고, JSTL 사용하여 결과를 화면에 출력
  - 장점 : 확장에 용이, 유지보수 수월함
  - 단점 : 초기 설계단계에 비용 많이 듦
  ![](https://velog.velcdn.com/images/24tngus/post/d2fcaef7-993a-4ad8-828f-164c038cd8db/image.png)
  
> 📖 참고 📖 [JSTL](https://daesuni.github.io/jstl/)
- 자바 서버 페이지 표준 태그 라이브러리 (Javaserver pages Standard Tag Library)
- Java EE 기반의 웹 애플리케이션 개발 플랫폼을 위한 컴포넌트 모음
- XML 데이터 처리, JSP 태그 라이브러리 추가하여 JSP 사양 확장

- 🗒️ 예시 (Controller)
```java
package hello.servlet.web.servletmvc;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

@WebServlet(name = "mvcMemberListServlet", urlPatterns = "/servlet-mvc/members")
public class MvcMemberListServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        List<Member> members = memberRepository.findAll();
        request.setAttribute("members", members);

        String viewPaht = "/WEB-INF/views/members.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPaht);
        dispatcher.forward(request, response);
    }
}
```
- 🗒️ 예시 (View)
```java
// /WEB-INF/views/members.jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
<head>
    <title>Title</title>
</head>
<body>
<a href="/index.html">메인</a>
<table>
  <thead>
  <th>id</th>
  <th>username</th>
  <th>age</th>
  </thead>
  <tbody>
  <c:forEach var="item" items="${members}">
    <tr>
      <td>${item.id}</td>
      <td>${item.username}</td>
      <td>${item.age}</td>
    </tr>
  </c:forEach>
  </tbody>
</table>
</body>
</html>
```