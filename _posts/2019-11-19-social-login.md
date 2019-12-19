---
title: 소셜로그인(카카오, 페이스북, 네이버)
date: 2019-11-19
---

# 소셜로그인 데이터 흐름  

1. React에서 jskey를 사용
2. 로그인시 회원정보를 가져오고 이 정보를 서버로 전달
3. 서버에서 회원정보를 받아 

  - 회원가입이 안되어있으면 회원가입 시킴
  - 회원가입이 되어있으면 로그인 시킴

** 카카오

로그인 버튼 클릭 시 로그인 할 수 있는 폼을 띄움

```
Kakao.Auth.loginForm()
```

```
<Kakao
  jsKey=' *your javascript key* '
  onSuccess={this.responseKakao}
  onFailure={this.responseFail}
/>
```

로그아웃 버튼 클릭 시 로그아웃

```
Kakao.Auth.logout()
```
> 로그아웃을 토큰을 만료시키는 기능을 하고 새로 로그인을 하려면 로그인 폼을 띄워야함
(카카오에서 토큰만료 -> 토큰을 null로 바꾸는 것으로 수정했다고 함)

* * *

** 페이스북

로그아웃 버튼 클릭 시 로그아웃 
```
FB.logout()
```
