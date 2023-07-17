# coroutine

* 코루틴 사용 이유: 대량 네트워크 통신을 위함
* 대표 사례:

{% embed url="https://engineering.linecorp.com/ko/blog/server-side-kotlin-clova-skill-challenge" %}

* 개념

`runBlocking {}` 으로 코루틴 블록을 생성함

크게 두가지로 나눌수있다

```kotlin
val job : Job = launch {
    ...
} // 결과값 반환 x

val deferred : Deferred<T> = async {
    ...
}
deferred.await() // 결과값 반환
```



* Dispatcher

코루틴 디스패처(Dispatcher)는 코루틴이 실행되는 스레드나 스레드 풀을 제어하기 위한 메커니즘입니다. 코루틴 디스패처는 코루틴이 실행되는 컨텍스트를 관리하고, 어떤 스레드에서 코루틴이 실행될지 결정합니다. 다양한 디스패처가 제공되며, 각각의 디스패처는 특정한 실행 환경을 제공합니다.

1. `Default` 디스패처: 기본 디스패처로, CPU 바운드 작업에 적합한 기본 스레드 풀을 사용합니다. 일반적으로 I/O 작업이나 CPU 작업을 동시에 다루는 경우에 사용됩니다.
2. `Main` 디스패처: Android 애플리케이션에서 UI 스레드와 상호작용하기 위한 디스패처입니다. UI 이벤트를 처리하고 UI 업데이트를 수행하는 데 사용됩니다.
3. `IO` 디스패처: I/O 바운드 작업에 최적화된 디스패처입니다. 네트워킹이나 파일 입출력과 같은 블로킹 작업을 처리하기 위해 사용됩니다. 이 디스패처는 기본적으로 고정 크기의 스레드 풀을 사용하며, 적절한 수의 스레드를 유지하면서 블로킹 작업을 처리합니다.
4. `Unconfined` 디스패처: 코루틴을 호출한 스레드에서 실행되는 디스패처입니다. 즉, 디스패처의 영향을 받지 않고 현재 스레드에서 직접 실행됩니다. 이 디스패처는 어떤 스레드에서 실행될지 결정하지 않으므로 주의해서 사용해야 합니다.
5. 사용자 정의 디스패처: 필요에 따라 사용자가 직접 디스패처를 정의할 수도 있습니다. `CoroutineDispatcher` 인터페이스를 구현하고 필요한 실행 환경을 제공하는 클래스를 작성하여 사용할 수 있습니다.

디스패처는 `withContext` 함수를 사용하여 코루틴 범위 내에서 변경하거나 지정할 수 있습니다. 예를 들어, `withContext(Dispatchers.IO)`와 같이 사용하여 특정 디스패처에서 코루틴이 실행되도록 지정할 수 있습니다.

중요한 점은 코루틴 디스패처를 선택할 때 작업 유형과 실행 환경을 고려해야 한다는 것입니다. CPU 바운드 작업에는 `Default` 디스패처를 사용하고, I/O 바운드 작업에는 `IO` 디스패처를 사용하는 것이 일반적인 패턴입니다. 또한 UI 스레드와 상호작용해야 하는 경우에는 `Main` 디스패처를 사용해야 합니다.



* 시간비교

```kotlin
fun chunkAdoptRemind(postList: List<Post>): List<Post> {
    runBlocking(Dispatchers.IO) {
        postList.forEach { post ->
            launch {
                schedulerHandler.adoptRemind(post)
            }
        }
    }
    return postList
}

@Transactional(propagation = Propagation.REQUIRES_NEW)
fun adoptRemind(post: Post): Post {
    try {
        linkerService.adoptRemindPublish(post.service, postMapper.toDto(post)) // 네트워크 통신
        postRepository.updateRemind(post.id)
    } catch (_: Exception) {
        log.warn { "adopt-remind Job Failed postId: ${post.postId}" }
    }
    return post
}
```

1. 네트워크통신 메서드에 @Async 89ms -> 통신 성공실패를 알수없음
2. 동기통신 -> 90125ms
3. **coroutine 적용 -> 46016ms**

결론: 시간 약 2배 단축



Ref.

{% embed url="https://underdog11.tistory.com/entry/Kotlin-%EC%BD%94%EB%A3%A8%ED%8B%B4-Coroutine-async%EA%B3%BC-await-LifecycleScope%EA%B3%BC-ViewModelScope-3%ED%8E%B8" %}

{% embed url="https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%BD%94%EB%A3%A8%ED%8B%B4-%EC%A0%9C%EC%96%B4-5132380dad7f" %}

{% embed url="https://medium.com/hongbeomi-dev/coroutines-basic-e32053f18fdf" %}
