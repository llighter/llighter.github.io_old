---
layout: post
title:  "javascript: Defining functions"
date:   2017-04-04 14:30:00 +0900
categories: javascript jQuery
---

# jQuery(javascript)에서는 함수를 사용하는 방법이 크게 두가지가 있다.
아래의 내용은 [MDN-Defining functions][Defining functions]의 내용을 참고한 것이다.

# The function declaration(function statement)
{% highlight javascript %}
function name([param[, param[, ... param]]]) {
   statements
}
{% endhighlight javascript %}
**name**
- The function name.

**param**
- The name of an argument to be passed to the function. A function can have up to 255 arguments.

**statements**
- The statements comprising the body of the function.

# Function expression(function expression)
{% highlight Javascript %}
function [name]([param[, param[, ... param]]]) {
   statements
}
{% endhighlight Javascript %}
**name**
- The function name. Can be omitted, in which case the function becomes known as an anonymous function.

**param**
- The name of an argument to be passed to the function. A function can have up to 255 arguments.

**statements**
- The statements comprising the body of the function.

anonymous function expression의 예 (name을 사용하지 않음):
{% highlight Javascript %}
var myFunction = function() {
    statements
}
{% endhighlight Javascript %}

named function expression의 예(name을 사용함):
{% highlight Javascript %}
var myFunction = function namedFunction(){
    statements
}
{% endhighlight Javascript %}

이름을 가지는 `function expression`을 사용하는 것의 장점은 코드의 실행중에 에러가 발생하는 경우 `stack trace`에 함수의 이름이 들어가서 에러의 원인을 찾기 쉽다는 장점도 있다.
위의 두 예제에서 볼 수 있듯이 둘다 function 키워드로 시작하지 않는 것을 볼수 있다. 이와 같이, function 키워드로 시작하지 않는 Statements를 `function expression`이라 한다.
만약 내가 사용하는 함수가 단 한번만 쓰인다면, `IIFE` (Immediately Invokable Function Expressions)으로 작성하는 것이 일반적이다.
{% highlight Javascript %}
(function() {
    statements
})();
{% endhighlight Javascript %}
`IIFT`는 `function expression` 으로 함수가 선언되자 마자 호출이 된다.

[Defining functions]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions#The_function_declaration_(function_statement)

