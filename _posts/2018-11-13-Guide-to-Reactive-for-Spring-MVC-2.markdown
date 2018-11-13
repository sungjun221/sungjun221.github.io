---
layout: post
title:  "Guide to 'Reactive' for Spring MVC Developers (2)"
date:   2018-11-13 20:00:00 +0900
categories: reactive spring java springmvc 리액티브 스프링 자바
---
(현재 작성중인 글입니다. Ver.0.1.2)

지난 글에서는 Spring에서 HTTP통신을 위해 기존에 사용하던 RestTemplate과 버전5부터 Reactive를 지원하기 위해 나온 WebClient를 살펴봤습니다. 이번에는 예제를 통해 이 둘이 어떻게 다르게 동작하는지 비교해 보겠습니다. 

예제는 서버 프로그램을 하나 띄우고, 클라이언트에서 각각 RestTemplate과 WebClient로 HTTP콜을 해보는 것입니다. 예제는 아래 Github Repository에서 다운 받으실 수 있어요. Spring Boot로 구성되어 Maven Update만 하시면 바로 테스트 해보실 수 있습니다. 

- [Demo Github Repository](https://github.com/sungjun221/reactive-for-webmvc)

<br>
먼저 서버프로그램은 간단한 구성입니다. Person의 데이터를 Map에 입력해두고 호출이 오면 전송합니다. 모든 호출에 Delay를 주어 네트워크 비용 등을 반영합니다.
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

<br>
그럼 이번엔 RestTemplate을 이용해 HTTP콜을 하는 클라이언트를 작성합니다. delay 파라미터에 값을 넘겨 2초간 딜레이를 줍니다.
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
    for (int i = 1; i <= 3; i++) {
      restTemplate.getForObject("/person/{id}", Person.class, i);
    }
    logTime(start);
  }

  private static void logTime(Instant start) {
    logger.debug("Elapsed time: " + Duration.between(start, Instant.now()).toMillis() + "ms");
  }
}
~~~
그럼 실행을 해볼까요? 아래와 같은 로그가 찍힙니다.
![Step1](image/path/url/image.png "Step1")  

정직한 결과가 나왔습니다. 2초 * 3번의 호출이 되어 약 6초를 조금 넘는 결과가 나왔습니다. 
응답이 올때까지 기다린 뒤 다시 요청하는 것을 알 수 있습니다.

<br>
그럼 이번엔 WebClient를 이용해보겠습니다.
~~~java
public class Step2 {
  private static final Logger logger = LoggerFactory.getLogger(Step2.class);
  private static WebClient client = WebClient.create("http://localhost:8081?delay=2");

  public static void main(String[] args) {
    Instant start = Instant.now();
    for (int i = 1; i <= 3; i++) {
      client.get().uri("/person/{id}", i)
          .retrieve()
          .bodyToMono(Person.class)
          .break();
    }
    logTime(start);
  }

  private static void logTime(Instant start) {
    logger.debug("Elapsed time: " + Duration.between(start, Instant.now()).toMillis() + "ms");
  }
}
~~~

<br>

- - -
* 참고
    - [SpringOne Platform 2018 - Guide to 'Reactive' for Spring MVC Developers](https://content.pivotal.io/springone-platform-2018/guide-to-reactive-for-spring-mvc-developers)