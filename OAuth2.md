# OAuth2 (=OAuth 2.0)
OAuth2는 OAuth1의 유저의 인증플로우, 전반적인 목적만 공유하고 OAuth1.0을 새로 작성한것이다.  
OAuth2는 OAuth1의 앱 애플리케이션, 웹 애플리케이션, 데스크탑 애플리케이션등의 인증방식을 강화하고, Consumer에 개발 간소화를 중심으로 개발 되었다.  
<br>

### [OAuth1.0과 OAuth2.0의 차이점]
OAuth 1.0과 OAuth2.0 차이점은 일단 인증 절차 간소화 됨으로써 개발자들이 구현하기 더쉬워졌고,  
기존에 사용하던 용어도 바뀌면서 Authorizaiton server와 Resource서버의 분리가 명시적으로 되었다.  
또한 다양한 인증 방식을 지원하게 됐다. 
#### 인증 절차 
* 기능의 단순화, 기능과 규모의 확장성 등을 지원하기 위해 만들어 졌다.
* 기존의 OAuth1.0은 디지털 서명 기반이었지만 OAuth2.0의 암호화는 https에 맡김으로써 복잡한 디지털 서명에관한 로직을 요구하지 않기때문에 구현 자체가 개발자입장에서 쉬워짐.
#### 용어 변경
![flow](https://user-images.githubusercontent.com/38516906/55344443-49dee100-54e8-11e9-80d8-490259cb7586.png)

* Resource Owner : 사용자 (1.0 User해당)
* Resource Server : REST API 서버 (1.0 Protected Resource)
* Authorization Server : 인증서버 (API 서버와 같을 수도 있음)(1.0 Service Provider)
* Client : 써드파티 어플리케이션 (1.0 Service Provider 해당)
#### Resource Server와 Authorization Server서버의 분리
* 커다란 서비스는 인증 서버를 분리하거나 다중화 할 수 있어야 함.
* Authorization Server의 역할을 명확히 함.
#### 다양한 인증 방식(Grant_type)
* Authorization Code Grant
* Implicit Grant
* Resource Owner Password Credentials Grant
* Client Credentials Grant
* Device Code Grant
* Refresh Token Grant

<br>

### [인증 종류]
총 6가지로 구성되어 있다.
#### [1] Authorization Code Grant
일반적인 웹사이트에서 소셜로그인과 같은 인증을 받을 때 가장 많이 쓰는 방식으로 기본적으로 지원하고 있는 방식이다. 
![ACG](https://user-images.githubusercontent.com/38516906/55340280-51e65300-54df-11e9-9b1f-a4672b117fd0.png)

1. 먼저 클라이언트가 Redirect URL을 포함하여 Authorization server 인증 요청을 한다.
2. AuthorizationServer는 유저에게 로그인창을 제공하여 유저를 인증하게 된다.
3. AuthorizationServer는 Authorization code를 클라이언트에게 제공해준다.
4. Client는 코드를 Authorization server에 Access Token을 요청한다.
5. Authorization 서버는 클라이언트에게 Access token을 발급해준다.
6. 그 Access token을 이용하여 Resource server에 자원을 접근할 수 있게 된다.
7. 그이후에 토큰이 만료된다면 refresh token을 이용하여 토큰을 재발급 받을 수 있다.

#### [2] Implicit Grant
Public Client인 브라우저 기반의 애플리케이션(Javascript application)이나 모바일 애플리케이션에서 바로 Resource Server에 접근하여 사용할 수 있는 방식이다.
![IG](https://user-images.githubusercontent.com/38516906/55340422-9eca2980-54df-11e9-8ebf-35a37cfb4ed6.png)

1. 클라이언트는 Authorization server에 인증을 요청한다.
2. 유저는 Authorization server를 통해 인증한다.
3. Authorization server는 Access token을 포함하여 클라이언트의 Redirect url을 호출한다.
4. 클라이언트는 해당 Access token이 유효한지 Authorization server에 인증요청한다.
5. 인증서버는 그 토큰이 유효하다면 토큰의 만기시간과함께 리턴해준다.
6. 클라이언트는 Resource server에 접근할 수 있게된다.

#### [3] Resource Owner Password Credentials Grant
Client에 아이디/패스워드를 받아 아이디/패스워드로 직접 access token을 받아오는 방식이다.  
Client가 신용이 없을 때에는 사용하기에 위험하다는 단점이 있다. 클라이언트가 확실한 신용이 보장될 때 사용할 수 있는 방식이다.
![ROPCG](https://user-images.githubusercontent.com/38516906/55340772-4f382d80-54e0-11e9-9f01-e7845d84e856.png)

1. User가 Id와 Password를 입력한다
2. 클라이언트는 유저의 id와 password와 클라이언트 정보를 넘긴다.
3. Authorization sever는 Access token을 넘긴다.

#### [4] Client Credentials Grant
애플리케이션이 Confidential Client일 때 id와 secret을 가지고 인증하는 방식이다.
![CCG](https://user-images.githubusercontent.com/38516906/55340585-f7012b80-54df-11e9-9c8f-68a0a1a1251f.png)

1. 클라이언트 정보를 Authorization server에 넘긴다.
2. Access Token을 Client에 전달한다.

#### [5] Device Code Grant
장치 코드 부여 유형의 브라우저가 없거나 입력이 제한된 장치에서 사용되는 방식이다.

#### [6] Refresh Token Grant
기존에 저장해둔 리프러시 토큰이 존재할 때 엑세스토큰 재발급 받을 필요가 있을 때 사용한다. 그러면 기존 액세스는 토큰이 만료된다.
