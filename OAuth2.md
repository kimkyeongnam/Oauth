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

### 인증 종류
총 6가지로 구성되어 있다.
#### Authorization Code Grant
일반적인 웹사이트에서 소셜로그인과 같은 인증을 받을 때 가장 많이 쓰는 방식으로 기본적으로 지원하고 있는 방식이다. 
![ACG](https://user-images.githubusercontent.com/38516906/55340280-51e65300-54df-11e9-9b1f-a4672b117fd0.png)

1. 먼저 클라이언트가 Redirect URL을 포함하여 Authorization server 인증 요청을 한다.
2. AuthorizationServer는 유저에게 로그인창을 제공하여 유저를 인증하게 된다.
3. AuthorizationServer는 Authorization code를 클라이언트에게 제공해준다.
4. Client는 코드를 Authorization server에 Access Token을 요청한다.
5. Authorization 서버는 클라이언트에게 Access token을 발급해준다.
6. 그 Access token을 이용하여 Resource server에 자원을 접근할 수 있게 된다.
7. 그이후에 토큰이 만료된다면 refresh token을 이용하여 토큰을 재발급 받을 수 있다.

### [OAuth1의 개념]

|**용어**|**설명**|
|:--:|:--:|
|User|Service Provider에 계정을 가지고 있으면서, Consumer앱을 이용하려는 사용자|
|Service<br>Provider|OAuth를 사용하는 Open API를 제공하는 서비스 (facebook,google등)|
|Protected<br>Resource|Service Provider로부터 제공되어지는 API 자원들|
|Consumer|OAuth 인증을 사용해 Service Provider의 기능을 사용하려는 애플리케이션이나 웹 서비스|
|Consumer<br>Key|Consumer가 Service Provider에게 자신을 식별하는 데 사용하는 키|
|Consumer<br>Secret|Consumer Key의 소유권을 확립하기 위해 Consumer가 사용하는 Secret|
|Request<br>Token|Consumer가 Service Provider에게 접근 권한을 인증받기 위해 사용하는 값<br>인증이 완료된 후에는 Access Token으로 교환한다.|
|Access<br>Token|인증 후 Consumer가 Service Provider의 자원에 접근하기 위한 키를 포함한 값|
|Token<br>Secret|주어진 토큰의 소유권을 인증하기 위해 소비자가 사용하는 Secret|

<br>

### [OAuth 1.0의 WorkFlow]
![OAuth 1.0](https://user-images.githubusercontent.com/38516906/55337252-4132de80-54d9-11e9-9f9c-1602eac8bb04.png)

1. (그림에는 없지만) 가장 먼저 Consumer는 Service Provider로부터 Client key와 Secret을 발급 받아야한다. 이것은 Service Provider에 API를 사용할것을 등록하는것과 동시에 Service Provider가 Consmer를 식별할 수 있게 해준다.
2. 그림에 A 처럼 Request Token을 요청할 때 Consumer 정보, Signature 정보를 포함하여 Request token을 요청을하고 B의 흐름처럼 Request token을 발급받는다.
3. Request Token값을 받은후 Consumer는 C처럼 User를 Service Provider에 인증 사이트로 다이렉트시키고, 유저는 그곳에서 Service Provider에 유저임을 인증하게 된다.
4. 그러면 Consumer는 D의 정보처럼 해당 유저가 인증이되면 OAuth_token와 OAuth_verifier를 넘겨준다.
5. 그 이후에 Consumer는 OAuth_token와 OAuth_verifier받았다면 E의 흐름처럼 다시 서명을 만들어 Access Token을 요청하게 된다.
6. 그리고 Service Provider는 받은 토큰과 서명들이 인증이 되었으면 Access Token을 F의 정보 처럼 넘기게된다.
7. 그리고 그 Access Token 및 서명정보를 통해 Service Provider에 Protected Resource에 접근할 수 있게 된다.

<br>

### [Request Token 발급 매개변수]
Request Token 발급 요청 시 사용하는 매개변수

|**매개변수**|**설명**|
|:--:|:--:|
|OAuth_callback|Service Provider가 인증을 완료한 후 리다이렉트할 Consumer의 웹 주소<br>만약 Consumer가 웹 애플리케이션이 아니라 리다이렉트할 주소가 없다면 소문자로 ‘oob’(Out Of Band라는 뜻)를 값으로 사용한다|
|OAuth_consumer_key|Consumer를 구별하는 키 값<br>Service Provider는 이 키 값으로 Consumer를 구분한다|
|OAuth_nonce|Consumer에서 임시로 생성한 임의의 문자열<br>OAuth_timestamp의 값이 같은 요청에서는 유일한 값이어야 한다<br>(악의적인 목적으로 계속 요청을 보내는 것을 막기 위해서)|
|OAuth_signature|OAuth 인증 정보를 암호화하고 인코딩하여 서명 값<br>OAuth 인증 정보는 매개변수 중에서 OAuth_signature를 제외한 나머지 매개변수와 HTTP 요청 방식을 문자열로 조합한 값이다<br>암화 방식은 OAuth_signature_method에 정의된다<br>|
|OAuth_signature_method|OAuth_signature를 암호화하는 방법<br>HMAC-SHA1, HMAC-MD5 등을 사용할 수 있다|
|OAuth_timestamp|요청을 생성한 시점의 타임스탬프<br>1970년1월 1일 00시 00분 00초 이후의 시간을 초로 환산한 초 단위의 누적 시간이다.|
|OAuth_version|OAuth 사용 버전<br>1.0a는 1.0이라고 명시하면 된다|

<br>

### [Request Token Signature 생성]
OAuth 1.0에서는 Service Provider에게 요청을 할려면 매번 전자 서명을 만들어서 보내야한다.

1. 요청 매개변수를 모두 모은다.  
OAuth_signature를 제외하고 'OAuth_'로 시작하는 OAuth 관련 매개변수를 모은다. 모든 매개변수를 사전순으로 정렬하고 각각의 키(key)와 값(value)에 URL 인코딩(rfc3986)을 적용한다. URL 인코딩을 실시한 결과를 = 형태로 나열하고 각 쌍 사이에는 &을 넣는다. 이렇게 나온 결과 전체에 또 URL 인코딩을 적용한다.

2. 매개변수를 정규화(Normalize)한다.  
모든 매개변수를 사전순으로 정렬하고 각각의 키(key)와 값(value)에 URL 인코딩(rfc3986)을 적용한다. URL 인코딩을 실시한 결과를 = 형태로 나열하고 각 쌍 사이에는 &을 넣는다. 이렇게 나온 결과 전체에 또 URL 인코딩을 적용한다.

3. Signature Base String을 만든다.  
HTTP method 명(GET 또는 POST), Consumer가 호출한 HTTP URL 주소(매개변수 제외), 정규화한 매개변수를 '&'를 사용해 결합한다. 즉 ‘[GET|POST] + & + [URL 문자열로 매개변수는 제외] + & + [정규화한 매개변수]’ 형태가 된다.

4. 키 생성  
3번 과정까지 거쳐 생성한 문자열을 암호화한다. 암호화할 때 Consumer Secret Key를 사용한다. Consumer Secret Key는 Consumer가 Service Provider에 사용 등록을 할 때 발급받은 값이다. HMAC-SHA1 등의 암호화 방법을 이용하여 최종적인 OAuth_signature를 생성한다.

<br>

### [Access Token 매개변수]
|**매개변수**|**설명**|
|:--:|:--:|
|OAuth_consumer_key|Consumer를 구별하는 키 값<br>Service Provider는 이 키 값으로 Consumer를 구분한다|
|OAuth_nonce|Consumer에서 임시로 생성한 임의의 문자열<br>OAuth_timestamp의 값이 같은 요청에서는 유일한 값이어야 한다<br>(악의적인 목적으로 계속 요청을 보내는 것을 막기 위해서)|
|OAuth_signature|OAuth 인증 정보를 암호화하고 인코딩하여 서명 값<br>OAuth 인증 정보는 매개변수 중에서 OAuth_signature를 제외한 나머지 매개변수와 HTTP 요청 방식을 문자열로 조합한 값이다<br>암호화 방식은 OAuth_signature_method에 정의된다|
|OAuth_signature_method|OAuth_signature를 암호화하는 방법<br>HMAC-SHA1, HMAC-MD5 등을 사용할 수 있다|
|OAuth_timestamp|요청을 생성한 시점의 타임스탬프<br>1970년1월 1일 00시 00분 00초 이후의 시간을 초로 환산한 초 단위의 누적 시간이다|
|OAuth_version|OAuth 사용 버전|
|OAuth_verifier|Request Token 요청 시 OAuth_callback으로 전달받은 OAuth_verifier 값|
|OAuth_token|Request Token 요청 시 OAuth_callback으로 전달받은 OAuth_token 값|

<br>

### [API호출을 위한 매개변수]
|**매개변수**|**설명**|
|:--:|:--:|
|OAuth_consumer_key|Consumer를 구별하는 키 값<br>Service Provider는 이 키 값으로 Consumer를 구분한다|
|OAuth_nonce|Consumer에서 임시로 생성한 임의의 문자열<br>OAuth_timestamp의 값이 같은 요청에서는 유일한 값이어야 한다<br>(악의적인 목적으로 계속 요청을 보내는 것을 막기 위해서)|
|OAuth_signature|OAuth 인증 정보를 암호화하고 인코딩하여 서명 값<br>OAuth 인증 정보는 매개변수 중에서 OAuth_signature를 제외한 나머지 매개변수와 HTTP 요청 방식을 문자열로 조합한 값이다<br>암호화 방식은 OAuth_signature_method에 정의된다|
|OAuth_signature_method|OAuth_signature를 암호화하는 방법<br>HMAC-SHA1, HMAC-MD5 등을 사용할 수 있다|
|OAuth_timestamp|요청을 생성한 시점의 타임스탬프<br>1970년1월 1일 00시 00분 00초 이후의 시간을 초로 환산한 초 단위의 누적 시간이다|
|OAuth_version|OAuth 사용 버전|
|OAuth_token|Request Token 요청 시 OAuth_callback으로 전달받은 OAuth_token 값|
