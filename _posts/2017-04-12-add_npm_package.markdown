---
layout: post
title:  "Package.json에 depencency 추가하기"
date:   2017-04-12 11:00:00 +0900
categories: participate mnd
---

Angular는 [NPM][1]을 통해 패키지를 관리한다. Angular 공부를 시작하면서 여러가지 툴들을 사용하게 되는데 이것도 npm은 외부 라이브러리를 사용하려면 필수적으로 알아야 하는 것 중 하나이다.

이번에는 프로젝트 중간에 외부 라이브러리를 추가할 때 어떻게 하는지 알아보려 한다.
현재 내가 작성하고 있는 프로그램은 angular에서 제공하는 [tutorial][2]이다. 이 튜토리얼의 6단계에는 http를 사용하여 서버에서 가져온 데이터를 어떻게 관리하는지(CRUD)를 보여주는데 이때 서버를 설정하기 위해 [InMemoryWebApiModule][3] 라이브러리가 필요하다. 나는 튜토리얼에서 제공해주는 파일을 사용한 것이 아니라 [angular-cli][4]를 사용하여 프로그램을 재 작성하고 있었기 때문에 이 라이브러리가 없었다. 그래서 npm을 통해 추가를 해야했다.

프로젝트 중간에 라이브러리를 추가할 때 사용목적에 따라 두가지 명령어를 사용할 수 있다.

**1) npm install angular-in-memory-web-api --save**

이 명령을 입력하면 아래와 같이 `angular-in-memory-web-api`가 추가 된다.

{% highlight json %}
"dependencies": {
  "@angular/common": "^4.0.0",
  "@angular/compiler": "^4.0.0",
  "@angular/core": "^4.0.0",
  "@angular/forms": "^4.0.0",
  "@angular/http": "^4.0.0",
  "@angular/platform-browser": "^4.0.0",
  "@angular/platform-browser-dynamic": "^4.0.0",
  "@angular/router": "^4.0.0",
  "angular-in-memory-web-api": "^0.3.1",
  "core-js": "^2.4.1",
  "rxjs": "^5.1.0",
  "zone.js": "^0.8.4"
},
{% endhighlight json %}


**2) npm install (library-name) --save-dev**

위의 명령어와 차이점은 `devDependencies`에 들어가서 실제 배포할 때는 제외되는 라이브러리를 설치할 떄 사용한다는 것이다. 즉, 개발 과정중에서만 필요한 경우 위의 명령어를 사용하면된다.

이상으로 기본적인 라이브러리 추가 방법에 대해 알아 보았다. 위의 포스트는 가장 기본적인 패키기 관리일 뿐이다. 더 자세한 정보를 알고 싶다면 [NPM][1]에서 확인하면 된다.(물론 영어)

[1]: https://docs.npmjs.com/cli/install
[2]: https://angular.io/docs/ts/latest/tutorial/
[3]: https://github.com/angular/in-memory-web-api
[4]: https://cli.angular.io/