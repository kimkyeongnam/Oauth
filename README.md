# OAuth
**(본 레포지토리는 OAuth 2.0에 대한 코드임을 미리 밝힙니다.)**

* **OAuth란?**  
**Open Authorization, Open Authentication** 라는 의미로 애플리케이션(페이스북,구글,트위터)(**Service Provider**)의 유저의 비밀번호를 Third party앱에 제공 없이 인증, 인가를 할 수 있는 오픈 스탠다드 프로토콜이다.<br>OAuth 인증을 통해 애플리케이션 API를 유저대신에 접근할 수 있는 권한을 얻을 수 있다.<br><br>OAuth가 사용되기 전에는 외부 사이트와 인증기반의 데이터를 연동할 때 인증방식의 표준이 없었기 때문에 기존의 기본인증인 아이디와 비밀번호를 사용하였는데, 유저의 비밀번호가 노출될 가망성이 크기 때문에 보안상 취약한 구조를 가지고 있었다.<br>그렇기에 이 문제를 보안하기 위해 OAuth의 인증은 API를 제공하는 서버에서 진행하고, 유저가 인증되었다는 Access Token을 발급하였다.<br>그 발급된 **Access token**으로 Third party(**Consumer**)애플리케이션에서는 Service Provider의 API를 안전하고 쉽게 사용할 수 있게 되었다.

<br><br>

## OAuth1 (=OAuth 1.0)

## OAuth2 (=OAuth 2.0)
