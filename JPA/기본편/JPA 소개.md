## SQL 중심적인 개발의 문제점


## SQL에 의존적인 개발

현재 시대는 객체를 관계형 DB에 관리한다. 여기서 문제는 관계형 DB는 SQL로 CRUD가 이루어지므로 반복되고 지루한 코드가 필요하다.

예를 들어 객체에 필드를 추가할 때에도 아래와 같이 SQL에 하나하나 추가해주어야 한다.

![객체 필드](https://user-images.githubusercontent.com/53935439/154186330-6fb10ad8-b027-4bdb-bdda-7401089cdb80.PNG)

결국 SQL에 의존적인 개발을 피하기 힘들다.

### 패러다임의 불일치

객체를 보관하는 방법은 아래와 같이 다양하다. 

![객체 보관 방법들](https://user-images.githubusercontent.com/53935439/154186738-b33c0baa-3471-48da-9b1e-06bda374bb98.PNG)

그리고 현실적인 대안으로 사용되는 것은 RDB에 저장해서 관리하는 방법이다. 

![객체를 DB에 저장](https://user-images.githubusercontent.com/53935439/154186644-84e9de69-befe-4522-aac6-9894f720c2f5.PNG)

이렇게 관계형 DB를 통해 객체를 관리하려면 SQL을 통해서 소통할 수 밖에 없기때문에 개발자가 SQL을 하나하나 관리해주어야 한다.

물론 SQL문을 하나하나 관리해주어야 하는 것만의 문제는 아니다. 객체를 상속 관계를 RDB에서 구현하고자 한다면
자바 코드에서 몇줄이면 구현가능한 기능이 굉장히 복잡해진다. 연관관계를 구혀 하고자 할 때에도 객체가 참조를 사용하는 것과
달리 테이블은 외래 키를 사용하기 때문에 객체를 테이블에 맞춰서 모델링해야 하는데 이것도 상대적으로 복잡해진다. 거기에 더해
첫 실행하는 SQL에 따라 탐색 범위가 결정되기 때문에 엔티티의 신뢰 문제가 발생하기도 한다.

즉, 객체답게 모델링 할 수록 매핑 작업만 늘어나게 된다. 결과적으로 생산성이 떨어진다. 객체를 객체답게 사용하고
객체를 자바 컬렉션에 저장하듯이 DB에 저장하고 싶은 욕심에서 등장한 것이 바로 JPA이다.

## JPA 소개

JPA는 Java Persistence Api의 약자로 자바 진영의 ORM 기술 표준이다.

```
객체 관계 매핑의 약자인 ORM은 객체는 객체대로 설계하고 RDB는 RDB대로 설계할 때에
중간에서 매핑하는 역할을 한다. 대중적인 언어에는 대부분 ORM 기술이 존재한다.
```

### JPA 동작
![JPA 동작](https://user-images.githubusercontent.com/53935439/154198288-35ee0532-4257-4949-a0bc-555fba977c09.PNG)

### JPA 저장
![JPA 저장](https://user-images.githubusercontent.com/53935439/154198290-38e90cb2-bcc7-4d91-be36-c249feefdf8f.PNG)

### JPA 조회
![JPA 조회](https://user-images.githubusercontent.com/53935439/154198291-76505358-d264-4e16-8295-1a56626dd8b6.PNG)


### JPA의 사용 이유

* SQL 중심적인 개발에서 객체 중심으로 개발하기 위해
* 생산성 향상
  ![JPA 생산성](https://user-images.githubusercontent.com/53935439/154255862-c160cc42-5964-4692-9e32-4d83decb5543.PNG)
* 유지보수 향상
  ![JPA 유지보수](https://user-images.githubusercontent.com/53935439/154255869-da567b1e-8834-41e7-8fdb-97cdf42301bb.PNG)
* 패러다임 불일치 해결
  ![JPA 상속 저장](https://user-images.githubusercontent.com/53935439/154255854-4f2eb381-afea-4f84-a91a-657e24a0497b.PNG)
  ![JPA 상속 조회](https://user-images.githubusercontent.com/53935439/154255859-721aa21a-d0e1-4a43-8e57-393e9a668d13.PNG)
  ![JPA 연관관계](https://user-images.githubusercontent.com/53935439/154255868-04c62fcd-03af-4302-8fbb-35f1469ddc9d.PNG)
  ![JPA 신뢰성2](https://user-images.githubusercontent.com/53935439/154255864-c6a12b8f-4daa-49f5-84a9-d22394711222.PNG)
  ![JPA 엔티티 신뢰성](https://user-images.githubusercontent.com/53935439/154255866-9b7ef790-9319-4494-a59e-5e6ea143ca04.PNG)
* 성능
  ![캐시 동일성](https://user-images.githubusercontent.com/53935439/154256923-062ea0c4-b60a-4dba-bcbc-132c4f2cb1f8.PNG)
![트랜잭션 지원1](https://user-images.githubusercontent.com/53935439/154256795-ac8592ab-3beb-44e8-87e8-f8db0f52dbd2.PNG)
![트랜잭션2](https://user-images.githubusercontent.com/53935439/154256801-bf6fef44-5dac-4657-a67b-26e8a65c4d5a.PNG)
![즉시 로딩과 지연 로딩](https://user-images.githubusercontent.com/53935439/154256805-d7732e84-b5bd-4e03-bd2c-1c3bc8d135a8.PNG)

* 데이터 접근 추상화와 벤더 독립성
* 표준

