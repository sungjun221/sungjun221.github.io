---
layout: post
title:  "Ionic2 시작하기"
date:   2017-02-20 12:56:00 +0900
categories: ionic
---

이번엔 ionic2를 설치해보고 실제 화면을 띄워보겠습니다.
ionic2를 이용하기 위해선 npm을 이용해 설치를 진행해야 합니다.

```
npm install -g ionic cordova
```

이제 ionic2 프로젝트를 만들어 봅니다.
아래의 명령어로 간단하게 프로젝트를 생성할 수 있습니다.

```
ionic start myProj --v2
```

그럼 'myProj'란 폴더가 생성되었습니다.
이동해서 화면을 구동해보겠습니다.

```
cd myProj
ionic serve
```

'ionic serve' 명령어를 통해 로컬환경에서 웹서버를 띄워
화면을 직접 확인할 수 있습니다.
이 간단한 과정을 통해 벌써 그럴듯한 화면이 떴습니다. (짝짝짝!)


Ionic2 프로젝트 구조
-

생성된 프로젝트의 구조가 어떤지 살펴보겠습니다.

| 폴더명				| 내용			|
| :--------------------: | :-----------: |
| ./src | 개발자가 app을 위해 작성하는 code가 있는 폴더.         |
| ./resources	   		| 각 모바일 플랫폼별 icon, splash image가 있는 폴더.         |
| ./www		    		| build를 위해 생성하는 code가 있는 폴더. 실제 app code는 /app안에 위치함.|
| ./hooks	    		| Cordova 빌드 프로세스 중 실행되는 스크립트가 포함된 폴더. app package 빌드 프로세스를 커스터마이징을 하고 싶을 때 유용. |
| ./node_modules   		| npm으로 설치한 Node 모듈이 있는 폴더. |
| ./plugins		   		| Cordova plugin이 있는 폴더. Native와의 연동을 위해 Cordova Plugin을 만들거나 수정하게 되면 보게 될 폴더. |
| ./platforms   		| 각 플랫폼 별(IOS, Android 등) 배포용 코드가 있는 폴더. XCode나 Android Studio로 import 하여 배포할 대상이 되는 코드. |
| ./package.json	 	| npm 의존성 관리 파일.         |
| ./tsconfig.json	 	| TypeScript 컴파일러를 위한 설정 파일.         |
| ./config.xml	 		| Cordova에서 app package를 생성할 때 사용하는 설정 정보 파일.           |

봐도 아직은 감이 잘 안옵니다. 그럼 다음 장에서 화면을 수정하며 실습 해보겠습니다.
- - -
* 참고
    - [http://ionicframework.com/docs/v2/intro/installation/](http://ionicframework.com/docs/v2/intro/installation/)
    - [http://gonehybrid.com/build-your-first-mobile-app-with-ionic-2-angular-2-part-5/](http://gonehybrid.com/build-your-first-mobile-app-with-ionic-2-angular-2-part-5/)

