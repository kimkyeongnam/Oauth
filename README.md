# OAuth
**(본 레포지토리는 OAuth 2.0에 대한 코드임을 미리 밝힙니다.)**

* **OAuth란?**  
**Open Authorization, Open Authentication** 라는 의미로 애플리케이션(페이스북,구글,트위터)(**Service Provider**)의 유저의 비밀번호를 Third party앱에 제공 없이 인증, 인가를 할 수 있는 오픈 스탠다드 프로토콜이다.<br>OAuth 인증을 통해 애플리케이션 API를 유저대신에 접근할 수 있는 권한을 얻을 수 있다.<br><br>OAuth가 사용되기 전에는 외부 사이트와 인증기반의 데이터를 연동할 때 인증방식의 표준이 없었기 때문에 기존의 기본인증인 아이디와 비밀번호를 사용하였는데, 유저의 비밀번호가 노출될 가망성이 크기 때문에 보안상 취약한 구조를 가지고 있었다.<br>그렇기에 이 문제를 보안하기 위해 OAuth의 인증은 API를 제공하는 서버에서 진행하고, 유저가 인증되었다는 Access Token을 발급하였다.<br>그 발급된 **Access token**으로 Third party(**Consumer**)애플리케이션에서는 Service Provider의 API를 안전하고 쉽게 사용할 수 있게 되었다.

<br><br>

## OAuth1 (=OAuth 1.0)

* **OAuth1의 개념**

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

* **OAuth 1.0의 WorkFlow**
![OAuth 1.0](https://user-images.githubusercontent.com/38516906/55337252-4132de80-54d9-11e9-9f9c-1602eac8bb04.png)

1. (그림에는 없지만) 가장 먼저 Consumer는 Service Provider로부터 Client key와 Secret을 발급 받아야한다. 이것은 Service Provider에 API를 사용할것을 등록하는것과 동시에 Service Provider가 Consmer를 식별할 수 있게 해준다.
2. 그림에 A 처럼 Request Token을 요청할 때 Consumer 정보, Signature 정보를 포함하여 Request token을 요청을하고 B의 흐름처럼 Request token을 발급받는다.
3. Request Token값을 받은후 Consumer는 C처럼 User를 Service Provider에 인증 사이트로 다이렉트시키고, 유저는 그곳에서 Service Provider에 유저임을 인증하게 된다.
4. 그러면 Consumer는 D의 정보처럼 해당 유저가 인증이되면 OAuth_token와 OAuth_verifier를 넘겨준다.
5. 그 이후에 Consumer는 OAuth_token와 OAuth_verifier받았다면 E의 흐름처럼 다시 서명을 만들어 Access Token을 요청하게 된다.
6. 그리고 Service Provider는 받은 토큰과 서명들이 인증이 되었으면 Access Token을 F의 정보 처럼 넘기게된다.
7. 그리고 그 Access Token 및 서명정보를 통해 Service Provider에 Protected Resource에 접근할 수 있게 된다.

<br>

* **Request Token 발급 매개변수**
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

* **Request Token Signature 생성**
OAuth 1.0에서는 Service Provider에게 요청을 할려면 매번 전자 서명을 만들어서 보내야한다.

1. ~요청 매개변수를 모두 모은다.~
OAuth_signature를 제외하고 'OAuth_'로 시작하는 OAuth 관련 매개변수를 모은다. 모든 매개변수를 사전순으로 정렬하고 각각의 키(key)와 값(value)에 URL 인코딩(rfc3986)을 적용한다. URL 인코딩을 실시한 결과를 = 형태로 나열하고 각 쌍 사이에는 &을 넣는다. 이렇게 나온 결과 전체에 또 URL 인코딩을 적용한다.

2. 매개변수를 정규화(Normalize)한다.
모든 매개변수를 사전순으로 정렬하고 각각의 키(key)와 값(value)에 URL 인코딩(rfc3986)을 적용한다. URL 인코딩을 실시한 결과를 = 형태로 나열하고 각 쌍 사이에는 &을 넣는다. 이렇게 나온 결과 전체에 또 URL 인코딩을 적용한다.

3. Signature Base String을 만든다.
HTTP method 명(GET 또는 POST), Consumer가 호출한 HTTP URL 주소(매개변수 제외), 정규화한 매개변수를 '&'를 사용해 결합한다. 즉 ‘[GET|POST] + & + [URL 문자열로 매개변수는 제외] + & + [정규화한 매개변수]’ 형태가 된다.

4. 키 생성
3번 과정까지 거쳐 생성한 문자열을 암호화한다. 암호화할 때 Consumer Secret Key를 사용한다. Consumer Secret Key는 Consumer가 Service Provider에 사용 등록을 할 때 발급받은 값이다. HMAC-SHA1 등의 암호화 방법을 이용하여 최종적인 OAuth_signature를 생성한다.
