---
layout: post
title: 텐서플로우 시작하기
categories : [Deep learning]
tags : [Deep learning, Machine Learning, Tensorflow, 모두의 딥러닝, Framework]
---

---

<span style = "line-height:50%"><br></span>

### [Tensorflow(텐서플로우) 시작하기]

- Tensorflow(텐서플로우) : 구글이 오픈소스로 공개한 머신러닝 및 딥러닝 라이브러리.  그래프 형식으로 되어 있어 일반적인 언어 사용과는 약간 다른 사용법을 요한다. 일반인들이 사용하기 쉽도록 다양한 라이브러리를 제공하여 널리 쓰인다.
- placeholder, constant, variable 3가지 변수 유형을 가지고 있다.

### [텐서플로우 설치하기 (Ubuntu 16.04 기준)]

리눅스 운영체제(기준) 설치 방법은 아래 블로그에 자세히 기재되어 있어 이것으로 대체 :

<a href = "http://yahwang.tk/posts/37"> Yahwang의 기술블로그 - Ubuntu(16.04)에 Tensorflow-gpu 1.8.0 설치 (+ CUDA & CUDNN )</a>

### [텐서플로우 자료형]

1. placeholder : Input Data를 담아두는 공간

2.  variable :  Weight(가중치)를 담아둘 때 쓰임
3. constant : 상수를 넣는 부분. 

y = Wx + b 에서 x와 y는 placeholder, W는 variant, b는 constant

보통 텐서플로우로 연산을 수행 시, input data를 가공하여 placeholder에 담아 두고, variable과 constant을 조정해 가면서 모델을 만든다.

### [텐서플로우로 "Hello, Tensorflow! 찍어보기"]

* 텐서플로우는 변수 자체로 연산을 하는 것을 허용하지 않는다. 자료형 선언과 계산식 등을 정의하고 run을 시키면 그곳에서부터 input data가 있는 곳까지 거슬러 올라가서 거기서부터의 모든 그래프를 Device에 올리고 계산한 결과를 출력한다. 이 때 run을 포함한 그래프를 Session이라고 한다. 세션을 file 형태로 존재하고 우리가 입출력을 할 수 있다.

```python
import tensorflow as tf
hello = tf.constant("Hello, TensorFlow!")
sess = tf.Session()
print(sess.run(hello))
```

문자열을 출력하려면 위와 같이 해야 한다.

그냥 print(hello)를 하면 오류가 뜬다. 왜? 그래프를 device에 올려서 돌리지 않았기 때문.

특정 자료형을 실행시키려면 위와 같이 sess = tf.Session()으로 세션을 돌리겠다는 메소드를 sess에 저장하고, sess.run(자료형) 으로 실행시킨다.

<span style = "line-height:50%"><br></span>

\* 본 포스트는 김성훈 교수님의 "모두를 위한 딥러닝" 강좌를 참고하여 작성되었습니다.

<a href = "https://www.youtube.com/playlist?list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm"> 김성훈 교수님의 "모두를 위한 딥러닝"</a>

