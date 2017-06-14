---
layout: post
title:  "Ionic: firebase 조회하여 TOP5 목록 출력하기"
date:   2017-06-14 15:00:00 +0900
categories: angular ionic
---

나는 현재 Angular 기반 [Ionic framework][2]를 [firebase][3]와 연동하여 기본적인 어플리케이션([ionic-studycard][1])을 개발하고 있다.

![CARD-SYSTEM]({{ site.url }}/assets/Leitner_system.png)

[라이트너 학습법][4](이하 학습카드 시스템)을 이용한 학습카드 시스템을 만드는 것인데 단순히 학습카드 시스템을 사용하는 것 이외에 추가로 발생하는 메타데이터를 활용하기로 했다. 그중 하나가 `실패 이력 조회`이다. 간단히 말하면 학습카드 시스템은 스테이지가 5단계로 나뉘는데 카드의 정보를 외우는데 성공했으면 다음 스테이지로 넘어가는 방식이다. 반대로 실패했다면 다시 첫번째 스테이지로 넘어와서 다시 **반복**을 하는 구조이다. 열심히 공부하다보면 내가 뭘 많이 틀리고 헷갈려 하는지 대략적으로는 알 수 있지만 정확한 정보를 가시적으로 제공하기 위해 `TOP 5` 서비스를 제공하기로 했다.
원리는 간단하다 각 카드를 사용하면서 결과를 `failCount`라는 변수에 누적하는 것이다. 그래서 카드를 대상으로 `쿼리 조건`을 넣어 결과를 출력하는 방식으로 진행을 했다.

카드의 데이터 구조이다.
{% highlight html %}
export class CardDTO {
    question: string;
    answer: string;
    source: string;
    failCount: number;
    stage: number;
}
{% endhighlight html %}

개발 환경이 Angular 기반의 Ionic이기 때문에 Firebase를 직접 사용하지 않고 [Angularfire2][5] 라는 모듈을 설치해서 진행을 하였다. Firebase를 이해하고 있다면 큰 어려움이 없을 것이라 생각하고 간단히 설명하려고 한다.

#### **출력화면은 다음과 같다.**
![TOP5]({{ site.url }}/assets/top5.gif)

> `select`에서 `카테고리`를 선택하면 그 카테고리에 있는 데이터를 조회하여 `TOP 5`를 보여주는 것이다.

내부적으로 `ion-select`태그는 [ngModel][7]로 `selectedCategory`가 양방향 바인딩되어 있고 값이 변경될 경우 [ngModelChange][8] 를 통해 `updateStageCount()`가 실행된다.
{% highlight html %}
<ion-select [(ngModel)]="selectedCategory" (ngModelChange)="updateStageCount()">
  <ion-option *ngFor="let category of categories | async">
    {{ category.categoryName }}
  </ion-option>
</ion-select>
{% endhighlight html %}

`updateStageCount()`에서는 내부적으로 `getTopFailCount()`가 실행되는데 내용은 아래와 같다.
{% highlight typescript %}
getTopFailCount(): void {
  this.top5Observable = this.db.list(`${this.uid}/${this.selectedCategory.trim()}`, {
    query: {
      orderByChild: 'failCount',
      limitToLast: 5
    }
  }).map( (list) => { return list.reverse() } );
}
{% endhighlight typescript %}

기본적으로 Firebase는 Observable을 반환하기 때문에 `top5Observable`라는 `Observable<SOME-DTO[]>` 타입 변수에 결과를 할당하고 있다.
안의 쿼리를 보면 `orderByChild`를 통해 `failCount`로 오름차순으로 정렬하고 카운트가 높은 마지막 5개를 가져오는 내용이다.

네이버나 다음의 실시간 검색어에서도 그렇지만 당연하게도 높은 카운트 값을 가지는 내용이 위로 올라가야 한다. **그러나** Firebase가 지원하는 쿼리에는 내림차순으로 결과를 받는 방법은 없다.

이 문제를 해결하는 방법을 두가지 떠올렸다.

1. [Custom Pipe][9]를 활용하기

리스트 형태로 값을 가지고 있기 때문에 [Custom Pipe][9]을 활용하면 리스트 목록을 내림차순으로 정리하여 출력할 수 있다.
관련 코드는 구글링을 통해 스택오버플로우에서 [Invert Angular 2 *ngFor][6]에 잘 정리가 되어있다. 나는 곧장 이 코드를 활용하여 내 어플리케이션에 적용을 했지만 결과는 실패였다.

이유는 앞서 말했듯이 Firebase는 조회 결과를 [Observable][10]로 반환하기 때문에 위의 코드 처럼 `| asnyc`파이프를 붙여야 하고 이 파이프는 결과를 비동기적으로 받아와서 출력하겠다는 것이기 때문에 리스트의 순서를 조작하는 파이프와 같이 쓰일 수가 없었다.

2. [Map Operator(Map 연산자)][11]을 활용하기

[Map 연산자][11]는 Observable 소스가 방출하는 각 항목에 대해 내가 구현한 함수를 적용하고 그 결과를 Observable에 반환한다.
동작 방식은 다음과 같다.
![MAP-OPERATOR]({{ site.url }}/assets/map-operator.PNG)

{% highlight typescript %}
.map( (list) => { return list.reverse() }
{% endhighlight typescript %}
따라서 위에서 처럼 받아온 소스를 list 인자로 받아서 리스트를 반전시키는 [reverse()][12] 함수를 호출한 결과를 반환할 수 있다.

결과적으로는 Observable이라는 어려움을 마추졌지만 [Observable의 연산자][11]를 활용해서 문제를 해결 할 수 있었다.




[1]: https://github.com/llighter/ionic-studycard
[2]: http://ionicframework.com/
[3]: https://firebase.google.com/
[4]: https://en.wikipedia.org/wiki/Leitner_system
[5]: https://github.com/angular/angularfire2
[6]: https://stackoverflow.com/questions/35703258/invert-angular-2-ngfor
[7]: https://angular.io/api/forms/NgModel
[8]: https://angular.io/guide/template-syntax
[9]: https://angular.io/guide/pipes#custom-pipes
[10]: http://reactivex.io/documentation/observable.html
[11]: http://reactivex.io/documentation/ko/operators/map.html
[12]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse?v=control


