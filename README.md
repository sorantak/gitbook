# coroutine

* 코루틴 사용 이유: 대량 네트워크 통신을 위함
* 대표 사례:

[Kotlin으로 서버사이드 개발과 Clova Skill Award 도전!](https://engineering.linecorp.com/ko/blog/server-side-kotlin-clova-skill-challenge)

* 개념

`runBlocking {}` 으로 코루틴 블록을 생성함

크게 두가지로 나눌수있다

```kotlin
val job : Job = launch {
    ...
}
val deferred : Deferred<T> = async {
    ...
}
```

* 시간비교

1. 네트워크통신 메서드에 @Async 89ms -> 통신 성공실패를 알수없음
2. 동기통신 -> 90125ms
3. coroutine 적용 -> 46016ms

결론: 시간 약 2배 단축



Ref.

[https://underdog11.tistory.com/entry/Kotlin-코루틴-Coroutine-async과-await-LifecycleScope과-ViewModelScope-3편](https://underdog11.tistory.com/entry/Kotlin-%EC%BD%94%EB%A3%A8%ED%8B%B4-Coroutine-async%EA%B3%BC-await-LifecycleScope%EA%B3%BC-ViewModelScope-3%ED%8E%B8)

[https://medium.com/@limgyumin/코틀린-코루틴-제어-5132380dad7f](https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%BD%94%EB%A3%A8%ED%8B%B4-%EC%A0%9C%EC%96%B4-5132380dad7f)
