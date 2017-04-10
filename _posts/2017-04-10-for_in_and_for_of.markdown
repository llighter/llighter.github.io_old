---
layout: post
title:  "QuerySelectorAll에서 For loop 사용하기"
date:   2017-04-10 18:00:00 +0900
categories: javascript for-loop
---

[QuerySelectorAll][1]은 아래와 같이 selectors의 조건에 만족하는 모든 element들의 list를 반환한다.
이때 반환하는 list의 형식은 `Nodelist`이다. 이 형식을 기억해두자.
{% highlight javascript %}
elementList = document.querySelectorAll(selectors);
{% endhighlight javascript %}

우리의 목적은 이렇게 반환된 elementList의 내용을 반목문을 통해 확인하는 것이다. 여기서는 3가지 방법의 for문을 살펴볼 것이다.

우리가 다룰 elements 이다.
{% highlight html %}
<h1>Loop Test</h1>
<div class="my-tag">my tag01</div>
<div class="my-tag">my tag02</div>
<div class="my-tag">my tag03</div>
{% endhighlight html %}

1) for (statement 1; statement 2; statement 3)
{% highlight javascript %}
for(var i = 0; i < myTagList.length; i++) {
    // my-tag 태그들의 글자 색을 하얀색으로 정한다.
    myTagList[i].style.color = "white";
}
{% endhighlight javascript %}
위의 방법은 가장 일반적이고 정형화된 방식이다.

2) [for (variable in object)][2]
{% highlight javascript %}
for(property in myTagList) {
    myTagList[property].style.color = "white";
}
{% endhighlight javascript %}
위의 방식은 사용할 수 있는 방식일까?
일반적인 배열인 경우 사실 사용해도 된다. 하지만 우리가 다루는 것은 `Nodelist`이다.
[for ... in][2] 의 작동방식을 자세히 살펴보면 객체의 열거형 속성들을 반복하는 것을 알 수 있다.
즉, 우리가 원하는 배열의 요소 이외에 기본적으로 내장되어 있는 요소들도 반복문에 포함이 된다는 것이다.

3) [for (variable of iterable)][3]<br>
이 방법을 피할 수 있는 방법은 [for ... of][3]를 사용하면 된다. 사실 [for ... in][2]는 객체의 내용을 다루는것이기 보다 객체가 가지고 있는 property(ex. lenth)를 다루는 방법에 적절하다.
{% highlight javascript %}
for(element of myTagList) {
    element.style.color="white";
}
{% endhighlight javascript %}
[for ... of][3]는 반복할수 있는 객체들(우리는 myTagList)에 대해 그 안에 있는 객체(myTagList[1], ..)를 다룬다. 따라서 myTagList의 property에 상관 없이 우리가 원하는 객체에 접근할 수 있다.




[1]: https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll
[2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
[3]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in
