# JWT

## JWT 란

+ Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token
+ 토큰 자체를 정보로 사용하는 Self-Contained 방식으로 정보를 안전하게 전달
+ 주로 회원 인증이나 정보 전달에 사용됨

## JWT 구조

![image](https://user-images.githubusercontent.com/53935439/207863293-ada28dcb-96a1-4f82-a969-8ecf024338c0.png)

+ Header, Payload, Signature 이렇게 3 부분으로 나누어짐
+ 각 부분은 Base64 URL로 인코딩 되어 표현됨

### header
+ typ, alg 두가지 정보로 구성됨
  + typ : 토큰의 타입을 지정함 ex) JWT
  + alg : 알고리즘 방식을 지정함, 서명 및 토큰 검증에 사용 ex) HS256, RSA
```
    {
        "alg": "HS256",
        "typ": JWT
    }
```

### payload
+ 토큰에서 사용될 정보의 조각들인 Claim이 담겨 있음
+ Json 형태로 다수의 정보를 넣을 수 있음


### signature
+ 토큰을 인코딩하거나 유효성 검사를 할 때 사용하는 고유한 암호화 코드
+ Header 와 Payload를 Base64URL로 인코딩하고, 인코딩 한 값을 비밀키를 이용해서 Header에서 정의한 알고리즘으로 해싱하고 이 값을 다시 인코딩해서 생산

## 단점

+ 토큰을 탈취당하면 대처하기 어려움
+ Payload 자체는 암호화 되지 않기 때문에 유저의 중요한 정보를 담을 수 없음
+ 인증 요청이 많아질 수록 네트워크 부하가 심해질 수 있음 (토큰 자체의 데이터가 길어서..)

## 장점

+ 확장성이 우수함
+ 별도의 저장소가 필요하지 않음
+ stateless 하기 때문에 서버 부하를 줄일 수 있고, 서버의 확장성이 우수함
+ 모바일 환경에서도 사용이 가능함 (세션은 불가능)

## JWT의 Access Token / Refresh Token 방식

- JWT의 경우 토큰을 탈취 당할 위험이 있음
- 따라서 있는 그대로 사용하는 것이 아닌 2개의 만료 시간이 다른 토큰을 사용해 위험 부담을 줄일 수 있음
- ATK : 클라이언트가 갖고 있는 실제 유저의 담긴 토큰 / 만료시간 짧음
- RTK : 오로지 ATK를 발급해주기 위해 존재하는 토큰
- ATK를 헤더에 포함하여 인증이 가능
- ATK 탈취 시 유효기간이 짧기 때문에 (대략 30분 정도) 위험부담을 줄 일 수 있음