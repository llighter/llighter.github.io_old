---
layout: post
title:  "Template literals 사용하기"
date:   2017-04-12 11:00:00 +0900
categories: participate mnd
---


[Angular][1]를 공부하던 중 [Template literal][2]에 대한 내용이 나와 정리하고자 한다.
[Angular][1]는 ES2015와 Typescript 을 지원하기 때문에 ES2015의 기능들을 사용할 수 있는데 실제 사용되는 코드는 아래와 같다.

{% highlight typescript %}
template: `
  <h1>{{title}}</h1>
  <h2>{{hero.name}} details!</h2>
  <div><label>id: </label>{{hero.id}}</div>
  <div><label>name: </label>{{hero.name}}</div>
  `
{% endhighlight typescript %}

Angular는 컴포넌트들로 이루어져 있고 컴포넌트에 대응되는 template(html)이 있다. 이 template은 내용이 많아져 크기가 큰 경우에는 따로 파일을 분리하지만 위와 같이 간단한 내용은 컴포넌트 파일 내부에 넣을 수 있다. 기존과 같은 경우 `'' 나 ""`을 사용하여 한 라인 씩 문자열을 쓰고 `+` 연산을 통해 붙여줬지만 `ES2015`에서는 backtick notation(\``)을 사용하여 여러줄을 가독성 있게 처리할 수 있다. 4월 5일에 작성하였던 [Volunteer in Mozilla Korea Community 번역으로 시작하기][3] 포스트를 통해 알게된 [MDN][4]에서는 웹기술에 대한 다양한 내용을 제공하는데 그 중 [Template literal][2]에 대한 내용을 간단히 설명하려 한다.

[Template literal][2]은 `embedded expression`들을 허용하는 `string literal`이다. 위의 angular 예제에서는 `multi-line string`과 `string interpolation`을 둘 다 사용했는데 각각 분리하여 살펴 보겠다.

1) Multi-line strings

**기존**

{% highlight typescript %}
template: `
  console.log("string text line 1\n"+
                "string text line 2");
    // string text line 1
    // string text line 2
  `
{% endhighlight typescript %}

**ES2015**

{% highlight typescript %}
template: `
  console.log(`string text line 1
                string text line 2`);
    // string text line 1
    // string text line 2
  `
{% endhighlight typescript %}

앞서 설명했던 것과 같이 여러줄로 내용을 입력할 경우에 backtick notation(\``)을 통해 간편하게 표현할 수 있다.

2) Expression interpolation

**기존**

{% highlight typescript %}
var a = 5;
var b = 10;
console.log("Fifteen is " + (a + b) + " and\nnot " + (2 * a + b) + ".");
// "Fifteen is 15 and
// not 20."
{% endhighlight typescript %}

**ES2015**

{% highlight typescript %}
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and\nnot ${2 * a + b}.`);
// "Fifteen is 15 and
// not 20."
{% endhighlight typescript %}

위의 방식은 ES2015에서 지원하는 방식이고 Angular에서는 처음 예제와 같이 {{}} 을 사용한다. 



[1]: https://angular.io/
[2]: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals
[3]: https://llighter.github.io/participate/mnd/2017/04/05/volunteer_mdn.html
[4]: https://developer.mozilla.org/ko/