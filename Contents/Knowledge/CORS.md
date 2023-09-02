### 목차
> 1. 서론
> 1.1 SOP
> 2. CORS
> 2.1 CORS 동작방법
> 2.2 CORS 접근 제어 시나리오
> 3. CORS 해결방법

# CORS

## 1.  서론
![Untitled (5)](https://github.com/24tngus/CS_STUDY/assets/75667075/2dd6fed0-c899-48c0-b572-9b6be6e52531)




- Rest API를 만들고 프론트개발자와 작업하다보면 꼭 발생하는 에러!

### 1.1 SOP(Same Origin Policy)

다른 출처의 리소스를 사용하는 것에 제한하는 보안 방식

즉, **동일 출처 정책은 웹 브라우저 보안을 위해 프로토콜, 호스트, 포트가 동일한 서버로만 요청을 주고 받을 수 있도록 한 정책**

**출처(origin)란?**

- 저희가 배웠던 origin
- `Protocol`, `Host`, `Port` 를 통해 같은 출처인지 다른 출처인지 판단할 수 있다.

![Untitled (6)](https://github.com/24tngus/CS_STUDY/assets/75667075/962c4042-9cbf-4e9c-b3bf-6fdf62cb839f)

<aside>
💡 **왜 필요한가?**

![Untitled (7)](https://github.com/24tngus/CS_STUDY/assets/75667075/004363b6-d1c3-4cea-8d72-89d3f84810b6)


1. 인스타그램 이용자가 로그인하여 사용
2. 해커가 이용자에게 DM으로 수상한 링크를 보냈는데 이용자가 클릭을 해버림.
3. 링크를 통해 이상한(❓) 사진을 피드에 박제하는 자동 기능이 실행됨

이를 방지하기위해 SOP가 중요한 역할을 수행한다.

인스타그램 입장에서는 **Origin 출처가 다르므로 Cross Origin이 발동**된 관계에 따라 SOP의 절대적 룰이 위반되어 해당 요청을 무시하게 된다.

</aside>

**다른 출처의 리소스가 필요하다면 어떻게 할까?**

## 2. CORS

**교차 출처 리소스 공유(Cross-Origin Resource Sharing, CORS)**는 추가 HTTP 헤더를 사용하여, **한 출처에서 실행중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여**하도록 브라우저에 알려주는 체제

즉, **현재 IP가 아닌 다른 IP로 리소스를 요청하는 구조이다.**

### 2.1 CORS 동작방법

1. HTTP 통신 헤더인 Origin 헤더에 요청을 보내는 곳의 정보를 담고 서버로 요청을 보낸다.
2. 이후 서버는 Access-Control-Allow-Origin헤더에 허용된 Origin(우리 리소스에 접근해도 되는 출처)이라는 정보를 담아 보낸다.
3. 클라이언트는 헤더의 값과 비교해 정상 응답임을 확인하고 지정된 요청을 보낸다.
4. 서버는 요청을 수행하고 200OK 코드를 응답한다.

기본 구조는 이렇지만 세 가지의 시나리오에 따라 변경된다.

### 2.2 CORS 접근 제어 시나리오

- 프리플라이트 요청(Preflight Request)
- 단순 요청(Simple Request)
- 인증정보 포함 요청(Credentialed Request)

**Preflight Request**

1. Options 메서드를 통해 다른 도메인의 리소스에 요청이 가능한 지 확인 작업
2. 요청이 가능하다면 실제 요청(Actual Request)을 보낸다

<aside>
💡 **Preflight request란?**
**실제 요청 전에 브라우저에서 보내는 작은 요청**입니다. 지금 요청을 보내는 프론트엔드가 백엔드 서버에서 허용한 Origin이 맞는지, 그리고 해당 엔드포인트에서 어떤 HTTP 메소드들을 허용하는지 등을 확인합니다. 만약 허용되는 Origin이고 요청하는 메소드도 허용되는 것이라면 실제 요청을 할 수 있게 해줍니다. 그렇지 않다면, 실제 요청을 보내기도 전에 보내지 못하게 막는 것이죠.

</aside>


![Untitled (8)](https://github.com/24tngus/CS_STUDY/assets/75667075/724aeffe-4f23-47fe-a01f-84b116a35647)

**왜 Preflight가 필요할까?**

→ CORS를 모르는 서버를 위해서

- CROS spec이 생기기 이전에 만들어진 서버들은 브라우저의 SOP Request만 가능하다는 가정하에 만들어졌다.
- 그런데, cross-site request가 CORS로 인해 가능해졌기 때문에 이러한 서버들은 cross-site request에 대한 보안 메커니즘(security mechanism)이 없다보니 보안적으로 문제가 생길 수 있었다.
- 이런 보안적인 문제들을 보호하기 위해 CORS spec에다가 preflight request를 포함하여 prefilght request를 통해 서버가 CORS를 인식하고 핸들링할 수 있는지 먼저 확인을 하게 한 뒤 , CORS를 인식하지 못하는 서버들을 보호하게 된다.
- 만약 preflight request가 서버로 전송을 하였을 때 CORS를 인식하지 못하는 서버라면 제대로 된 응답(response)를 보내지 못하게 되고 그렇게 되면 실제 client로 부터의 요청(request)도 전송되지 않게 되어 preflight 가 cross-origin 요청(request)에 대비 되지 않은 서버를 보호해주게 된다.

**Simiple Request**

- Preflight 요청 없이 바로 요청을 날린다
- 다음 조건을 모두 만족해야 한다.
    - GET, POST, HEAD 메서드
    - Content-Type
        - application/x-www-form-urlencoded
        - multipart/form-data
        - text
    - 헤더는 Accept, Accept-Language, Content-Language, Content-Type만 허용

즉, 까다로운 조건을 걸고 해당 조건에 부합하다면 안전한 요청이라 인식하고 데이터에 응답하는 형식이다.

![Untitled (9)](https://github.com/24tngus/CS_STUDY/assets/75667075/a71c4953-7d37-43c9-b638-b3582f3b698b)


**Credentialed Request**

인증 관련 헤더를 포함할 때 사용하는 요청

CORS기본적인 방식 보다는 다른 출처 간 통신에서 조금 더 보안을 강화하고자 할때 사용하는 방법이다. 

→ 예를들어 JWT Token을 보내고 싶을 때 jwt token을 담아서 보낸다.

**클라이언트측** credentials: include

**서버측** Access-Control-Allow-Credentials : true

다른 출처의 경우 로그인 유지를 위한 쿠키가 서로 전송되지 않는 문제가 있어서 해당 옵을 주는 것으로 사용하는 것 같다

## 3. CORS 해결방법

스프링 부트를 이용하기

해결방법

- `CorsFilter`로 직접 response에 header 넣어주기
- Controller에서 `@CrossOrigin` 애너테이션 추가하기
- `WebMvcConfigurer` 이용하기

```java
package com.newyearletter.newyearletter.configuration;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig  implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry){
        registry.addMapping("/**") //해당 서버의 모든 URL 요청 허용
                .allowedOrigins("https://www.newyearletter.com", "localhost:8484") //원하는 도메인만 허용
                .allowedMethods("GET", "POST", "PATCH", "PUT", "DELETE") //원하는 HTTP 메서드 허용
                .allowedHeaders("headers") // 헤더 네임
                .maxAge(3000); //preflight요청 캐싱
    }

}
```
필자는 WebMvcConfigurer를 이용했다. 
다른 방법을 추가로 확인하기 위해서는 아래의 링크를 참고하면 된다.
[Springboot CORS 허용](https://wonit.tistory.com/572)

## Error log
**error**
- Response to preflight request doesn’t pass access control check
![Untitled (10)](https://github.com/24tngus/CS_STUDY/assets/75667075/798ac7d7-73ea-4373-a086-741408c5e615)

**해결방법**
Spring Security가 Option Method를 막아서 일어난 문제.
Preflight는 Option Method로 날아오기 때문에 아래와 같이 Option메서드를 허용해주어야 한다.
![Untitled (11)](https://github.com/24tngus/CS_STUDY/assets/75667075/4dc7e530-69d4-4c5b-8d2c-b8e8d06284f0)
