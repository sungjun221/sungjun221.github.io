---
layout: post
title:  "Ionic2 + Firebase3로 빠르게 나만의 앱 만들기"
date:   2017-02-19 12:56:00 +0900
categories: ionic
---

개발자라면 나만의 앱을 하나씩 가지고 싶은 마음이 있을 겁니다.
요새는 잘 나온 프레임워크와 인프라 서비스가 많아 개인도 전보다 쉽고 빠르게 앱을 개발할 수 있는 환경이 됐습니다.

특히 웹개발자라면 JavaScrpit를 이용해서 앱을 개발하는 방법도 있죠.
Ionic2를 이용해 JavaScript로 개발하고, Google의 Firebase를 이용해 인프라 구축에 드는 비용을 최소화 할 수 있습니다.


Ionic2
-
Ionic에 대해 공식사이트에서는 아래와 같이 설명하고 있습니다.

> Ionic Framework is an open source SDK that enables developers to build performant, high-quality mobile apps using familiar web technologies (HTML, CSS, and JavaScript).

즉 웹 기술을 이용해서 하이브리드웹앱을 개발할 수 있게 해주는 오픈소스 프레임워크입니다.
한 번의 개발로 IOS/Android에 동시 배포할 수 있어 공수를 줄일 수 있습니다.
기본으로 제공되는 템플릿과 컴포넌트가 훌륭해서 쉽고 빠르게 이쁜 앱을 만들 수 있는 점도 장점이죠.
  
  
Ionic2로 만든 앱은 느리지 않나요?
-
Ionic2에 관심을 갖는 분들이 가장 많이 떠올리는 질문일 겁니다. 제 경험으로는 높은 퍼포먼스를 요구하는 B2C 서비스앱을 만드는 것이 아니라면 Ionic2도 좋은 선택입니다.

Ionic2로 빠르게 앱을 하나 만들어보고 반응이 좋고 서비스가 커지면 Native로 변경하는 것도 하나의 방법이란 생각이 듭니다. 제가 만든 앱의 url을 올려놓으니 받아보시고 ionic2로 만든 앱이 어떤 느낌인지 살펴보세요.
#### 오사카놀이 : 오사카여행 가이드 앱
- [IOS](https://itunes.apple.com/kr/app/osakanol-i-osakanori-osaka/id1163599168?mt=8)
- [Android](https://play.google.com/store/apps/details?id=com.sjkim.osakanoriapp&hl=ko)

  
Ionic2를 쓰기위해 필요한 기반지식은?
-
Ionic2는 아래의 기술을 베이스로 하고 있습니다.
- Angular2
- ECMAScript6
- TypeScript

Angular2는 백엔드에 비해서 구조화 된 개발이 어려웠던 뷰영역을 컴포넌트 중심의 개발이 가능하도록 만든 프레임워크입니다. ECMAScript는 JavaScript의 기반이 되는 Spec입니다. ECMAScript6은 2015년 6월에 발표된 명세이고 기존의 JavaScript 문법과는 차별화 되는 요소가 추가되었습니다.

TypeScript는 2012년에 발표된 MS에서 만든 언어로 기존의 JavaScript를 확장하였습니다. 이름 그대로 Type을 명확히 표기하고 객체지향적인 개발이 가능하도록 하였습니다.
  
  
위 내용을 몰라도 개발할 수 있나요?
-
..없습니다. 그러나 웹개발을 하고 계시다면 어차피 보는 것이 좋으므로 이번 기회에 공부를 해보시는 것이 좋을 것 같습니다. TypeScript는 JavaScript의 확장된 언어라 JavaScript를 쓸 줄 안다면 약간의 공부만으로 금방 쓰실 수 있는 정도입니다.

Angular2는..그냥 공부해야 합니다.


ionic1 VS ionic2
-
ionic2는 아직 베타버전입니다. 그냥 ionic을 검색하시면 ionic1 사이트로 들어갈 겁니다.
두 버전의 차이는 ionic2는 Angular2, ionic1은 Angular1 기반이라는 것입니다.

Angular2와 1은 문법과 구조가 완전히 바뀐 다른 프레임워크입니다.
Angular개발진은 제대로 된 구조를 위해 1을 더 이상 지원하지 않고, 호환이 되지 않는 2를 개발한다고 하였습니다.

때문에 폐기된 Angular1 기반이 아닌 2기반의 ionic2를 사용하는 것이 좋습니다. 퍼포먼스도 2가 1에 비해서 많이 개선되었습니다.



Firebase란?
-
위키피디아에선 Firebase를 아래와 같이 소개하고 있습니다.
> Firebase is a mobile and web application platform with tools and infrastructure designed to help developers build high-quality apps.

Firebase는 모바일 또는 웹어플리케이션을 만들 때 메시징, 인증, DB, 저장소 등의 운영/인프라영역 구축을 제공해주는 플랫폼입니다. 그래서 개발자는 서버구성이나 관리, 인증체계 구축 등을 신경쓰지 않고 Firebase에서 제공해주는 것을 그대로 사용하면 됩니다.

그럼 Ionic2+Firebase3를 이용한 앱개발 방법에 대해 다음 장에서 설명을 이어가겠습니다.
