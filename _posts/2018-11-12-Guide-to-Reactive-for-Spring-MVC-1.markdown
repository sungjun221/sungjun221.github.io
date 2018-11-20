---
title: "Guide to 'Reactive' for Spring MVC Developers (1)"
categories: 
  - Java
  - Spring
  - Reactive
tags:
  - Java
  - Spring
  - Reactive
last_modified_at: 2018-11-12T12:04:24-04:00
toc: true
---
Reactive 지원을 시작한 Spring
-

![SpringOne Platform 2018](https://user-images.githubusercontent.com/4060030/48657448-3fa9d200-ea74-11e8-86f5-250669140534.png "SpringOne Platform 2018")
최근 몇 년간 애플리케이션은 점점 커지고 복잡해졌습니다. 그래서 요청을 보내면 응답이 올 때까지 대기하고 있는 기존의 Synchronous, Blocking I/O에 기반한 시스템으로는 감당하기 어려운 상황도 나오고 있습니다.

그래서 최근에는 요청을 보내고 자기 할 일은 따로 하는 Asynchronous, Non-Blocking I/O에 기반한 시스템에 대한 관심이 커지고 있습니다. 이것이 바로 Reactive Programming입니다.

특히 많은 기업에서 사용하고 있는 Spring Framework의 5버전부터는 본격적으로 Reactive에 대한 지원을 시작하는데요. 이제 더 이상 Java개발자에게도 Reactive는 낯설지 않은 이야기가 되었습니다.

Spring 개발사인 Pivotal에서 매년 개최하는 컨퍼런스인 SpringOne Platform의 올해 행사에서는 이런 분위기를 더욱 강하게 느낄 수 있었습니다. 그 중에서 기존 Spring 개발자를 대상으로 Reactive개발에 대해 가이드 하는 세션이 있었는데요. 부족하지만 이 세션의 내용을 공유해볼까 합니다. 혹시 잘못되거나 부족한 부분이 있으면 댓글로 남겨주시면 감사합니다.

Reactive가 생소한 분은 아래의 링크를 참고하시면 도움이 될 것 같습니다.

- [Reactive Programming](http://wiki.sys4u.co.kr/pages/viewpage.action?pageId=7766819)

<br><br>

Sync/Blocking? Async/Non-Blocking?
-

시작에 앞서 용어 정리를 하고 가면 좋을 것 같습니다. Sync, Blocking의 차이는 무엇일까요? 먼저 Blocking I/O란 내가 요청을 하면 응답이 올 때까지 대기하고 있는 것입니다. 내게 더 일할 수 능력이 있지만 응답이 올 때까지 그 자리에서 기다리고 있습니다. 

그럼 Synchronous하다는 것은 어떤 것일까요? 내 프로그램이 할 일을 line by line으로 쭉 기술한 것입니다. 이 일을 하고 나면 어떤 일을 해야할지 순서대로 알려주는 것입니다. 그래서 Sync와 Blocking은 궁합이 좋습니다. 응답을 기다리고 있다가 그 다음 할 일을 순서대로 하면 되기 때문이죠.

Async와 Non-Blocking은 그 반대입니다. Non-Blocking I/O는 요청을 하고 난 뒤 응답이 올 때까지 기다리지 않습니다. 내 할 일 다 하다가 응답이 오면 그에 대한 처리를 합니다. Asynchronous하다는 것은 내가 한 요청에 대한 응답이 왔을 때 어떤 일을 할지를 기술하는 것입니다. JavaScript의 콜백함수가 그런 방식입니다. 그래서 마찬가지로 Async와 Non-Blocking이 궁합이 좋습니다. 일을 기다리면서 순서대로 처리하지 않으니, 내가 응답을 받으면 어떻게 행동하겠다를 기술하면 되는 것입니다.

<br><br>

어떻게 다른가?
-
기존 서블릿 컨테이너에선 요청 하나마다 하나의 스레드를 할당해서 처리했습니다. Blocking 기반에선 응답이 오기까지 기다리고 있어야 하기 때문에 다른 요청이 들어오면 이를 처리할 수 없습니다. 그 문제를 스레드마다 요청을 담아 처리하는 것으로 해결했습니다. 이를 통해 다수의 요청에도 응답 할 수 있게 되었습니다.

Non-Blocking 기반은 다른 접근방식입니다. 애초에 응답을 기다리지 않고 돌아오기 때문에 굳이 많은 스레드가 필요하지 않습니다. 요청만 하고 돌아온 스레드에 다른 요청을 또 전달하면 됩니다.

그렇다면 많은 스레드는 어떤 점이 문제일까요? 우선 스레드는 프로세스의 공용 자원에 동시에 접근할 수 있습니다. 그래서 늘 동기화 문제가 따라다니기 마련입니다. 이런 부분은 병목구간이 되기 쉽습니다. 

그리고 각각의 스레드는 메모리를 필요로 합니다. 더 많은 스레드를 관리하기 위해 스레드풀에 배치해둔 많은 양의 스레드는 유휴스레드로 낭비될 수 있습니다. 반대로 스레드풀에 남는 스레드가 없으면 지연현상이 발생하게 됩니다.

무엇보다 근본적으로 네트워크는 동기적으로 움직이지 않습니다. 비동기로 움직이는 네트워크 위에서 동기화 된 시스템은 필연적으로 자원의 낭비를 초래하게 됩니다.

<br><br>

Reactive로의 전면적인 전환은 현실적으로 어렵다
-
![Spring WebFlux](https://user-images.githubusercontent.com/4060030/48329064-53c38d00-e68a-11e8-823f-56bce05a06c5.PNG "Spring WebFlux")  
그럼 이렇게 장점이 많은 Reactive를 기존의 시스템에 적용하고 싶어집니다. 대부분의 Reactive에 대한 이야기들은 Full Stack의 Reactive를 이야기합니다. 물론 Reactive의 장점을 최대한 누릴 수 있는 가장 좋은 구성입니다.

하지만 현실적으로 Full Stack을 Reacitve로 전환하는 것은 어렵겠죠. 그리고 모든 시스템이 Reactive가 필요한 것도 아닙니다. 기존에 잘 돌아가는 시스템을 굳이 Reactive로 전환해야 할 필요는 없겠지요. 그렇다면 리스크를 줄이면서도 리액티를 적용해보는 방법은 없을까요?

<br><br>

Spring MVC에서도 비동기 요청 프로세싱을 이용할 수 있다
-
![Spring MVC](https://user-images.githubusercontent.com/4060030/48329150-af8e1600-e68a-11e8-879a-33cdf69edb35.PNG "Spring MVC")
Servlet 3.0, Spring 3.2 이상의 환경이라면 Spring MVC에서도 DeferredResult 등을 활용하여 비동기 요청 프로세싱(Asynchronous Request Processing)을 이용할 수 있습니다. 

그림과 같이 서블릿 컨테이너 영역은 여전히 Blocking하게 동작하지만, 애플리케이션 영역을 비동기로 동작하게 할 수 있는 것입니다. 물론 Full Stack으로 구성한 것처럼 완전한 Reactive는 아니지만 기존 시스템의 기반을 유지하면서 Reactive의 장점을 일부 취할 수 있습니다. 아래의 글을 참고해보시기 바랍니다.

- [Guide to DeferredResult in Spring](https://www.baeldung.com/spring-deferred-result)

<br><br>

Reactive Client for HTTP in Spring 5
-
이번엔 Spring 5버전부터 등장한 HTTP 통신을 위한 Reactive 클라이언트를 살펴보겠습니다. 겸사겸사 그동안 활약했던 RestTemplate도 살펴보구요.

### RestTemplate
Spring 세계에서 오랫동안 우리를 지원해준 든든한 이름입니다. 손쉽게 REST클라이언트 개발을 할 수 있도록 도와주는 템플릿입니다. Java 1.5 당시에 소개되어 지난 10년간 수많은 애플리케이션에서 활약해왔습니다. RestTemplate은 Sync, Blocking 기반입니다. 

<br>

### 지난 10년간의 변화
그렇다면 이렇게 잘 사용해온 RestTemplate을 놔두고 Spring개발진들은 왜 새로운 HTTP 클라이언트를 개발하게 되었을까요? 이를 위해선 지난 10년간의 변화를 살펴봐야합니다.

먼저, 새로운 구성요소를 가진 Java 8이 발표되었습니다. 람다와 스트림이 등장했고 함수형 프로그래밍을 적극적으로 지원하게 되었습니다.

하드웨어쪽은 어떨까요? 더 이상 무어의 법칙은 유효하지 않게 되었습니다. 하드웨어의 지속적이고 빠른 성장은 언젠가부터 정체되기 시작했습니다. 더 이상 하드웨어의 발전에 기댄 성능향상을 기대할 수 없게 되었습니다. 더욱이 같은 기간동안 애플리케이션은 더욱 거대하고 복잡해졌습니다.

<br>

### 새롭게 등장한 WebClient
그래서 Spring 개발진들은 5버전부터 WebClient를 선보였습니다. Async, Non-Blocking I/O를 기반으로 합니다. RestTemplate과는 정반대의 접근방향입니다. Reactive하고 선언적(Declaritive)이며 Streaming을 활용합니다. 그럼 다음 2편에서 샘플예제를 실행해보며 WebClient와 RestTemplate간의 차이점을 살펴보겠습니다.

<br>

- - -
* 참고
    - [SpringOne Platform 2018 - Guide to 'Reactive' for Spring MVC Developers](https://content.pivotal.io/springone-platform-2018/guide-to-reactive-for-spring-mvc-developers)