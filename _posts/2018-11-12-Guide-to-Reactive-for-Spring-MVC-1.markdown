---
title: "Guide to 'Reactive' for Spring MVC Developers (1)"
categories: 
  - post
last_modified_at: 2018-11-12T12:04:24-04:00
toc: true
---

최근 몇 년간 애플리케이션이 점점 커지고 복잡해지면서 Asynchronous, Non-Blocking I/O 기반의 Reactive System에 대한 관심이 높아지고 있습니다.

특히 Spring Framework 5부터 Spring WebFlux란 이름으로 Reactive에 대한 지원이 시작되었는데요. 더이상 Java 개발자에게도 낯설지 않은 이야기가 되었습니다. 올해 SpringOne Platform에서는 기존 Spring 개발자를 대상으로 한 Reactive 가이드 세션이 진행되었는데요, 부족하지만 그 내용을 이번 글을 통해 공유하고자 합니다.

먼저 Reactive가 무엇인지 궁금하신 분은 아래 링크를 참고하셔요.

- [Reactive Programming](http://wiki.sys4u.co.kr/pages/viewpage.action?pageId=7766819)

<br><br>

대부분의 Reactive 이야기는 Full Stack에 대한 것이다
-

보통 Reactive에 대한 이야기는 Full Stack에 대한 것입니다. Async와 Non-Blocking I/O의 이점을 최대한 누리는 방법이죠. 그럼 먼저 Full Stack의 Reactive를 살펴볼까요?

![Spring WebFlux](https://user-images.githubusercontent.com/4060030/48329064-53c38d00-e68a-11e8-823f-56bce05a06c5.PNG "Spring WebFlux")  
우리는 Spring WebFlux를 이용하여 완전하게 Async, Non-Blocking I/O에 기반한 환경을 구성할 수 있습니다. 더 적은 스레드로 요청(Request)과 응답(Response)을 처리할 수 있구요. 때때로 더 많은 스레드 스위칭이 발생하여 속도가 저하될 수 있으나 궁극적으로 Reactive가 가지고 있는 동시성(Cuncurrency)모델의 장점이 더 효과적이고, 확장가능하고, 탄력성(Resilient)이 있습니다. 기존에 서블릿 컨테이너가 하나의 스레드에 특정한 요청을 할당하여 동작하고, Blocking I/O에 기반하여 움직이던 것과는 다른 방식이죠. 

하지만 장점이 많다고 모두에게 이런 구성이 반드시 필요한 것은 아닙니다. 또한 기존 애플리케이션을 Full Stack Reactive 환경으로 전환하는 것은 현실적으로 쉽지 않습니다.

<br><br>

어떻게 기존 애플리케이션에 Reactive의 가치를 더할까? 
-

그렇다면 어떻게 리스크를 최소화하면서 기존 애플리케이션에 Reactive의 장점을 더할 수 있을까요?

![Spring MVC](https://user-images.githubusercontent.com/4060030/48329150-af8e1600-e68a-11e8-879a-33cdf69edb35.PNG "Spring MVC")  
우리가 그동안 사용해왔던 Spring MVC는 Blocking I/O 기반에 Synchronous한 서블릿 컨테이너를 기반으로 구성되어 있습니다. 최근에 Servlet API는 일부 Non-Blocking I/O에 대한 옵션을 가지고 있지만, 이것은 기존 프레임워크 에코시스템(Spring MVC, Thymeleaf 등)에 추가하기에는 만족스럽지 않구요.

그러나 Servlet 3.1과 Spring 3.2 기반이라면 Spring MVC에서도 비동기 요청 프로세싱(Asynchronous Request Processing)을 이용할 수 있습니다. 이를 통해 요청 하나당 서블릿 컨테이너 스레드가 할당되는 것과 같은 결합을 분리할 수 있습니다. Spring MVC에서도 DeferredResult나 Collable을 이용할 수 있고 이벤트를 기다리는 동안 다른 무언가를 할 수 있습니다.

- [Guide to DeferredResult in Spring](https://www.baeldung.com/spring-deferred-result)

<br><br>

Reactive Client for HTTP
-
그럼 이번에는 HTTP 통신을 위해 우리가 그동안 사용했던 RestTemplate을 살펴보고, 새롭게 버전5부터 지원하는 Reactive Client를 살펴보겠습니다.

<br>

### RestTemplate  
익숙한 이름입니다. RESTful Service 호출과 관련된 여러 메소드를 제공하여 REST 클라이언트를 쉽게 개발할 수 있도록 도와주는 템플릿입니다. Java 1.5 당시에 소개되어 지난 10년간 수 많은 애플리케이션에서 사용되어 왔습니다.

RestTemplate은 Synchronous하며 Blocking API로 구성되어 있습니다. 그리고 Streaming을 이용할 수 없습니다. 조금 더 정확히 말하면 양방향의 Streaming을 이용할 수 없습니다.

참고로 처음에는 24가지 메소드를 가지고 있었는데 현재는 40가지가 넘는 많은 메소드를 가지고 있습니다. 점점 많은 변수와 메소드가 추가되었고 Spring개발진들은 더 이상 공통적이지 않은 내용은 추가하지 않기로 했다고 합니다. 예를들어 HTTP DELETE를 Body와 함께 호출하는 메소드는 RestTemplate에 없습니다. 이런 동작을 원한다면 execute()나 exchange()등의 메소드를 이용하여 만들어야하고 이것이 더 공통적인 방식의 메소드를 활용하는 것이라고 합니다.

<br>

### 10년동안 무슨 일이 일어났나?  

그렇다면 10년이 지나면서 우리에겐 어떤 일들이 일어났을까요?

새로운 구성요소를 가진 Java8이 발표되었습니다. Java5의 어노테이션처럼 람다와 스트림 등을 이용해 함수형 프로그래밍을 적극적으로 지원하게 되었습니다. 이것은 10년전과 분명 다른 맥락입니다.

무어의 법칙은 더 이상 유효하지 않게 되었습니다. 하드웨어의 지속적이고 빠른 성장은 언젠가부터 멈추었습니다. 더 이상 우리는 하드웨어의 발전에 기댄 성능 향상을 기대할 수 없게 되었습니다. 더욱이 같은 기간동안 애플리케이션은 더욱 거대해지고 복잡해졌습니다. 그리고 네트워크의 동작은 전혀 동기적(Synchronous)이지 않습니다. 비동기적(Asynchronous)으로 움직이는 네트워크 속에서 동기화된 동작은 더 많은 자원과 시간을 소모합니다.

<br>

### WebClient  
그래서 Spring개발진들은 5.0 버전에서 WebClient를 선보였습니다. Java8 이상에서 이용할 수 있으며, 함수형 스타일의 API를 지원합니다. Async, Non-Blocking I/O 기반입니다. RestTemplate과는 정반대의 접근방향을 가지고 있습니다. Reactive하고 선언적(Declaritive)입니다. Streaming을 활용합니다.

그럼 다음 글에서 데모를 통해 기존 RestTemplate과 WebClient간의 차이점을 살펴보겠습니다.

<br>

- - -
* 참고
    - [SpringOne Platform 2018 - Guide to 'Reactive' for Spring MVC Developers](https://content.pivotal.io/springone-platform-2018/guide-to-reactive-for-spring-mvc-developers)