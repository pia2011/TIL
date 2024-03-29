# AOP 정리

## AOP 란?

AOP(Aspect-Oriented Programming)이란 이름 그대로 관점(관심) 지향 프로그래밍으로 **기존의
기능 중심에서 횡단 관심사 관점으로** 달리 보는 것이다.

횡단 관심사는 여러 곳에서 반복적으로 사용되는 부가 기능을 말한다.
<img width="811" alt="image" src="https://user-images.githubusercontent.com/53935439/210136578-7bc41bda-39b4-45eb-9f67-3fb4d43c51b8.png">

만약 100개의 클래스에 똑같은 부가 기능을 적용해야 한다면..? 그리고 그걸 또 수정해야 한다면..?
아주아주 피곤할 것이다. 이러한 문제를 해결하기 위해 부가 기능과 부가 기능을 어디에 적용할 지(포인트컷) 선택할 수 있는
기능을 하나로 합하여 모듈화하여 만든것이 Aspect이며 Aspect를 사용한 프로그래밍 방식이 AOP 이다.

쉽게 말해 AOP는 부가 기능과 핵심 기능을 분리하여 애플리케이션에서 반복적으로 사용되는 부가 기능을
모듈화하여 재사용하는 프로그래밍 방식이다.

대표적인 구현으로는 AspectJ 프레임워크가 있으며 스프링은 Aspect가 제공하는 문법을 차용하며,
일부 기능만 제공한다.

- OOP : 비즈니스 로직의 모듈
- AOP : 인프라 혹은 부가 기능의 모듈화


### AOP의 장점

- 애플리케이션에 흩어진 공통 기능을 한데 모아 관리하기 때문에 **유지보수**가 좋음
- 핵심 로직과 부가 기능을 명확하게 분리할 수 있음

## AOP의 적용방식

AOP 의 적용방식에는 3가지가 있으며 가장 많이 사용되는 방식은 **런타임 시점에 적용하는 방식**이다.

- 컴파일 시점
  - .java파일을 컴파일러를 통해 .class로 만드는 시점에 부가 기능 로직을 추가하는 방식
  - 모든 지점에 적용 가능
  - AspectJ가 제공하는 특별한 컴파일러를 사용해야 하기 때문에 특별할 컴파일러가 필요한 점과 복잡하다는 단
- 클래스 로딩 시점
  - .class 파일을 JVM 내부의 클래스 로더에 보관하기 전에 조작하여 부가 기능 로직 추가하는 방식
  - 모든 지점에 적용 가능
  - 특별한 옵션과 클래스 로더 조작기를 지정해야하므로 운영하기 어려움
- 런타임 시점
  - 스프링이 사용하는 방식
  - 실제 대상 코드는 그대로 유지되고 프록시를 통해 부가 기능이 적용
  - 컴파일이 끝나고 클래스 로더에 이미 다 올라가 자바가 실행된 다음에 동작하는 런타임 방식
  - 스프링 빈에만 AOP 적용 가능

## AOP 용어 

- Join point
  - 추상적인 개념 으로 advice가 적용될 수 있는 모든 위치를 한다.
  - ex) 메서드 실행 시점, 생성자 호출 시점, 필드 값 접근 시점 등등..
  - 스프링 AOP는 프록시 방식을 사용하므로 조인 포인트는 항상 메서드 실행 지점
- Pointcut
  - 조인 포인트 중에서 advice가 적용될 위치를 선별하는 기능
  - 스프링 AOP는 프록시 기반이기 때문에 조인 포인트가 메서드 실행 시점 뿐이 없고 포인트컷도 메서드 실행 시점만 가능
- Target
  - advice의 대상이 되는 객체
  - Pointcut으로 결정
- advice
  - 실질적인 부가 기능 로직을 정의하는 곳
  - 특정 조인 포인트에서 Aspect에 의해 취해지는 조치
- Aspect
  - advice + pointcut을 모듈화 한 것
  - @Aspect와 같은 의미
- Advisor
  - 스프링 AOP에서만 사용되는 용어로 advice + pointcut 한 쌍
- Weaving
  - pointcut으로 결장한 타겟의 join point에 advice를 적용하는 것
- AOP 프록시
  - AOP 기능을 구현하기 위해 만든 프록시 객체
  - 스프링에서 AOP 프록시는 JDK 동적 프록시 또는 CGLIB 프록시
  - 스프링 AOP의 기본값은 CGLIB 프록시

