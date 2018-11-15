---
title: "Guide to 'Reactive' for Spring MVC Developers (1)"
tags:
  - Java
  - Spring
  - Reactive
last_modified_at: 2018-11-12T12:04:24-04:00
toc: true
---
(작성중인 글입니다. Ver 0.1.1)

최근 몇 년간 애플리케이션은 점점 커지고 복잡해졌습니다. 그래서 요청을 보내면 응답이 올 때까지 대기하고 있는 기존의 Synchronous, Blocking I/O에 기반한 시스템으로는 감당하기 어려운 상황도 나오고 있습니다.

그래서 최근에는 요청을 보내고 자기 할 일은 따로 하는 Asynchronous, Non-Blocking I/O에 기반한 시스템에 대한 관심이 커지고 있습니다. 이것이 바로 Reactive Programming입니다.

특히 많은 기업에서 사용하고 있는 Spring Framework의 5버전부터는 본격적으로 Reactive에 대한 지원을 시작하는데요. 이것이 바로 Spring WebFlux입니다. 이제 더 이상 Java개발자에게도 Reactive는 낯설지 않은 이야기가 되었습니다.

Pivotal에서 매년 개최하는 컨퍼런스인 SpringOne Platform의 올해 행사에서는 이런 분위기를 더욱 강하게 느낄 수 있었습니다. 그 중에서 기존 Spring 개발자를 대상으로 Reactive개발에 대해 가이드 하는 세션이 있었는데요. 부족하지만 이 세션의 내용을 공유해볼까 합니다. 혹시 잘못되거나 부족한 부분이 있으면 댓글로 남겨주시면 감사합니다.

시작에 앞서 Reactive가 생소한 분은 아래의 링크를 참고하시면 도움이 될 것 같습니다.

- [Reactive Programming](http://wiki.sys4u.co.kr/pages/viewpage.action?pageId=7766819)

<br><br>

Sync/Blocking? Async/Non-Blocking?
그런데 아까부터 Sync하고 Blocking하다고 하는데 두 용어가 무슨 차이가 있는 걸까요? 먼저 Blocking I/O란 내가 요청을 하면 응답이 올 때까지 대기하고 있는 것입니다. 내게 더 일할 수 능력이 있지만 응답이 올 때까지 그 자리에서 기다리고 있습니다. 

그럼 Synchronous하다는 것은 어떤 것일까요? 내 프로그램이 할 일을 line by line으로 쭉 기술한 것입니다. 그래서 Sync와 Blocking은 궁합이 좋습니다. 응답을 기다리고 있다가 그 다음 할 일을 순서대로 하면 되기 때문이죠.

Async와 Non-Blocking은 그 반대입니다. Non-Blocking I/O는 요청을 하고 난 뒤 응답이 올 때까지 기다리지 않습니다. 내 할 일 다 하다가 응답이 오면 그에 대한 처리를 합니다. Asynchronous하다는 것은 내가 한 요청에 대한 응답이 왔을 때 어떤 일을 할지를 기술하는 것입니다. JavaScript의 콜백함수가 그런 방식입니다. 그래서 마찬가지로 Async와 Non-Blocking이 궁합이 좋습니다. 일을 기다리면서 순서대로 처리하지 않으니, 내가 응답을 받으면 어떻게 행동하겠다를 기술하면 되는 것입니다.

<br><br>

어떻게 다른가?
-
기존에는 서블릿 컨테이너에서 요청이 하나 오면 하나의 스레드를 생성해서 처리했습니다. 동기화 된 기존의 시스템에서 대량의 요청을 처리하기 위한 방법이었습니다. 하지만 스레드 생성은 비용이 듭니다. 특히 스레드는 각각 메모리가 필요합니다. 많은 스레드는 많은 메모리를 필요로 합니다.

Reactive의 방식은 다릅니다. 요청을 하고 기다리지 않습니다. 자기 할 일을 합니다. 그래서 요청만큼의 스레드가 필요하지 않습니다. 요청을 전달하고 돌아온 스레드가 다시 요청을 처리할 수 있습니다. 10개의 요청을 할 때 예전에는 10개의 스레드가 필요했지만 이제 그보다 적은 6,7개의 스레드만으로 처리할 수 있습니다. 그리고 응답을 기다리지 않고 동작하기 때문에 자원을 더 밀도있게 사용합니다. 

<br><br>

Reactive로의 전면적인 전환은 현실적으로 어렵다
-
그럼 이렇게 장점이 많은 Reactive를 기존의 시스템에 적용하고 싶어집니다. 대부분의 Reactive에 대한 이야기들은 Full Stack의 Reactive를 이야기합니다. 물론 Reactive의 장점을 최대한 누릴 수 있는 가장 좋은 구성입니다.

하지만 현실적으로 Full Stack을 Reacitve로 전환하는 것은 어렵겠죠. 그리고 모든 시스템이 Reactive가 필요한 것도 아닙니다. 기존에 잘 돌아가는 시스템을 굳이 Reactive로 전환해야 할 필요는 없겠지요. 그렇다면 리스크를 줄이면서도 리액티를 적용해보는 방법은 없을까요?

<br><br>

Spring MVC에서도 비동기 요청 프로세싱을 이용할 수 있다
-

<br>

- - -
* 참고
    - [SpringOne Platform 2018 - Guide to 'Reactive' for Spring MVC Developers](https://content.pivotal.io/springone-platform-2018/guide-to-reactive-for-spring-mvc-developers)