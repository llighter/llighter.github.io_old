---
layout: post
title:  "no such file or directory - xx.sh^M"
date:   2017-05-04 16:00:00 +0900
categories: tutorial VSCode
---


나는 현재 맥북을 사용하고 있고 내 터미널의 인터페이스를 세팅하다가 문제이 부딪혔다.
현재 내 터미널 환경은 iterm2를 사용하고 있고 zsh을 사용하고 있다 쉘 인터페이스는 많이 사용하는 zsh + oh my zsh을 사용하는데 여기에는 사용자 커스텀 플러그인들을 적용하여 개인에 맞게 사용할 수 있다.
[DailyEngineering][1] 에서는 이 분이 사용하는 여러 플러그인들을 및 테마를 올려놓으셨는데 내가 적용하던 중 문제가 발생했다.

**문제가 된 상황은 이러하다**

![ERROR_PRINT]({{ site.url }}/assets/not-found-sh.png)
위의 그림은 zshrc 파일에서 내가 내려받은 플러그인을 적용하려고 설정한 후 내 터미널을 다시 실행시켰을 때 나타나는 에러 화면이다.
자세히 보면 알겠지만 끝의 `^M`이라는 문자가 눈에 띈다. 뭔가 이게 문제가 있는 것 같다.


![PLUGIN_FILE]({{ site.url }}/assets/plug-in.png)
문제가 되는 파일을 열러보니 위와 같이 끝에 `^M`이라는 문자가 포함되어 있었다.
일반적인 vim 설정으로 열어보면 끝의 문자가 보이지 않는다.
{% highlight sh %}
vim -b (file-name)
{% endhighlight sh %}
위와 같이 `-b`옵션을 줘서 바이너리 형태로 열어야 한다.

구글링을 해본 결과 맥, 리눅스 환경과 윈도우 환경에서의 개행문자(줄 바꿈) 처리 방식이 달라 생기는 문제라고 한다. [이곳][2]에서 해결방법을 안내해준대로 `^M`문자를 제거하고 실행시켜 보았다.
결론은 잘 동작하였다.

**참고)** 추가로 개행문자가 달라서 한 파일에 전체적으로 적용해야하는 경우에는 VIM을 활용하여 문제를 해결 할 수 있다.
VIM 명령어를 활용하여 정규표현식을 이용해서 끝에 붙어있는 `^M`을 전부 지울 수 있다.

{% highlight vim %}
:%s/^M//g
{% endhighlight vim %}
위의 명령어는 `^M`이라는 문자를 없애는 동작을 한다. 주의할 점은 [ctrl]+V, [ctrl]+M 을 타이핑해서 입력해주어야 한다는 것이다.





[1]: https://hyunseob.github.io/2017/02/05/my-command-line-interface/
[2]: http://tod2.tistory.com/28