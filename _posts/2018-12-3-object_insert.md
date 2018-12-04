---
layout: post
title: 블로그에 각종 개체 삽입 방법 (지킬 블로그 기준)
categories : [Web]
tags : [Blog, Web]
---

<span style = "line-height:50%"><br></span>

아래에 포스팅할 삽입 방법은 제 블로그(지킬 깃허브 블로그)에 적용 가능함을 밝힙니다. (다른 종류의 블로그에는 그에 맞는 삽입 방법을 쓰세요)

## [블로그에 유튜브 동영상 삽입하기]

매우 간단하다.

먼저 삽입할 유튜브 동영상의 주소를 복사한 다음,

<a href = "http://embedresponsively.com/ ">embedresponsively.com</a>에 들어가서 붙여넣기 하고 Embed 클릭. -> HTML 코드 생성.

이 코드를 그대로 복사해서 블로그 편집창에 붙여넣기 하면 끝. (저는 Typora 편집기를 쓰기 때문에 HTML 코드를 자동으로 인식해서 동영상을 보여줍니다. 다른 마크다운 편집기를 쓰시는 분들은 HTML을 인식하게끔 처리해 주세요)

<div>
    <style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/DCJ7XY12lwM' frameborder='0' allowfullscreen></iframe></div>
</div>


동영상을 삽입하는 방법은 몇 가지가 있는데, 이 방법이 모바일에서도 잘리지 않고 깔끔하게 재생되며 가장 쉽다.

<span style = "line-height:50%"><br></span>

## [블로그에 사진 삽입하기]

사진을 자신의 깃허브에 올리고 그곳의 링크를 가지고 와도 상관없지만, 깃허브의 용량 제한 때문에 그림이 많아지면 나중에 문제가 발생할 수 있다고 판단, <a href = "https://imageshack.us/">ImageShack</a> 사이트를 이용하기로 했다. 이곳에 가입하고 그림을 올리면 오른쪽에 GET LINKS / IMAGE SIZES 버튼이 나오는데, 여길 눌러서 나오는 링크를 블로그에 <img src ="이미지 주소"> 형식으로 걸어주면 된다.

<img src = "https://imageshack.com/a/img923/9969/Tnpsah.jpg">

깔끔하다. 개발자 도구 확인해 보시길.

<span style = "line-height:50%"><br></span>

## [블로그에 수식 삽입하기]

몇 가지 방법이 있지만, mathjax를 활용한 삽입이 가장 다루기 쉽고 깔끔하기에 이 방법을 소개한다. mathjax는 크로스 브라우저 자바스크립트 라이브러리라고 하는데; 삽입하는 방법만 알면 되기 때문에 자세한 건 알 필요 없고,

먼저, 블로그 html의 head 부분에 다음을 붙여넣는다 :

```html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
  });
</script>
```

mathjax는 latex문법으로 수식을 생성하는데, 사용자가 편집하기 좋은 사이트를 소개한다 :

www.codecogs.com

여기에 들어가면 수식을 gui로 편집하기 쉽게 되어 있다. 마우스로 열심히 클릭해 수식을 만들면 중앙에 Latex 문법으로 된 수식이 자리잡을 것이다. 이 수식을 복사해 와서 블로그에 붙여넣는다. 그리고 붙여넣은 수식 앞뒤에 $를 붙여주면 완성.

편집기에서는 안 보일 건데, 업로드한 후에는 예쁘게 나와 있는 모습을 확인할 수 있을 것이다.

$\frac{1}{m}\sum_{i=1}^{m}(H(x^{(i)})-y^{(i)})^{2}$

헿헿



<span style = "line-height:50%"><br></span>

