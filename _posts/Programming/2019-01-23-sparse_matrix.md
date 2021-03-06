---
layout : post
title : 희소행렬(Sparse Matrix)
categories : [Programming]
tags : [NLP, Machine learning, Deep learning, Mathmatics, Programming]
---

---

<span style = "line-height:50%"><br></span>

희소행렬(Sparse Matrix)이란, 0이나 Null Data가 대부분이고 의미있는 데이터는 몇 없는 자료를 핸들링하기 위해 고안된 자료구조이다.

보통 NLP나 웹 관련 작업 중 각각의 문서별로 Word-Embedding을 해야 할 때, 기본이 되는 input 모양새는 전체 코퍼스의 단어 갯수만큼의 0이 있는 벡터를 만들고 그 중 해당 문서에서 나온 단어들에 해당하는 열만 1을 가지는 one-hot encoding을 사용한다.

```python
matrix = [[0,0,0,...1,0,0,1,...0],
          [0,1,0,...0,0,1,0,...0],
                    ...
          [0,1,0,...0,1,0,1,...0]]
```

(행의 개수 : 문서의 개수, 열의 개수 : 코퍼스의 전체 단어 개수)

요런 식으로. 그런데 이렇게 만들면 matrix 안의 0의 개수가 너무 많고 의미있는 1의 개수가 굉장히 적다. 이러한 데이터의 형태를 Sparse(희소)하다고 말하는데, 이를 해결하기 위해 Sparse Matrix 구조를 사용한다.

Sparse Matrix 구조를 다루는 방법은 여러 가지가 있다.(사실 만들기 나름이다)

파이썬 기준으로, 처음에는 딕셔너리를 사용했다.

```python
matrix = {(0,1):1, (2,2):3, (2,5):2, ...}
```

key값의 튜플은 (행,열)을 나타내고 value값은 그 위치에 저장되어 있는 숫자를 나타낸다.

그런데 이렇게 만들면 단순한 행 하나만 뽑아내는 데에도 전체 딕셔너리를 순회해야 하기 때문에 연산 시간에서 매우 손해를 본다. 내적 등은 말할 것도 없고...

그래서 리스트와 살짝 혼합해 봤다.

```python
matrix = [{0:3, 1:4, 6:2, ...}, {2:3, 3:1, 10:2, ...}, ...{5:3, 6:2, ...}]
```

여기서 각 딕셔너리는 각 행을 나타내고, 각 딕셔너리의 key는 열값, value는 그곳에 들어 있는 숫자를 나타낸다.

이렇게 만들면 해당하는 행을 순회 없이 바로 접근할 수 있어서 연산 시간이 많이 단축된다.

---

파이썬 같은 경우는 scipy에서 희소행렬 연산을 지원하는 <a href = "https://docs.scipy.org/doc/scipy/reference/sparse.html">라이브러리</a>를 제공한다. 들어가 보면 희소행렬을 표현하는 방법이 꽤 많음을 알 수 있다. 익숙해지면 이것도 쓸만할 것 같지만 적용해 본 사람들의 경험담에 따르면 속도가 빠르진 않다고(...)

구현도 어렵지 않은데 쓸 일이 있을 때마다 함수나 클래스로 정의해서 쓰면 될 것 같다.