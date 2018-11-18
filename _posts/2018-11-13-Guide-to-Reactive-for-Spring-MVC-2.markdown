---
title: "Guide to 'Reactive' for Spring MVC Developers (2)"
tags:
  - Java
  - Spring
  - Reactive  
last_modified_at: 2018-11-13T12:04:24-04:00
toc: true
---
> 현재 작성중인 글입니다.

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

#### [실행결과]

![RestTemplate Example](https://user-images.githubusercontent.com/4060030/48671555-597b1000-eb6d-11e8-8a5f-ef2dd6fb3f67.png "RestTemplate Example")

참 정직한 결과가 나왔습니다. 호출마다 약 2초간씩 5번의 호출이 반복되어 약 10초를 조금 넘는 결과가 나왔습니다. 응답이 올 때까지 대기한 후 다음 호출을 했기 때문입니다. 

<br>
<br>

WebClient를 이용한 호출 예
-

### WebClient를 이용한 호출 예 1

그럼 이번엔 WebClient로 호출하는 예제를 살펴보겠습니다. 참고로 예제의 마지막 부분에 호출한 block 메소드는 WebFlux에서 가능하면 사용하지 말아야 할 메소드입니다. Non-Blocking I/O 기반인 WebFlux에서 block 메소드를 호출하면 WebFlux를 사용하는 의미가 없기 때문입니다. 

예제에서 사용한 이유는 비동기로 구독(Subscribe)하여 호출하면 비동기답게 호출만 하고 바로 돌아와버리기 때문입니다. 서버에 가서 2초간 딜레이를 받고 돌아오는 과정이 측정이 되지 않습니다. 그럼 예제의 내용대로 실행해보겠습니다. 

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

![WebClient Example 01](https://user-images.githubusercontent.com/4060030/48671542-17ea6500-eb6d-11e8-9f8e-edf83e46df7e.png "WebCLient Example 01")

흠..? 뭔가 이상합니다. 

### WebClient를 이용한 호출 예 2

블라블라

~~~java
public class Step2b {
  // ...
  public static void main(String[] args) {
    Instant start = Instant.now();

    List<Mono<Person>> list = Stream.of(1, 2, 3, 4, 5)
        .map(i -> client.get().uri("/person/{id}", i)
                .retrieve()
                .bodyToMono(Person.class))
        .collect(Collectors.toList());

    Mono.when(list).block();
    logTime(start);
  }
  // ...
}
~~~

#### [실행결과]

![WebClient Example 02](https://user-images.githubusercontent.com/4060030/48671443-a4942380-eb6b-11e8-84aa-56cf5ab3166f.png "WebCLient Example 02")

블라블라

### WebClient를 이용한 호출 예 3

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

![WebClient Example 03](https://user-images.githubusercontent.com/4060030/48671483-38fe8600-eb6c-11e8-83d9-6bbbb2c68ae2.png "WebClient Example 03")

블라블라

### WebClient를 이용한 호출 예 4

블라블라

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

![WebClient Example 04](https://user-images.githubusercontent.com/4060030/48671588-005fac00-eb6e-11e8-89a8-baae641fae6d.png "WebClient Example 04")

블라블라

<br>

- - -
* 참고
  - [SpringOne Platform 2018 - Guide to 'Reactive' for Spring MVC Developers](https://content.pivotal.io/springone-platform-2018/guide-to-reactive-for-spring-mvc-developers)