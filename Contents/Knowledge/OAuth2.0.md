## **1.  OAuth2.0**

### **1) 등장배경**

![image](https://github.com/24tngus/CS_STUDY/assets/122462263/600d0eb8-3168-4ee9-b44a-a435b21d317b)

-   사용자가 B사이트의 계정을 이용해 A사이트를 사용하고자 할 때 A사이트는 B사이트에 유저의 정보를 요청하고 받아온 데이터를 A사이트에서 직접 저장해 관리하는 방식을 사용해야 했다.
-   이런 방식은 문제가 있는데 해당 문제를 해결하기 위해 OAuth프로토콜이 만들어졌다.
-   **문제점**  
    ● 사용자 : A 사이트에 B사이트의 ID와 Password를 넘겨주는 것에 대해 신뢰할 수 없다.  
    ● A 사이트 : ID와 Password를 받았기 때문에 보안 문제가 생기는 경우 모든 책임을 져야한다.  
    ● B 사이트 : A 사이트를 신뢰할 수 없다. 

### **2) OAuth2.0란?**

> **인증을 위한 표준 프로토콜의 한 종류**

-   소유한 리소스에 소프트웨어 애플리케이션이 접근할 수 있게 **접근 권한을 위임해주는 개방형 표준 프로토콜**
-   사용자 정보를 가지고 있는 웹 서비스에서 **사용자의 인증을 대신**해 주고, **접근 권한에 대한 토큰을 발급**하고 **토큰을 사용해 원하는 리소스의 접근** 할 수 있다.

      ex) client가 유저의 정보를 Resource Server에 요청해 대신 로그인한다.

### **3) OAuth2.0 참여자**

![image](https://github.com/24tngus/CS_STUDY/assets/122462263/022e6f36-8eaa-485b-b08b-ea3ad2965468)

### **4) OAuth2.0 기본 정보**

-   **RedirectURI**
    -   Authorization Server는 사용자가 정보제공 동의를 하게되면 사용자를 등록된 Redirect URI로 이동시킨다.
    -   client에 권한을 부여하는 과정에서 나중에 Authorized code를 전달하는 통로
    -   등록되지 않은 Redirect URI를 담은 요청이 들어오면 무효 처리한다.
-   **ClientID** 
    -   어플리케이션을 식별하는 식별 ID이다.
    -   Resource Server 입장에서 어떤 서비스에게 제공할 것 인지 구분하는 ID를 의미한다.
-     **ClientSecret**
    -   clientID의 비밀번호라고 생각하면 된다.
    -   절대 노출되면 안된다.
-   **Scope**
    -   Resource Server에서 사전에 사용 가능하도록 미리 정의한 기능
    -   해당 서비스에서 이용할 수 있는 기능을 말한다.
-   **Access Token**
    -   자원 서버에 자원을 요청할 수 있는 토큰
    -   사용자가 로그인에 성공하면, 서버에서 access토큰을 발급해준다.
    -   짧은 유효기간.
-   ****Refresh Token****
    -   권한 서버에 접근 토큰을 요청할 수 있는 토큰
    -   access 토큰의 갱신과 세션관리에 사용된다.
    -   긴 유효시간을 갖고있다.

### **5) OAuth2.0 동작흐름**

![image](https://github.com/24tngus/CS_STUDY/assets/122462263/1bee51ee-4a57-4d5a-8136-b24e552bf84a)

**1\. 유저가 로그인 하기 서비스 페이지로 접근해서 로그인******버튼을 클릭한다.****

![image](https://github.com/24tngus/CS_STUDY/assets/122462263/a3f51124-cce7-42cd-a70b-722691bf50ac)

**2\. clilent는 Authorization server에게 로그인요청한다.**

-   response\_type , client\_id , redirect\_uri , scope등의 매개변수를 쿼리스트링에 담아서 Authorization URL에 보낸다.
-   client가 보낸 Authorization URL를 Resource Server에있는 정보와 비교한다.
-   확인 후, 사용자에게 로그인 페이지를 리다이렉트 해준다.

```
https://resource.server/?   //리소스 서버(네이버, 카카오 사이트 url)
client_id=1    //어떤 client인지를 id를 통해 Resouce Owner에게 알려주는 부분
&scope=B,C   //Resource Owner가 사용하려는 기능, 달리 말해 client가 자신 서비스에서 사용하려는 Resource Server 기능을 표현한 부분
&redirect_uri=https://client/callback   //개발자 홈페이지에 서비스 개발자가 입력한 응답 콜백.
```

****3~4.**** ****Authorization **Server가 전용 로그인 페이지를 제공해주면 사용자는 정보를 입력한다.******

**5~6.  **Authorization Code를 발급해주고 제공된 Redirect URI로 사용자를 리디렉션시킨다.****

-   Redirect URI에 Authorization Code를 포함하여 사용자를 리디렉션시킨다.
-   Authorization Code: Client가 Access Token을 획득하기 위해 사용하는 임시 코드이다.
-   이 코드는 수명이 매우 짧다. (일반적으로 1~10분)

**7. Client는 Authorization Server에 Authorization Code를 전달하고, Access Token을 응답받는다.**

**8\. Client는 발급받은 Access Token을 저장하고, 이후 Resource Server에서 사용자의 리소스에 접근하기 위해 Access Token을 사용한다.** 

-   Access Token은 유출되어서는 안된다.
-   제 3자가 가로채지 못하도록 HTTPS 연결을 통해서만 사용될 수 있다.
-   Authorization Code와 Access Token 교환은 token 엔드포인트에서 이루어진다.

**9\.** **Client는 Resource Owner에게 로그인이 성공하였음을 알린다.**

******10~13. Resource Owner가 Resource Server의 리소스가 필요한 기능을 Client에 요청한다.******

-   Client는 발급받고 저장해둔 Resource Owner의 Access Token을 사용하여 제한된 리소스에 접근하고, Resource Owner에게 자사의 서비스를 제공한다.

### **6) Authorization Code가 필요한 이유**

-   Redirect URL의 데이터 전달방식은 URL에 데이터를 직접 실어서 전달하는 방법
-   브라우저에 바로 노출되는 문제가 있다 -> 보안 떨어진다
-   Redirect URL을 프론트엔드 주소로 설정한 후에 Authorization Code를 프론트엔드에서 백엔드로 전달하면 백엔드는 Authorization Server의 endpoint로 요청후 access token을 발급하기 때문에 Access Token이 항상 우리의 어플리케이션과 OAuth 서비스의 백채널을 통해서만 전송된다.
-   \-> 공격자가 Access Token을 가로챌 수 없게된다.

### **7) JWT와 OAuth의 관계** 

**OAuth는 하나의 framework이고 jwt는 토큰의 한 종류이다.**

#### **● OAuth가 Framework인 이유**

1\. 토큰을 요청할 때 사용할 수 있어야하는 요청 및 응답의 순서와 형식만 있다.

2\. 각기 다른 시나리오에서 어떤 방식으로 권한 부여 유형을 사용할지 정한다.

\-> **JWT는 이러한 Framework에서 발생하는 산출물**로 볼 수 있다.

#### **● OAuth토큰 & JWT토큰**

#### **① OAuth 토큰**

```
{
	"token_type":"bearer",
	"access_token":"eyJ0eXAiOiJKV1QiLCJh",
	"expires_in":20,
	"refresh_token":"fdb8fdbecf1d03ce5e6125c067733c0d51de209c"
}
```

-   oauth에서 사용하는 bearer라는 토큰의 일종
-   중요한 정보가 들어있는 토큰은 아니다. -> 랜덤한 문자열
-   oauth token을 사용하는 사용자는 토큰에 들어있는 정보를 알 수 없음
-   가지고 있는 토큰은 일종의 포인터 역할

#### [**② JWT토큰**](https://here-is-my-studyroom.tistory.com/75)

-   명확한 정보를 가지고 있는 토큰임
-    HTTP 헤더에 실어 전달함으로써 유저 세션을 유지할 필요가 없고 가볍게 데이터를 주고받을 수 있음