# OAuth
**(본 레포지토리는 OAuth 2.0에 대한 코드임을 미리 밝힙니다.)**

* OAuth란?  
**Open Authorization, Open Authentication** 라는 의미로 애플리케이션(페이스북,구글,트위터)(**Service Provider**)의 유저의 비밀번호를 Third party앱에 제공 없이 인증, 인가를 할 수 있는 오픈 스탠다드 프로토콜이다.<br>OAuth 인증을 통해 애플리케이션 API를 유저대신에 접근할 수 있는 권한을 얻을 수 있다.<br><br>OAuth가 사용되기 전에는 외부 사이트와 인증기반의 데이터를 연동할 때 인증방식의 표준이 없었기 때문에 기존의 기본인증인 아이디와 비밀번호를 사용하였는데, 유저의 비밀번호가 노출될 가망성이 크기 때문에 보안상 취약한 구조를 가지고 있었다.<br>그렇기에 이 문제를 보안하기 위해 OAuth의 인증은 API를 제공하는 서버에서 진행하고, 유저가 인증되었다는 Access Token을 발급하였다.<br>그 발급된 **Access token**으로 Third party(**Consumer**)애플리케이션에서는 Service Provider의 API를 안전하고 쉽게 사용할 수 있게 되었다.

<br><br>

## OAuth1 (=OAuth 1.0)

* OAuth1의 개념

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

* OAuth 1.0의 WorkFlow
![OAuth 1.0](https://user-images.githubusercontent.com/38516906/55337252-4132de80-54d9-11e9-9f9c-1602eac8bb04.png)
1. (그림에는 없지만) 가장 먼저 Consumer는 Service Provider로부터 Client key와 Secret을 발급 받아야한다. 이것은 Service Provider에 API를 사용할것을 등록하는것과 동시에 Service Provider가 Consmer를 식별할 수 있게 해준다.
2. 그림에 A 처럼 Request Token을 요청할 때 Consumer 정보, Signature 정보를 포함하여 Request token을 요청을하고 B의 흐름처럼 Request token을 발급받는다.
3. Request Token값을 받은후 Consumer는 C처럼 User를 Service Provider에 인증 사이트로 다이렉트시키고, 유저는 그곳에서 Service Provider에 유저임을 인증하게 된다.
4. 그러면 Consumer는 D의 정보처럼 해당 유저가 인증이되면 OAuth_token와 OAuth_verifier를 넘겨준다.
5. 그 이후에 Consumer는 OAuth_token와 OAuth_verifier받았다면 E의 흐름처럼 다시 서명을 만들어 Access Token을 요청하게 된다.
6. 그리고 Service Provider는 받은 토큰과 서명들이 인증이 되었으면 Access Token을 F의 정보 처럼 넘기게된다.
7. 그리고 그 Access Token 및 서명정보를 통해 Service Provider에 Protected Resource에 접근할 수 있게 된다.
