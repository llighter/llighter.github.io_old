---
layout: post
title:  "QUERY PROCESSING ORDER 퀴리 처리 순서"
date:   2017-03-27 12:30:00 +0900
categories: database sql
---

[W3School SQL Tutorial]: https://www.w3schools.com/sql/default.asp
[ORACLE Live SQL]: https://livesql.oracle.com/apex/livesql/file/index.html
[Oracle SQL and PL/SQL Optimization for Developers]: http://oracle.readthedocs.io/en/latest/index.html

- 이 포스트에서 작성된 글은 Oracle SQL & PL/SQL Optimization for Developers 의 문서의 내용을 참고한 것입니다.[Oracle SQL & PL/SQL Optimization for Developers][Oracle SQL & PL/SQL Optimization for Developers]

쿼리의 실행 계획(execution plan)을 이야기하기전에 이해해야할 것은 어떻게 오라클이 논리적으로 쿼리를 처리 하는가이다. 아래의 쿼리를 통해 쿼리가 어떻게 논리적으로 동작하는지 살펴보겠다.

{% highlight SQL %}
SELECT
    f.product AS beer
  , p.product AS crisps
FROM
  fridge f
CROSS JOIN
  pantry p
WHERE
  f.product         = 'Beer'
  AND f.temperature < 5
  AND f.size        = '50 fl oz'
  AND p.product     = 'Crisps'
  AND p.style       = 'Cream Cheese'
  AND p.size        = '250g'
ORDER BY
    crisps
  , beer
;
{% endhighlight SQL %}

우리가 쿼리를 작성할 때 흔히 우리가 위에서 아래로 쓴 순서대로 실행될것이라 기대하지만 오라클(그 외 RDBMS 포함)은 쿼리를 단순히 위에서 아래로 실행시키지 않는다. 오라클은 우리가 작성한 SQL을 기계가 이해하고 실행할 수 있는 형태로 변환한다. 본질적으로 데이터 로봇은 우리가 시킨 그대로의 명령을 수행한다. 이해를 돕기위해 이 로봇을 집안일을 돕기위해 구매했다고 가정해보자. 이 로봇이 하는 가장 중요한 일은 당신의 목마름과 배고픔을 처리하는 것이다.
자, 이제 맥주와 포테이토칩을 가져오게 하려면 이 로봇에게 뭐라고 명령해야할까?

아마 가장 먼저 할 일은 냉장고로 가는 것이다. 온도가 5도 이하의 차가운 맥주 한병을 찾고 식료품 쪽으로 가서 250g 짜리 크림치즈 포테이토칩을 찾아야한다. 목적을 완료했다면 이제 나에게 돌아와 가져온 물건을 내려놓고 내가 요청한 순서대로 나열한다. 정리하자면 가장 먼저 냉장고와 식료품이 있는 장소로 가라고 명령한다.(아마도 부엌: `FROM`), 그리고 내가 말한 조건에 맞는 모든 것들을 찾는다.(`WHERE`) 마지막으로 돌아와서 찾은 물품들을(`SELECT`) 내가 말한 순서대로 정렬(`ORDER BY`)하여 보여준다.

이것이 오라클이 하는 일이다. 오라클이 논리적으로 처리하는 순서는 다음과 같다.
`FROM -> CONNECT BY -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY`
물론 위의 순서가 모든 구절을 포함하는 것은 아니다.

이 사진은 옵션을 포함한 오라클의 쿼리 프로세싱 순서이다.
![QUERY-PROCESS-ORDER]({{ site.url }}/assets/query-proc-order.png)

이러한 처리 순서때문에 예상치 못한 에러가 발생하기도 한다.
{% highlight SQL %}
SELECT
    product          AS item
  , MIN(temperature) AS min_temperature
  , COUNT(*)         AS num_products
FROM
  fridge
GROUP BY
  item
;
{% endhighlight SQL %}

위의 쿼리에서 `item`은 `select` 절에 가야만 알수 있는 `alias`이기 때문에 쿼리 순서상 `group by` 절에서는 알지못하는 상태이다 따라서 `ORA-00904: invalid identifier` 와 같은 에러 메시지를 출력할 것이다.

[Oracle SQL & PL/SQL Optimization for Developers]: http://oracle.readthedocs.io/en/latest/sql/basics/query-processing-order.html
