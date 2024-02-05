
# Blocking/Non-Blocking
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/cbce8f47-6310-4d7e-a548-26f3c4020f32)


- `제어권` 의 반환여부를 통해 Blocking과 NonBlocking을 구분
- 제어권: 행동할 수 있는 권리

### Blocking 
- 명령수행을 위해 대상을 호출할 때, `제어권` 을 대상에게 넘기는 방식
    - 대상은 동작을 완전히 수행했을 때 제어권을 Return함
- 작업이 진행되는 동안 자신의 작업을 중단한 채 대기

### Non-Blocking 
- 명령수행을 위해 대상을 호출할 때, `제어권` 은 내가 계속 가지고 있는 방식
    - 대상은 제어권을 바로 Return하여 분리된 곳에서 작업을 처리함
    - 필요한 경우 추후에 반환값을 확인할 수 있는 값을 함께 Return (테이블링 앱처럼 동작)
- 작업이 진행되는 동안 Thread는 자신의 작업을 지속
- Blocking에 비해 상대적으로 난이도가 높음



# Sync/Async
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/354c4ea3-714f-4e73-8e18-b906177352de)

- `동시성` 을 기준으로 Synchronous와 Asynchronous를 구분
- 동시간대에 하나의 작업을 집중해서 처리하는가 vs 여러개의 작업을 동시(Concurrency)에 처리하는가
- 작업 완료를 기다릴지, 신경쓰지않고 다른 일을 하고 있을지의 차이

### Sync
- 동시간대에 하나의 작업을 처리하는 것
- Task가 실행되고 완료되는 것을 지켜보면서 기다린다.

### Async
- 동시간대에 여러개의 작업을 동시에 처리하는 것
- Task가 실행되면 신경쓰지않고 다른 일을 진행한다. 
    - Task는 작업이 완료되면 콜백(또는 이벤트)를 발생시켜 해당 사실을 알린다. (Notify)



# 각 케이스
- 식사를 한다고 할 때, 매장에서 주문(Blocking)과 배달 주문(Non-Blocking)으로 구분
- 음식이 나오는 걸 신경쓰는지에 따라, Sync와 Async로 구분

## Blocking + Sync (매장주문 + 기다리기)
> 매장 주문 -> 나올 때까지 기다리기 -> 먹기

![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/10bf6df8-db83-4d52-bbb2-4eee23401899)

```java
System.out.println("매장 주문");
Thread.sleep(2000); // 주문이 처리되는데 2초가 걸린다고 가정
System.out.println("먹기");
return "식사 완료";
```

-  주문을 한 후, 주문 처리가 완료될 때까지 대기합니다.
- 주문 처리가 완료되기 전까지는 다른 작업을 수행하지 않으며, 대기시간이 발생할 수 있습니다.
- e.g.Spring JDBC + MySQL


## Blocking + Async (매장주문 + 금방나와요!) 
> 매장 주문 -> 할 일 하기 (금방 나오니깐 잠시만 기다리라고 해서 그냥 대기) -> 먹기
  ![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/8177055f-d467-465d-af02-f800ba88e3ba)

```java
System.out.println("매장 주문");
Future<String> asyncTask = executor.submit(() -> {
    Thread.sleep(2000); // 주문이 처리되는데 2초가 걸린다고 가정
    System.out.println("먹기");
    return "식사 완료";
});
System.out.println("할 일 하기 (금방 나오니깐 잠시만 기다리라고 해서 그냥 대기)");
return asyncTask.get();
```

- 주문을 한 후, 주문 처리를 별도의 스레드에서 비동기적으로 처리합니다.
- 주문 처리가 완료될 때까지 다른 작업을 수행할 수 있습니다.
- 주문 처리 결과를 얻기 위해 `Future.get()` 메서드를 호출하면, 필요한 시점에 결과가 준비되기 전까지는 대기합니다.
- e.g. Node.js + MySQL


## NonBlocking + Sync
> 배달 주문 -> 언제 도착하는지 새로고침 계속하기 -> 도착과 동시에 수령하고 먹기

![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/3e4daf7c-024a-4fac-95c6-c6191502a9b4)

```java
System.out.println("배달 주문");
for (int i = 1; i <= 5; i++) {
    Thread.sleep(1000); // 1초마다 상태 확인을 위해 새로고침
    System.out.println("도착 예정 시간 확인...");
}
System.out.println("도착과 동시에 수령하고 먹기");
return "식사 완료";
```

- 주문을 한 후, 주문 처리 상태를 주기적으로 확인하면서 다른 작업을 수행합니다.
- 주문 처리가 완료될 때까지 주기적으로 상태를 확인하며, 대기하지 않고 다른 작업을 수행할 수 있습니다.
- 주문 처리가 완료되면 즉시 결과를 얻을 수 있습니다.
- e.g. Spring JDBC + MongoDB


## NonBlocking + Async 
> 배달 주문 -> 할 일 하기 -> 배달원이 노크 -> 받아서 먹기
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/193f28a6-8ada-4bd1-9b36-26705463e587)


```java
System.out.println("배달 주문");
System.out.println("할 일 하기");
CompletableFuture<String> asyncTask = CompletableFuture.supplyAsync(() -> {
    try {
        Thread.sleep(2000); // 주문이 처리되는데 2초가 걸린다고 가정
        System.out.println("배달원이 노크");
        return "식사 완료";
    } catch (InterruptedException e) {
        e.printStackTrace();
        return "오류 발생";
    }
}).thenRun(() -> System.out.println("잘먹었습니다 ^_^"));
return asyncTask.get();
```

- 주문을 한 후, 주문 처리를 별도의 비동기 작업으로 처리하면서 다른 작업을 수행합니다.
- 주문 처리 상태를 확인하는 동안 다른 작업을 수행할 수 있으며, 주문 처리 결과를 얻기 위해 `CompletableFuture.get()` 메서드를 호출하면 필요한 시점에 결과가 준비될 때까지 대기하지 않습니다.
    - CompletableFuture는 Future와 다르게 기본 Return값 및 예외처리 제공, CallBack 제공
- e.g. Node.js + MongoDB


# 전체 케이스 정리
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/7bc7425e-dd67-4440-b7e4-0712d22df4b1)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/69d82d3d-6de3-46ef-a2a4-3d631216fde7)



(IBM 자료)



# 예상 질문
1. 동기(Synchronous)와 비동기(Asynchronous)의 차이점은 무엇인가요?

- 동기적 작업은 호출한 함수가 작업이 완료될 때까지 대기하는 방식입니다. 작업이 완료되기 전까지 다른 작업을 수행하지 않습니다.
- 반면 비동기적 작업은 호출한 함수가 작업을 요청한 후에도 다른 작업을 수행할 수 있고, 작업이 완료되면 알림을 받아 처리할 수 있는 방식입니다.

2. 동기적인 작업과 비동기적인 작업을 구분하는 기준은 무엇인가요?

- 작업을 요청한 후 결과를 기다리면서 다른 작업을 수행할 수 있는지 여부입니다. 동기 작업은 결과를 기다리는 동안 다른 작업을 수행할 수 없지만, 비동기 작업은 결과를 기다리는 동안 다른 작업을 수행할 수 있습니다.

3. 동기 방식으로 동작하는 코드의 예시는 어떤 것들이 있을까요?

- 함수를 호출하고 반환값을 받을 때까지 대기하는 경우입니다. 예를 들어, 파일을 읽어오는 함수를 호출하고 읽어온 데이터를 반환받을 때까지 대기하는 것이 동기 방식입니다.

4. 비동기 방식으로 동작하는 코드의 예시는 어떤 것들이 있을까요?

- 비동기적 작업에는 콜백 함수, 프로미스, async/await 등을 사용할 수 있습니다. 예를 들어, 비동기적으로 데이터를 가져오는 함수를 호출하고 나중에 콜백 함수가 호출되어 데이터를 처리하는 것이 비동기 방식입니다.

5. 동기와 비동기 방식 중 어떤 것을 선택해야 할 때가 있을까요?

- 작업의 복잡성과 응답 시간을 고려해야 합니다. 동기 방식은 작업이 순차적으로 처리되어야 하거나 응답 시간이 짧을 때 유리하며, 비동기 방식은 작업이 독립적이거나 응답 시간이 길 때 유리합니다.

6. 동기적인 작업에서 발생할 수 있는 문제점은 무엇인가요?

- 동기 작업은 작업이 완료될 때까지 대기하므로 작업이 느리면 전체 시스템의 응답 시간이 느려질 수 있습니다. 또한, 하나의 작업이 실패하면 다른 작업도 영향을 받을 수 있습니다.

7. 비동기적인 작업에서 발생할 수 있는 문제점은 무엇인가요?

- 비동기 작업은 작업의 순서가 보장되지 않으므로 예상치 못한 결과가 발생할 수 있습니다. 또한, 콜백 지옥(callback hell)이 발생할 수 있고, 작업이 복잡해질수록 코드의 가독성과 유지보수성이 떨어질 수 있습니다.

8. 동기와 비동기 방식을 혼합하여 사용할 수 있는 방법은 있을까요?

- 동기 작업을 비동기적으로 처리하는 방법으로는 멀티스레딩, 이벤트 기반 프로그래밍 등이 있습니다. 예를 들어, 동기 작업을 백그라운드 스레드에서 수행하고 결과를 비동기적으로 받아 처리할 수 있습니다.

9. 비동기 작업을 처리하기 위해 주로 사용되는 기술이나 패턴은 무엇인가요?

- 비동기 작업을 처리하는 기술로는 콜백 함수, 프로미스, async/await, RxJS 등이 주로 사용됩니다.

10. 동기와 비동기 방식을 선택할 때 고려해야 할 요소는 무엇인가요?

- 작업의 종류, 작업의 복잡성, 응답 시간, 시스템의 성능 등을 고려해야 합니다. 동기 방식은 간단한 작업이나 응답 시간이 짧을 때 유리하며, 비동기 방식은 작업이 복잡하거나 응답 시간이 길 때 유리합니다.



# 참고자료
https://developer.ibm.com/articles/l-async/
http://asfirstalways.tistory.com/348
https://luminousmen.com/post/asynchronous-programming-blocking-and-non-blocking
https://velog.io/@brucehan/CompletableFuture-%EC%A0%95%EB%A6%AC
