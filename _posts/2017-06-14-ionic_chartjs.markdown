---
layout: post
title:  "Ionic에서 그래프(chart.js) 그리기"
date:   2017-06-14 12:00:00 +0900
categories: angular ionic
---

나는 현재 Angular 기반 [Ionic framework][3]를 [firebase][4]와 연동하여 기본적인 어플리케이션([ionic-studycard][1])을 개발하고 있다.

![BASIC-USAGE]({{ site.url }}/assets/basic.gif)

위와 같이 학습카드 시스템을 사용하는데에는 문제가 없지만 각각의 스테이지(5단계로 나누어져있다.)을 얼마만큼 사용하고 있는지 확인을 하고 싶어졌다. 일단은 간단히 각각의 스테이지에 들어있는 카드 개수를 화면에 표시했지만 임시적인 조치이고 이 내용을 그래프로 그려서 한눈에 파악하는 것이 좋다고 판단했다.

![TEMP]({{ site.url }}/assets/temp.png)

그래프를 그리기 위해 조사해본 결과 대부분 사용법이 비슷하고 지원하는 그래프 종류 또한 대동소이 하기 때문에 사용하기 편하고 예제가 잘 작성이 되어 있는(대부분이 그렇지만) [chart.js][2] 라이브러리를 사용하기로 헀다.

먼저 완성한 화면을 보면 아래와 같다.

![STAT]({{ site.url }}/assets/stat.gif)

보이는 바와 같이 `stat` 탭으로 이동하면 내가 보유하고 있는 카테고리 목록을 선택할 수 있다. 목록을 선택하면 동적으로 그래프를 업데이트 해주는 것이 주된 기능이다.

사용량을 표시하는 것이 목적이기 때문에 [Stacked Bar Chart][5]를 사용하였다.
그럼 이제 실제 구현 방법을 간단히 보여주겠다. 아래 작성하는 예제는 내 어플리케이션에 맞게 사용한 코드이고 기본적인 프로젝트를 생성하고 그래프를 넣는 것을 보고 싶다면 [joshmorony][6]라는 분이 운영하는 사이트에 자세한 설명이 되어 있으니 참고하도록 하자.

1. 먼저, 기존 프로젝트에 [Chart.js][2]를 설치해야한다. 

{% highlight bash %}
npm install chart.js --save
{% endhighlight bash %}

2. 템플릿(html)을 설정한다.
데이터의 작성과 업데이트는 ts파일에서 하기 때문에 템플릿에서는 `<canvas>`만 선언해주면 된다.
자동으로 크기가 맞춰지지만 나는 높이 속성을 300으로 조금 늘려줬다.
{% highlight html %}
<ion-card>
  <ion-card-header>
    Bar Chart
  </ion-card-header>
  <ion-card-content>
    <canvas #barCanvas height="300"></canvas>
  </ion-card-content>
</ion-card>
{% endhighlight html %}

3. 이제 그래프를 생성한다.
그래프 이외에 다른 로직도 같이 있기 때문에 주요 부분만 가져와서 설명을 하겠다.

{% highlight typescript %}
import { Chart } from 'chart.js';

... 

ionViewDidLoad() {
  this.barChart = new Chart(this.barCanvas.nativeElement, {
    type: 'bar',
    data: {
      labels: ["1", "2", "3", "4", "5"],
      datasets: [{
        label: '# of card in use',
        data: this.stageCount,
        backgroundColor: 'rgba(255, 99, 132, 0.2)',
        borderColor: 'rgba(255,99,132,1)',
        borderWidth: 2
      },
      {
        label: '# of Remaining card',
        data: this.remainCount,
        backgroundColor: 'rgba(54, 162, 235, 0.2)',
        borderColor: 'rgba(54, 162, 235, 1)',
        borderWidth: 2
      }],
    },
    options: {
      scales: {
        xAxes: [{
            stacked: true
        }],
        yAxes: [{
          ticks: {
            beginAtZero:true
          },
          stacked: true,
        }],
      },
      responsive: true
    }
  });
}

...

this.barChart.update();
{% endhighlight typescript %}

위 코드는 3부분으로 나누어 볼 수 있는데 먼저 맨위에 [Chart.js][2]를 사용하기 위해 `import` 해준다.
다음으로 `ionViewDidLoad()`는 해당 페이지가 로드되면 실행되는 함수로써 그래프 객체를 생성하고 데이터와 설정값을 지정한다.
설정 자체가 기본적인 사항들만 사용했기 때문에 이해하는데에는 어려움이 없고 `data: this.stageCount` 같은 경우는 클래스 내 변수를 할당한 부분이다. 또한 `Stacked Bar Chart`를 사용하기 위해 옵션에서 `stacked`옵션을 `true`로 설정해 주었다.
마지막으로 `this.barChart.update();`에서는 상태가 변한 그래프를 다시 그리라는 update 함수를 호출한것이다. 이 함수는 처음 카테고리를 선택하면 실행되도록 되어 있다. 전체 코드는 [Github Repo][1]에서 확인할 수 있다.

이상으로 간단히 [Chart.js][2] 라이브러리를 활용하여 쉽고 간단하게 내 프로젝트에 연동을 시켰다. 사용법이 쉽고 간단하기 때문에 누구나 손쉽게 따라 할 수 있고 보기 좋은 결과를 얻을 수 있을 거라고 생각한다.

전체 코드나 궁금한 점이 있다면 [Github Repo][1]에 들어가서 확인할 수 있고 만약 들어갔다면 상단에 `스타` 버튼을 클릭해준다면 조금 더 보람이 있을 것 같다. :)

[1]: https://github.com/llighter/ionic-studycard
[2]: http://www.chartjs.org/
[3]: http://ionicframework.com/
[4]: https://firebase.google.com/
[5]: http://www.chartjs.org/docs/latest/charts/bar.html#stacked-bar-chart
[6]: https://www.joshmorony.com/adding-responsive-charts-graphs-to-ionic-2-applications/



