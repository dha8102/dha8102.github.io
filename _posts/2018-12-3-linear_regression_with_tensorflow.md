---
layout: post
title: Tensorflow로 선형 회귀 만들기
categories : [Deep learning]
tags : [Deep learning, Regression, Tensorflow, 모두를 위한 딥러닝]
---

<span style = "line-height:50%"><br></span>



## [선형 회귀]

선형 회귀에 관한 내용은 따로 포스팅하도록 한다.



## [텐서플로우로 선형 회귀 만들기]

일반적인 선형 회귀식은 y=ax+b의 직선으로 나타낼 수 있다. 최소자승법에 의해 x와 y의 값으로 하여금 적절한 a와 b를 찾아내게 하는 것이 선형 회귀인데, 텐서플로우에서는 다음과 같이 표현한다 :

$H(x) = Wx+b$

이 때, 최소자승식은 다음과 같다 :

$\frac{1}{m}\sum_{i=1}^{m}(H(x^{(i)})-y^{(i)})^{2}$

딥 러닝에서는 최소화시켜주어야 하는 값을 cost라고 지칭한다. 따라서 위의 식을 W와 b에 관한 cost로 나타내면 :

$cost(W,b) = \frac{1}{m}\sum_{i=1}^{m}(H(x^{(i)})-y^{(i)})^{2}$

우리는 텐서플로우로 하여금 많은 x와 y 세트를 집어넣어 $H(x^{(i)})$를 구해낼 것이다. 최소가 될 때  $H(x^{(i)})$(기대값)과 $y^{(i)}$(실제값)의 차이는 0에 가깝게 다가갈 것이다.

Gradient Descent 기법을 사용하여 cost함수를 최소화시키는 코드를 구현하였다.

~~~python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
~~~

필요한 라이브러리를 import한다. (seaborn은 pyplot을 조금 더 예쁘게 보여주는 라이브러리)

~~~python
ph_input = tf.placeholder(tf.float32, shape = [128, 1])
ph_output = tf.placeholder(tf.float32, shape = [128, 1])
~~~

128행 1열로 구성된 placeholder들을 선언한다. 한 번에 128개씩의 데이터까지 받을 수 있게 만들겠다는 뜻이다. 그러니까 최대로 맞출 수 있는 batch의 개념이 된다는 말.

~~~python
variable1 = tf.Variable(tf.random_normal([3]))  # W
variable2 = tf.Variable(tf.random_normal([3]))  # b
~~~

W와 b의 역할을 할 variable들을 선언한다. 이 때 [3]은 batch의 사이즈가 된다. 최대 128까지 맞출 수 있다. (그 이상으로 넣어도 돌아가긴 하지만 결과가 이상하다)

~~~python
output = variable1 * ph_input + variable2
loss = tf.reduce_mean(tf.square(output - ph_output))
optimizer = tf.train.GradientDescentOptimizer(0.01)
train = optimizer.minimize(loss)
~~~

output은 기대값이 될 것이다. loss는 cost를 텐서플로우 형식에 맞추어 나타낸 것이고, optimizer는 데이터들을 어떠한 학습률로, 어떤 최적화 기법을 사용하여 최적화할 것인지를 정의한 변수이다. train은 optimizer의 minize 메소드를 사용하여 loss를 optimizer의 방법에 맞춰 최적화한다는 뜻을 담고 있다. 결국 session을 열어 그래프를 돌릴 때 우리는 train을 기점으로 시작하면 된다는 이야기다.

(거의) 모든 텐서플로우를 활용한 딥 러닝은 위와 같은 구조에 맞추어서 그래프를 생성한다. 머릿속에 담아둘 것.

~~~python
init_op = tf.global_variables_initializer()
sess = tf.Session()
sess.run(init_op)
~~~

변수를 초기화하고 세션을 시작한다.

~~~python
varnum = []
var1print = []
var2print = []
~~~

이들은 나중에 그래프를 그릴 때 사용될 list들이다. varnum에는 epoch 숫자, varprint들에는 각 에폭마다의 예측된 W와 b값들이 들어간다.

~~~python
for i in range(10000):
    input_value = np.random.random_sample(128)
    input_value = np.reshape(input_value, (128, 1))
    output_value = input_value * 2 + 0.5
    output_value = np.reshape(output_value, (128, 1))

    sess.run(train, feed_dict={ph_input: input_value, ph_output: output_value})
~~~

epoch(반복횟수)를 10000으로 설정하고, 학습에 쓰일 데이터를 생성한다.

input_value는 random하게 생성한 128개의 숫자를 numpy([128,1]) 형식에 맞추어 넣고, output value는 input_value * 2 + 5의 값들을 똑같은 형식에 맞추어 넣는다. 그러니까 이 데이터가 제대로 학습된다면, 모델은 var1을 2로 수렴시킬 것이고, var2를 0.5로 수렴시킬 것이다.

마지막 줄에서는 만들어진 데이터들을 각각 placeholder들에 feeding하고 train을 진행시킨다.

~~~python
    if i % 1000 == 0:
        loss_value = sess.run(loss, feed_dict={ph_input: input_value, ph_output: output_value})
        var1_value, var2_value = sess.run([variable1, variable2])

        print('loss: %.3f' % loss_value)
        print('var1: ', var1_value)
        print('var2: ', var2_value)
        print('')
        var1print.append(var1_value[0])
        var2print.append(var2_value[0])
~~~

1000번마다 한 번씩 loss값과 var1, var2들의 값을 출력하기 위해서 따로 세션을 돌려서 값을 받아낸다.

var1,var2print에는 var1,var2_value의 첫 번째 값들을 추가시켜 나간다. (그래프 그릴려고)

결과는 다음과 같았다 :

---

loss: 6.168
var1:  [-0.25723684  0.07613225  0.9911254 ]
var2:  [-0.07696786 -2.0157108  -0.8618597 ]

loss: 0.027
var1:  [1.0213816 1.7091606 1.8589249]
var2:  [1.0217636 0.6544363 0.5748732]

loss: 0.011
var1:  [1.3697628 1.8128656 1.9092383]
var2:  [0.8386766 0.6005624 0.5487737]

loss: 0.006
var1:  [1.5941049 1.8794786 1.9415462]
var2:  [0.71761054 0.56461424 0.53133845]

loss: 0.002
var1:  [1.7380259 1.9222114 1.9622731]
var2:  [0.6404018  0.54168946 0.52021956]

loss: 0.001
var1:  [1.8307557 1.9497468 1.9756279]
var2:  [0.59064263 0.52691466 0.51305306]

loss: 0.000
var1:  [1.8907079 1.9675488 1.9842607]
var2:  [0.5585419  0.51738256 0.5084307 ]

loss: 0.000
var1:  [1.9295092 1.979069  1.9898494]
var2:  [0.5378429 0.5112365 0.5054495]

loss: 0.000
var1:  [1.9544458 1.9864748 1.9934407]
var2:  [0.5242374 0.5071963 0.5034901]

loss: 0.000
var1:  [1.9707134 1.9913046 1.9957832]
var2:  [0.5156961 0.5046603 0.50226  ]

---

var1들의 값은 2로, var2들의 값은 0.5로 수렴해 감을 볼 수 있다.

~~~python
for i in range(len(var1print)) :
    varnum.append(i*1000)
plt.plot(varnum, var1print)
plt.plot(varnum, var2print)
plt.show()
~~~

값들이 어떻게 최적화되어 가는지를 보기 위해 그래프를 만들어 출력한다.

결과는 다음과 같다 :

<a href="http://www.imagebam.com/image/6dd5a51066268044" target="_blank"><img src="http://thumbs2.imagebam.com/4c/95/76/6dd5a51066268044.jpg" alt="imagebam.com" align="center"></a>

파란색이 var1, 노란색이 var2이다. 대략 6000번의 epoch를 거치면서 각각 2.0과 0.5로 수렴하여 간다.

<span style = "line-height:50%"><br></span>

---

포스팅을 진행하면서 몇 가지 생각이 들었다. 첫 번째는 이런 간단한 회귀(고전 통계 프로그램으로는 단숨에 결과가 나온다)도 6000번이 넘는 횟수를 수행해야 적절한 값을 찾아내는 모델을 보면서 훨씬 더 복잡한 문제를 푸는 모델들은 도대체 epoch를 몇 번을 돌리는 것인가..에 대한 경외심이었고, 두 번째는 이런 많은 계산을 몇 초만에 해내는 GPU의 성능에 대한 경외심이었다. (750ti만 되어도 이 정도 속도가 나오네..)

아래는 전체 코드이다.

```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()


ph_input = tf.placeholder(tf.float32, shape = [128, 1])
ph_output = tf.placeholder(tf.float32, shape = [128, 1])


variable1 = tf.Variable(tf.random_normal([3]))  # W
variable2 = tf.Variable(tf.random_normal([3]))  # b

output = variable1 * ph_input + variable2
loss = tf.reduce_mean(tf.square(output - ph_output))
optimizer = tf.train.GradientDescentOptimizer(0.01)
train = optimizer.minimize(loss)

init_op = tf.global_variables_initializer()
sess = tf.Session()
sess.run(init_op)
varnum = []
var1print = []
var2print = []
for i in range(10000):
    input_value = np.random.random_sample(128)
    input_value = np.reshape(input_value, (128, 1))
    output_value = input_value * 2 + 0.5
    output_value = np.reshape(output_value, (128, 1))

    sess.run(train, feed_dict={ph_input: input_value, ph_output: output_value})

    if i % 1000 == 0:
        loss_value = sess.run(loss, feed_dict={ph_input: input_value, ph_output: output_value})
        var1_value, var2_value = sess.run([variable1, variable2])

        print('loss: %.3f' % loss_value)
        print('var1: ', var1_value)
        print('var2: ', var2_value)
        print('')
        var1print.append(var1_value[0])
        var2print.append(var2_value[0])

for i in range(len(var1print)) :
    varnum.append(i*1000)

plt.plot(varnum, var1print)
plt.plot(varnum, var2print)
plt.show()
```



<span style = "line-height:50%"><br></span>

\* 본 포스트는 김성훈 교수님의 "모두를 위한 딥러닝" 강좌를 참고하여 작성되었습니다.

<a href = "https://www.youtube.com/playlist?list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm"> 김성훈 교수님의 "모두를 위한 딥러닝"</a>

