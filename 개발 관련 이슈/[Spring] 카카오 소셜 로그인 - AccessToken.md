이번 프로젝트에서 회원 로그인 처리를 카카오 API를 활용해서 구현하기로 했다. 

### logic
![image](https://user-images.githubusercontent.com/53935439/160279216-6e5d7e7a-4852-4ac1-9b82-adc2441f545f.png)

로그인의 경우 위와 같은 로직으로 처리하기로 했고, 따라서 서버 쪽에서는 accessToekn을 가지고 회원정보를 받아와서 처리하면 되었다.

### Step1. 내 App 등록하기

![image](https://user-images.githubusercontent.com/53935439/160279425-63bde0d0-fa98-4f38-8afb-ae8d7d9cf2a4.png)

먼저 Kakao에 내 Application을 등록해주어야 한다. 이 과정을 거쳐야 추후에 내 앱으로 토큰을 발급받고 Kakao에 접근할 수가 있다.

### Step2. Token 발급 받기

나의 경우 서버단에서 TDD로 테스트를 하고 있기 때문에 테스트를 위한 AccessToken이 필요했다. 따라서 Kakao에 developers에서 토큰을 발급 받았다.

![image](https://user-images.githubusercontent.com/53935439/160279404-bf12324b-8398-4638-9f24-025e0a57db76.png)

카카오 로그인 API를 보면 다양한 목록을 확인할 수 있다. 나의 경우 사용자 정보 가져오기에서 Token을 발급 받았다.

### ⚠주의해야할 것⚠

이때 주의해야하는 것은 developers-sample 이 아니라 내 APP을 선택하고 토큰을 발급 받아야 한다는 것이다.(이것땜에 거의 몇시간을 헤맸는지 모르겠다..)

```
developers-samvle로 토큰을 발급 받으면, 이 홈페이지에서만! 동작한다.(사용자 정보 받아오기) 
아마 테스트용으로만 만드는 토큰이라 사용범위를 한정시켜둔 것 같다.. 
```

나 같은 경우 Application에서 자꾸 401 error(인증 관련 오류) 가 뜨길래, 코드 상의 문제인가 싶어서 한참을 헤맸었다.
그러다가 혹시 토큰 자체에 문제가 아닌가 싶어 PostMan으로 확인을 해봤는데..

PostMan을 통해서도 사용자 정보를 받아올 수 없는 것을 확인한 뒤, 구글링을 통해서 토큰의 발급 방식이 잘못된 것을 깨달았다.😭

### Step3. 코드로 구현하기

```java
public User findUserEmailByToken(String accessToken){

        final String RequestUrl = "https://kapi.kakao.com/v2/user/me";
        final HttpClient client = HttpClientBuilder.create().build();
        final HttpPost post = new HttpPost(RequestUrl);

        // add header
        post.addHeader("Authorization", "Bearer " + accessToken);

        JsonNode returnNode = null;

        try {
            final HttpResponse response = client.execute(post);
            final int responseCode = response.getStatusLine().getStatusCode();

            System.out.println("\nSending 'POST' request to URL : " + RequestUrl);
            System.out.println("Response Code : " + responseCode);

            // JSON 형태 반환값 처리
            ObjectMapper mapper = new ObjectMapper();
            returnNode = mapper.readTree(response.getEntity().getContent());


        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // clear resources
        }

        String id = returnNode.path("id").asText();
        String name = returnNode.path("properties").path("nickname").asText();
        String email = returnNode.path("kakao_account").path("email").asText();

        System.out.println(id+" "+name+" "+email);

        return User.builder()
                .socialId(id)
                .name(name)
                .email(email)
                .build();

    }
```

```
	implementation 'org.apache.httpcomponents:httpclient:4.5'
	implementation group: 'com.fasterxml.jackson.core', name: 'jackson-databind', version: '2.13.1'
	implementation group: 'com.googlecode.json-simple', name: 'json-simple', version: '1.1.1'
```

라이브러리의 경우 [mvnrepository](https://mvnrepository.com/) 에서 가장 사용자 수가 많은 안정화 버전을 사용했으며
JsonNode형식으로 데이터를 받아 처리해주었다.