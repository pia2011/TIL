# ThreadLocal

## 쓰레드 로컬이란?

쓰레드 로컬은 해당 쓰레드만 접근할 수 있는 특별한 저장소를 말한다.

쉽게 이야기해서 물건 보관 창구를 떠올리면 된다. 
여러 사람이 같은 물건 보관 창구를 사용하더라도 창구 직원은 사용자를 인식해서 사용자별로 
확실하게 물건을 구분해준다.

### 일반적인 변수 필드
일반적인 변수 필드의 경우 여러 쓰레드가 같은 인스턴스의 필드에 접근하면 처음 쓰레드가 보관한 데이터가 사라질 수 있다.
즉, 동시성 문제가 발생할 수 있다.

<img width="807" alt="image" src="https://user-images.githubusercontent.com/53935439/209423889-7df70ba8-3271-4a4b-a781-f2181ea6c1e6.png">
<img width="808" alt="image" src="https://user-images.githubusercontent.com/53935439/209423922-1f43a835-c10a-4b5d-bcf0-0d9b92f3c826.png">
<img width="802" alt="image" src="https://user-images.githubusercontent.com/53935439/209424615-04c3801f-ba01-4932-8d28-d9cd9100a732.png">

이러한 경우 Thread-A 입장에서 저장한 데이터와 조회한 데이터가 다른 문제가 발생한다. 그리고 이처럼 여러 쓰레드에서 동시에 같은 인스턴스의 필드값을 변경하면서
발생하는 문제를 **동시성 문제**라고 한다.

동시성 문제는 이처럼 여러 쓰레드가 인스턴스 필드에 접근하는 상황에서 트래픽이 점점 많아질수록 자주 발생하게 된다.

특히 스프링 빈처럼 **싱글톤 객체의 필드를 변경**하며 사용할 때에는 이러한 동시성 문제를 항상 조심해야 한다. 

### 쓰레드 로컬
하지만 쓰레드 로컬의 경우 각 쓰레드마다 별도의 내부 저장소를 사용하기 때문에 같은 인스턴스에 로컬 필드에 접근하더라도 문제가 없다.

<img width="810" alt="image" src="https://user-images.githubusercontent.com/53935439/209423995-58a16440-dd56-499d-a5eb-d4c34b801853.png">

## 쓰레드 로컬 사용법

- 값 저장 : ThreadLocal.set()
- 값 조회 : ThreadLocal.get()
- 값 제거 : ThreadLocal.remove()

## 예제

이제 예시를 통해 쓰레드 로컬의 사용방법을 알아보자

```java
class ThreadLocalService{
    private ThreadLocal<String> nameStore = new ThreadLocal<>(); // ThreadLocal 사용으로 동시성 문제 해결
    public String logic(String name){
        nameStore.set(name);
        sleep(1000);
    }
    private void sleep(int millis){
        try{
            Thread.sleep(millis);
        }catch(InterruptedException e){
            e.printStackTrace();
        }
    }
}
class Test{
    private ThreadLocalService servie = new ThreadLocalService();
    @Test
    void ThreadLocal(){
        
        Runnable userA = ()->{
          service.logic("userA");  
        };
        Runnable userB = ()->{
          service.logic("userB");
        };
        
        Thread threadA = new Thread(userA);
        threadA.setName("thread-A");
        Thread threadB = new Thread(userB);
        threadB.setName("thread-B");

        threadA.start();
        sleep(100);
        threadB.start();
        sleep(2000);
    }
    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    
}
```

위 예제에서 ThreadA와 ThreadB가 모두 같은 인스턴스의 필드값 nameStore를 변경하는 로직을 수행하기 때문에
동시성 문제가 발생할 수 있다. 이때 해당 필드값을 ThreadLocal으로 선언하면 각각의 쓰레드마다 별도의 내부 저장소를 사용하기 때문에
동시성 문제를 해결할 수 있다.

## 주의사항

쓰레드 로컬을 사용할 때는 주의사항이 있다.

사용자의 요청이 끝날 때 쓰레드 로컬의 값을 ThreadLocal.remove() 를 통해서 꼭 제거해야 한다.
쓰레드 로컬을 사용할 때는 이 부분을 꼭! 기억해야 한다.

쓰레드 로컬의 값을 사용 후 제거하지 않고 그냥 두면 WAS(톰캣)처럼 쓰레드 풀을 사용하는 경우에 심각한 문제가 발생할 수 있다.

다음 예시를 보자

<img width="808" alt="image" src="https://user-images.githubusercontent.com/53935439/209425113-11ce6675-894b-4ac8-addd-e6db6c1222dd.png">
<img width="808" alt="image" src="https://user-images.githubusercontent.com/53935439/209425134-da7f3d1f-e733-4d4e-a372-c0f343f5e83a.png">
<img width="811" alt="image" src="https://user-images.githubusercontent.com/53935439/209425164-622bd8b7-09cf-4610-9ef0-2ae4bb6cc3ff.png">

만약 Thread-A의 쓰레드 로컬 값을 제거하지 않고
Thread-B가 데이터를 조회하게 될때 쓰레드 풀에서 thread-A 가 할당되게 된다면(물론 다른 쓰레드가 할당 될수도 있음) 사용자A값이 반환되게 되므로
결과적으로 사용자 B가 사용자 A의 데이터를 확인하게 되는 심각한 상황이 벌어질 수 있다.

그렇기 때문에 꼭 이부분을 주의해서 개발해야 한다.