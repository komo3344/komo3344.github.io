---
title: Oauth2 + jwt란?
date: 2019-12-24
---

# oauth2 + jwt 의 이해
내가 이해한 내용들만 작성하였다.  
더 자세한 내용들은 알고 싶다면 잘 정리되어 있는 블로그 링크를 걸어두었다.  

## oauth 2.0 이란?
- 웹, 앱 서비스에서 제한적으로 권한을 요청해 사용 할 수 있는 키를 발급 해 주는 것
- 외부 서비스의 인증 및 권한부여를 관리하는 범용 프레임워크  
[oauth 2.0에 대해 잘 설명되어 있는 블로그]
  
![Not found](https://t1.daumcdn.net/cfile/tistory/25238637583547EC0A "oauth flow")

### 초간단 정리
사용자가 웹페이지 로그인하여 인증이 성공하면 access token 발급해준다.  
사용자는 access token을 가지고 다른 리소스들을 사용할 수 있다.

## jwt(Json Web Token)란?

JSON 형식의 body를 가지며, 인증토큰을 만들어 통신할때 쓰는 인증방식    
[jwt에 대해 더 자세히 보기]

# Reference
<https://interconnection.tistory.com/76>
<https://bcho.tistory.com/999>

[oauth 2.0에 대해 잘 설명되어 있는 블로그]: https://interconnection.tistory.com/76
[jwt에 대해 더 자세히 보기]: https://bcho.tistory.com/999
