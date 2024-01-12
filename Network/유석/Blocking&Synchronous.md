# Blocking / Non-Blocking
Blocking과 Non-Blocking을 나누는 차이는 다른 주체가 작업할 때 자신의 제어권이 있는지 없는지로 구분한다. 

## Blocking
Blocking은 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 다른 작업이 끝날 때까지 기다렸다가 자신의 작업을 시작하는 것을 의미한다.
> 신입 개발자: 팀장님 작업한 자료 가지고 왔습니다. 검토 부탁드립니다. <br>
Blocking 팀장: 자료 검토할 동안 옆에서 기다리고 있으세요.

```
A: B 호출한 함수 
B: 호출을 받아 제어권을 A에게서 건네받은 함수 
```
즉, A가 B가 할 일을 모두 마칠때까지 제어권을 계속 가지고 있고 A에게 바로 제어권을 돌려주지 않고 B의 메서드내에 모든 작업이 끝난 후에 A에게 제어권과 반환값을 동시에 준다.

## Non-Blocking
Non-Blocking은 다른 주체의 작업에 관계없이 자신의 작업을 계속 하는 것을 의미한다.
> 신입 개발자: 팀장님 작업한 자료 가지고 왔습니다. 검토 부탁드립니다.
Blocking 팀장: 자료 검토후에 다시 부를테니 하던 작업하고 계세요.

즉, B가 할 일을 모두 마치지 않았더라도 바로 제어권을 A에게 건네주어 A가 다른 일을 진행할 수 있도록 한다. 그리고 A가 일정시간이 지난 후 한번씩 B에게 작업이 다 끝났는지 물어본다. 그래서 A는 작업하는 도중에 한번씩 B에게 작업완료 여부를 물어보고 작업은 계속 진행중이고 만약 B가 작업이 완료되었다고 하면 결과를 받아와서 마저 작업을 진행한다.


# Sync / Async
단어 자체로 접근을 먼저 해봤을 때, Syn는 together이라는 뜻이고, chrono는 time이다. 즉, Synchronous는 함께 시간을 맞춘다는 뜻으로 해석된다. 반면에 Asynchronous는 앞에 A라는 접두사가 붙어 부정하는 형태가 되어 시간을 맞추지 않는 것이라 해석할 수 있다.

## Synchronous(동기)
동기는 두 가지 이상의 대상(함수, 어플리케이션 등)이 서로 시간을 맞춰 행동하는 것이다.
어떤 대상 A와 B가 있을 때 동기적으로 처리하는 방법 두 가지가 있다.


A와 B가 시작 시간 또는 종료 시간이 일치하면 동기이다.
> - A,B 쓰레드가 동시에 작업을 시작하는 경우
![](https://velog.velcdn.com/images/gkdlzjaqor92/post/4f90725a-3ff3-4af7-ab76-10cc853d3bd0/image.png)
> - 메서드 리턴 시간(A)과 결과를 전달받는 시간(B)이 일치하는 경우
![](https://velog.velcdn.com/images/gkdlzjaqor92/post/9b2ad2f1-d2d4-4821-a4ac-3e8855e0dcf9/image.png)

## Asynchronous(비동기)
비동기는 동기와 반대로 대상이 서로 시간을 맞추지 않는 것을 말한다. 예를 들어 호출하는 함수가 호출되는 함수에게 작업을 맡겨놓고 신경을 쓰지 않는 것을 말한다. 

# Blocking, Non-Blocking IO
## Blocking & Synchronous
![](https://velog.velcdn.com/images/gkdlzjaqor92/post/0ef9f6b3-cdcc-4032-ad5a-c93b5b913c06/image.png)

## Blocking & Asynchronous
![](https://velog.velcdn.com/images/gkdlzjaqor92/post/bb844fdc-07b3-483a-9453-cb9751d860df/image.png)

## Non-Blocking & Synchronous
![](https://velog.velcdn.com/images/gkdlzjaqor92/post/85159d98-9487-47af-a1d4-00e5c790af9a/image.png)

## Non-Blocking & Asynchronous
![](https://velog.velcdn.com/images/gkdlzjaqor92/post/cebd24e3-5946-4ea3-bbc5-8c47a5a7c9a8/image.png)


# 조합별 상황(백엔드 vs 프론트엔드)
프론트엔드 개발자가 백엔드 개발자에게 API를 만들어달라고 요청하는 상황이다.

## Blocking & Synchronous
> Front: 회원가입 API 만들어주세요.<br>
Back: 네! 금방 끝나니깐 자리로 가지말고 잠깐만 기다려줘요.<br>
Back: (Create...Validation...duplication...)<br>
Front: (과정 지켜보는 중.. 나도 나중에 백엔드로 이직하고 싶으니깐 자세히 봐야겠다. )

## Blocking & Asynchronous
> Front: 회원가입 API 만들어주세요.<br>
Back: 네! 금방 끝나니깐 자리로 가지말고 잠깐만 기다려줘요.<br>
Back: (Create...Validation...duplication...)<br>
Front: (별로 안 궁금한데.. 아까 짰던 코드가 어떻게 구성됐더라.. 나 빨리 개발해야 하는데..)

## Non-Blocking & Synchronous
> Front: 회원가입 API 만들어주세요.<br>
Back: 네! 만들어드릴테니 하던 작업하시면 될 것 같아요.<br>
Front: 네! (자리로 감.)<br>
Back: (Create...Validation...duplication...)<br>
Front: 끝나셨나요?<br>
Back: 아직이요!<br>
Front: 끝나셨나요?<br>
Back: 아직이요..!<br>
Front: 끝나셨나요?<br>
Back: 아직이요!!!!!!

## Non-Blocking & Asynchronous
> Front: 회원가입 API 만들어주세요.<br>
Back: 네! 만들어드릴테니 하던 작업하시면 될 것 같아요.<br>
Front: 네! (자리로 감.)<br>
Back: (Create...Validation...duplication...)<br>
Front: (열일중..)<br>
Back: API 만들어서 서버 배포해놨습니다~<br>
Front: 감사합니다!

#### References
https://musma.github.io/2019/04/17/blocking-and-synchronous.html <br>
https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx <br>
https://velog.io/@maketheworldwise/SyncAsync-BlockingNon-Blocking-%EB%AC%B4%EC%8A%A8-%EC%B0%A8%EC%9D%B4%EC%9D%BC%EA%B9%8C <br>
https://limdongjin.github.io/concepts/blocking-non-blocking-io.html#ibm-%E1%84%8B%E1%85%A1%E1%84%90%E1%85%B5%E1%84%8F%E1%85%B3%E1%86%AF
