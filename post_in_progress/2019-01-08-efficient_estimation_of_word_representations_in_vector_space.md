---
​-------------------- post in progress -----------------
layout : post
title : Efficient Estimation of word Representations in Vector Space
categories : [NLP]
tags : [NLP, Machine learning, Paper]
---

---

<span style = "line-height:50%"><br></span>

word2vec의 기본이 되는 논문을 리뷰한다. 매우 중요한 논문이니만큼 자세히 리뷰한다.

발표 : ICLR(2013)

저자 : Tomas Mikolov, Kai Chan, Greg Corrado, Jeffrey Dean

<span style = "line-height:50%"><br></span>

## [Abstract]

초록은 대량의 데이터로부터 단어를 벡터화하는 새로운 모델을 제안한다는 문장으로 시작한다. 이들이 제안하는 모델은 16억 단어의 데이터셋으로부터 높은 품질의 word vector를 만들어 내는 데에 하루도 걸리지 않는다고 한다. (예전의 다른 모델들보다 훨씬 좋은 성능이라고...)

이 모델의 성능은 모델이 설정한 단어들 간의 유사도(similarity)로 측정할 수 있다고 한다.

## [1. Introduction]

예전의 많은 NLP 시스템들은  단어들을 atomic unit(개개의 원자적 단위? 쯤으로 해석할 수 있겠다)으로 다루고 있어서 단어 간의 유사성에 대한 개념을 가지고 갈 수 없었다. 물론 이러한 시스템들의 장점도 있었다. 단순성과 강건성을 무기로 삼아, 많은 양의 데이터를 이들의 간단한 모델로 훈련시키면 적은 데이터로 훈련시킨 복잡한 모델의 성능을 뛰어넘는 결과를 보였다. 대표적인 것이 N-gram Model이다.

그러나 간단한 모델들은 곧 많은 한계에 부딪히게 된다. 대표적으로 자동 음성 인식 시스템을 위해 쓰이는 적절한 in-domain 데이터의 양은 제한될 수밖에 없다. 굉장히 잘 정제된 높은 품질의 말로 표현된 데이터만 쓸모가 있기 때문에...게다가 기계 번역을 할 때 corpora(말뭉치)가 몇십억 개 정도의 단어밖에 없는 언어들이 상당수인지라, 적은 수의 데이터만으로 훌륭한 성능을 낼 수 있는 모델의 필요성이 대두되었다.

다행히 최근 컴퓨터 기술의 발전으로 더 많은 데이터 세트를 더 복잡한 모델에 집어넣어서 훈련할 수 있게 되었다. 최근의 가장 성공적이었던 것은 단어의 분포를 표현한 것이다. 신경망에 기반한 언어 모델은 N-gram Model을 훨씬 뛰어넘는 좋은 성능을 보인다.

### [1.1 Goals of the Paper]

이 논문의 목적은 엄청나게 많은 양의 데이터셋으로부터 높은 품질의 word vectors를 추출하는 기술을 제안하는 것이다. (여기에서 엄청나게 많은 양은 수십억 개의 단어와 수백만 개의 단어장 정도의 양을 의미한다.) 예전에 제안되었던 모델들은 몇백만 개의 단어와 50 - 100 차원 정도의 word vector 이상을 성공적으로 학습시키지 못했다.

본 논문에서는 word vector들의 품질을 평가하기 위해서 최근에 제안된 기술을 사용하기로 했다. <b>multiple degress of similarity</b>라는 기술인데, 단순히 비슷한 언어가 서로 붙어 있다는 사실을 넘어서 복수의 유사성을 가진다는 주장을 바탕으로 제안된 기술이다. 이 기술은 Inflectional language(굴절어) 에서 많이 관찰될 수 있는데, 비슷한 어미를 가진 단어들은 벡터 공간 상에서 가깝게 위치하게 된다.

이러한 단어의 유사도는 놀랍게도 단순한 구문적 규칙을 넘어서는 속성을 가지게 된다. 예를 들어, 단어의 간격을 이용해서 word vector끼리의 대수적 연산을 실행할 수 있다. vector"King" - vector"Man" + vector"Woman" = vector"Queen" 이라는 수식이 가능하다는 말이다.

이 논문에서는 단어들 간의 선형 규칙들을 보존하는 새로운 모델 구조를 개발해서 벡터 연산의 정확도를 최대화하려고 한다. 이를 위해 syntactic(구문론적), semantic(의미론적) 규칙을 측정하기 위한 새로운 테스트 셋을 디자인했다. 추가적으로 학습 시간과 정확도가 word vectors의 차원과 학습 데이터 셋의 양에 의존한다는 것에 대해서도 논의하려 한다.

### [1.2 Previous Work]

연속 벡터로 단어를 표현하는 방식은 매우 긴 역사를 가지고 있다. 굉장히 유명한 모델 중의 하나는 NNLM(Neural Network Language Model)이다.

(논문에서는 NNLM에 대한 기본적인 설명과 발전된 모델들을 설명한다. NNLM에 대해서는 다음에 따로 포스팅하기로 한다.)

그러나 NNLM은 큰 단점을 가지고 있는데, computationally expensive라고 표현되어 있는데 쉽게 말하면 느리다는 얘기다.

## [2. Model Architectures]

단어의 연속적인 표현들을 추산하기 위한 LSA(Latent Semantic Analysis), LDA(Latent Dirichlet Allocation) 같은 많은 모델들이 제안되었다. 본 논문에서는 LSA와 LDA보다 훨씬 좋은 성능을 내는 신경망에 기초해서 학습된 단어의 분포(distributed representations of words learned by neural networks)를 표현하는 모델을 제시할 것이다.

먼저 모델의 복잡도를 정의하기 위해 모델을 완벽히 학습시키기 위한 parameter의 개수를 정한다. 그 다음, 정확도를 최대화하고 복잡도를 최소화하는 작업을 진행할 것이다.

먼저 모든 모델의 복잡도는 다음과 같이 정의할 수 있다.

$ O = E\times T\times Q $ - (1)

여기에서

E : training epoch의 수(보통 3~50)

T : training set의 단어 개수(보통  ~10억)

Q : 각각의 모델 구조마다 정의되는 parameter로 보임

모든 모델은 SGD와 역전파 알고리즘을 사용한다.

### [2.1 Feedforward Neural Net Language Model(NNLM)]

NNLM은 (1)에서 제안되었다. 이 모델은 input, projection, hidden, output 레이러로 구성된다. input 레이어에서는 N개의 이전 단어들이 V개의 단어장의 언어로부터 one-hot-encoding된다. 이 input 레이어는 N*D 차원을 가진 projection 레이어 P로 사영되는데, 이 때 shared projection matrix를 사용한다. 언제나 N개의 input만 들어가기 때문에 pojection 레이어를 구성하는 것은 상대적으로 쉬운 과정이다.

NNLM 모델의 복잡도는 projection과 hidden 레이어 사이의 계산 때문에 증가하는데, projection 레이어의 값들이 굉장히 밀도있게 들어서 있기 때문이다. N = 10 같은 보통의 경우에, P의 크기는 500~2000 즈음 되는데, 이 때 hidden 레이어 H의 크기는 보통 500~1000을 왔다갔다 한다. 게다가, hidden 레이어는 단어장 내 전체 단어의 확률분포를 계산하여 V의 차원을 가진 output 레이어를 만들어 내는 데에 쓰인다. 따라서 각 training example의 시간복잡도는 다음과 같다 :

$ Q=N\times D + N\times D\times H + H\times V $

(이 때 domination term은 H * V이다)

그러나, 몇 가지 실용적인 솔루션들은 이 방법을 피하는 것을 제안한다. 몇 가지 예로써 hierarchical version의 softmax 함수를 사용하던지, 학습 중에 정규화되지 않은 모델을 사용해서 모델의 정규화를 피하는 방법 등이 있다. 그리고 단어장을 이진 트리로 표현하면 결과의 개수는 $ log_{2}(V) $ 까지 줄어들 수 있다. 그러므로, 시간복잡도에 가장 큰 요인을 차지하는 term은 $ N\times D\times H $이다.

우리의 모델에서, 