---
title: "Guide to 'Reactive' for Spring MVC Developers (2)"
tags:
  - Java
  - Spring
  - Reactive  
last_modified_at: 2018-11-13T12:04:24-04:00
toc: true
---

지난 글에 이어 이번에는 RestTemplate과 WebClient의 차이점을 예제를 통해 살펴보겠습니다. 예제는 간단한 서버를 하나 띄우고, 클라이언트에서 각각 RestTemplate과 WebClient로 호출을 해보는 겁니다. 예제는 아래 Github Repository에서도 다운 받으실 수 있으니 참고해주세요. Spring Boot로 구성되어 있어 Maven Update만 하면 바로 테스트 해보실 수 있습니다.

- [Demo Github Repository](https://github.com/sungjun221/reactive-for-webmvc)

<br>

RestTemplate을 이용한 호출 예
-

먼저 서버프로그램입니다. Person의 데이터를 Map에 입력해두고 호출이 오면 전송합니다. 모든 호출에 Delay를 주어 네트워크 비용 등을 반영합니다.

~~~java
@Bean
public RouterFunction<?> routes() {
  return RouterFunctions.route()
    .GET("/person/{id}", request -> {
      Long id = Long.parseLong(request.pathVariable("id"));
      Map<String, Object> body = PERSON_DATA.get(id);
      return body != null ? ServerResponse.ok().syncBody(body) : NOT_FOUND;
    })
    .GET("/persons", request -> {
      List<Map<String, Object>> body = new ArrayList<>(PERSON_DATA.values());
      return ServerResponse.ok().syncBody(body);
    })
    .GET("/person/{id}/hobby", request -> {
      Long id = Long.parseLong(request.pathVariable("id"));
      Map<String, Object> body = HOBBY_DATA.get(id);
      return body != null ? ServerResponse.ok().syncBody(body) : NOT_FOUND;
    })
    .filter((request, next) -> {
      Duration delay = request.queryParam("delay")
          .map(s -> Duration.ofSeconds(Long.parseLong(s)))
          .orElse(Duration.ZERO);
      return delay.isZero() ? next.handle(request) :
          Mono.delay(delay).flatMap(aLong -> next.handle(request));
    })
    .build();
}
~~~

그럼 이번엔 RestTemplte을 이용하여 호출하는 클라이언트입니다. delay 파라미터에 값을 넘겨 호출마다 2초간 딜레이를 줍니다. 그럼 실행해 볼까요?

~~~java
public class Step1 {
  private static final Logger logger = LoggerFactory.getLogger(Step1.class);
  private static RestTemplate restTemplate = new RestTemplate();

  static {
    String baseUrl = "http://localhost:8081?delay=2";
    restTemplate.setUriTemplateHandler(new DefaultUriBuilderFactory(baseUrl));
  }

  public static void main(String[] args) {
    Instant start = Instant.now();
    for (int i = 1; i <= 5; i++) {
      restTemplate.getForObject("/person/{id}", Person.class, i);
    }
    logTime(start);
  }

  private static void logTime(Instant start) {
    logger.debug("Elapsed time: " + Duration.between(start, Instant.now()).toMillis() + "ms");
  }
}
~~~

### [실행결과]

![RestTemplate Example](https://user-images.githubusercontent.com/4060030/48671555-597b1000-eb6d-11e8-8a5f-ef2dd6fb3f67.png "RestTemplate Example")

참 정직한 결과가 나왔습니다. 호출마다 약 2초간씩 5번의 호출이 반복되어 약 10초를 조금 넘는 결과가 나왔습니다. 응답이 올 때까지 대기한 후 다음 호출을 했기 때문입니다. 

<br>
<br>

WebClient를 이용한 호출 예
-

### WebClient를 이용한 호출 예 1

그럼 이번엔 WebClient로 호출하는 예제를 살펴보겠습니다. 참고로 예제의 마지막 부분에 호출한 block 메소드는 WebFlux에서 가능하면 사용하지 말아야 할 메소드입니다. Non-Blocking I/O 기반인 WebFlux에서 block 메소드를 호출하면 WebFlux를 사용하는 의미가 없기 때문입니다. 

예제에서 사용한 이유는 비동기로 구독(Subscribe)하여 호출하면 비동기답게 호출만 하고 바로 돌아와버리기 때문입니다. 서버에 가서 2초간 딜레이를 받고 돌아오는 과정이 측정되지 않습니다. 그럼 예제의 내용대로 실행해보겠습니다. 

~~~java
public class Step2a {
  private static final Logger logger = LoggerFactory.getLogger(Step2a.class);
  private static WebClient client = WebClient.create("http://localhost:8081?delay=2");

  public static void main(String[] args) {
    Instant start = Instant.now();

    for (int i = 1; i <= 5; i++) {
      client.get().uri("/person/{id}", i)
          .retrieve()
          .bodyToMono(Person.class)
          .block();
    }

    logTime(start);
  }
  // ...
}
~~~

#### [실행결과]

![WebClient Example 01](https://user-images.githubusercontent.com/4060030/48671542-17ea6500-eb6d-11e8-9f8e-edf83e46df7e.png "WebClient Example 01")

..?? 뭔가 이상합니다. 비동기 호출을 했으니 동기호출했을 때보다 좀 더 나은 결과를 기대했는데 별반 다르지 않습니다. 로그를 살펴보니 for문으로 순차적으로 호출하고 있습니다. for문과 block메소드가 만나 Reactive 클라이언트인 WebClient를 동기적으로 동작하게 만들고 말았습니다.

### WebClient를 이용한 호출 예 2
이번엔 WebFlux의 기능을 제대로 활용해보겠습니다. Reactor의 객체인 Flux를 이용하여 호출합니다. Flux는 0-N개인 여러개의 결과를 처리하는 Reactor 객체입니다. 참고로 다른 Reactor 객체인 Mono는 0-1개의 결과를 처리합니다. 그럼 아래와 같은 예제를 실행해보겠습니다.

~~~java
public class Step2c {
  // ...
  public static void main(String[] args) {
    Instant start = Instant.now();

    Flux.range(1, 5)
        .flatMap(i -> client.get().uri("/person/{id}", i)
                .retrieve()
                .bodyToMono(Person.class))
        .blockLast();
    logTime(start);
  }
  // ...
}
~~~

#### [실행결과]

![WebClient Example 02](https://user-images.githubusercontent.com/4060030/48671483-38fe8600-eb6c-11e8-83d9-6bbbb2c68ae2.png "WebClient Example 02")

드디어 비동기의 진가가 나옵니다. 5개의 요청이 약 2초대에 실행되었습니다. 5개의 호출이 응답을 기다리지 않고 바로바로 실행했기 때문입니다. 여기서 한가지 더 눈여겨 볼 것이 있습니다. 로그에 보면 Reactor-http-nio 스레드가 생성된 것을 볼 수 있는데요. 5개의 요청을 했는데 4개의 스레드로 처리한 것을 알 수 있습니다. (스레드 번호 2,4,6,8) 기존의 방식이라면 요청마다 스레드가 필요하기 때문에 5개의 스레드가 필요합니다. 

### WebClient를 이용한 호출 예 3

조금 더 살펴보겠습니다. 이번엔 중첩된 요청을 해보겠습니다. Person의 id를 조회한 후. 해당인의 Hobby를 호출하는 예제입니다. 이번엔 어떤 결과가 나올까요? 자..실행해봅니다.

~~~java
public class Step2e {
  // ...
  public static void main(String[] args) {
    Instant start = Instant.now();

    Flux.range(1, 5)
        .flatMap(i -> client.get().uri("/person/{id}", i)
            .retrieve()
            .bodyToMono(Person.class)
            .flatMap(person -> client.get().uri("/person/{id}/hobby", i)
                .retrieve()
                .bodyToMono(Hobby.class)))
        .blockLast();
    logTime(start);
  }
  // ...
}
~~~

#### [실행결과]

![WebClient Example 03](https://user-images.githubusercontent.com/4060030/48671588-005fac00-eb6e-11e8-89a8-baae641fae6d.png "WebClient Example 03")

5번씩 2번, 총 10번의 호출이 발생하지만 약 4초의 시간만이 걸렸습니다. 비동기로 동작하여 2초씩 2번하여 4초대의 시간이 나온 것입니다. 이렇게 WebClient와 RestTemplate 동작의 차이점을 살펴봤습니다.

마무리
-

Spring WebFlux의 예제를 통해 Reactive의 동작방식과 장점을 살펴봤습니다. 하지만 모든 시스템에 Reactive가 필요한 것은 아닐 겁니다. 잘 돌아가는 기존 시스템을 Reactive로 전환하는 것보단 기존 문제점을 트러블슈팅 하는 것이 더 효과적일 것입니다. 그리고 개발 구성원이 Reactive에 대한 충분한 이해 없이 운영 시스템에 사용하는 것은 오히려 문제를 키울 수도 있겠죠.

하지만 Reactive의 시대가 다가오는 것은 확실한 것 같습니다. 시스템의 일부 스택이나 작은 시스템에 Reactive를 적용해보고 앞으로의 변화를 준비해보면 좋을 것 같습니다.

<br>

- - -
* 참고
  - [SpringOne Platform 2018 - Guide to 'Reactive' for Spring MVC Developers](https://content.pivotal.io/springone-platform-2018/guide-to-reactive-for-spring-mvc-developers)