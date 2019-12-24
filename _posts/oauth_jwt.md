---
title: Oauth2 + jwt란?
date: 2019-12-24
---

# oauth2 + jwt 의 이해
## oauth 2.0 이란?
- 웹, 앱 서비스에서 제한적으로 권한을 요청해 사용 할 수 있는 키를 발급 해 주는 것
- 외부 서비스의 인증 및 권한부여를 관리하는 범용 프레임워크

  
![Not found](https://t1.daumcdn.net/cfile/tistory/25238637583547EC0A "oauth flow")

### 초간단 정리
사용자가 웹페이지 로그인하여 인증이 성공하면 access token 발급해준다.  
사용자는 access token을 가지고 다른 리소스들을 사용할 수 있다.

## jwt란?
JSON 형태로 인증토큰을 만들어 통신할때 쓰는 인증방식  
(프로젝트에서 access token은 jwt를 사용할 것이다)  

# Reference
