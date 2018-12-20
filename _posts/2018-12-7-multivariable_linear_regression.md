---
layout: post
title: 다변량 회귀분석+파일 읽어오기
categories : [Deep learning]
tags : [Deep learning, Tensorflow, Regression, 모두를 위한 딥러닝]
---

<span style = "line-height:50%"><br></span>

김성훈 교수님 왈,

"일변량 회귀는 Toy Project 수준이고, 일반적으로 많이 쓰이는 회귀는 <b>다변량 회귀</b>이다"

꽤나 맞는 말. 다변량 회귀 개념도 되돌아볼 겸, 텐서플로우로 다변량 회귀를 진행하는 방법을 포스팅한다.

추가적으로 외부 파일에 있는 많은 데이터들을 텐서플로우에 올리는 방법을 알아본다.

## [텐서플로우에서 다변량을 표현하는 방법]

일반적으로 회귀식을 나타낼 때는 <a href="https://www.codecogs.com/eqnedit.php?latex=H(x)=Wx&plus;b" target="_blank"><img src="https://latex.codecogs.com/gif.latex?H(x)=Wx&plus;b" title="H(x)=Wx+b" /></a>로 표현한다. 이건 x가 일변량일 때다. 하지만 다변량일 때는?

"행렬"을 사용하여 표현한다.

<a href="https://www.codecogs.com/eqnedit.php?latex=w_{1}x_{1}&plus;w_{2}x_{2}&plus;w_{3}x_{3}&plus;...&plus;w_{n}x_{n}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?w_{1}x_{1}&plus;w_{2}x_{2}&plus;w_{3}x_{3}&plus;...&plus;w_{n}x_{n}" title="w_{1}x_{1}+w_{2}x_{2}+w_{3}x_{3}+...+w_{n}x_{n}" /></a>

요 식을

<img src = "https://lh3.googleusercontent.com/-0K0BOgL43u16dkJnz7QF1MvawfYmGJkABOZIlu6EpPAComjC2sHIHZMco1PVhoKflLTnhDVKP2pQOoI0GBMYxaitlEWABkAL3VECsuPaq4TtPmoSA9W8dGZJ3DEpuaG7Oa-lmYNQrehkmFYDk5z2p03WIpoOXiy2SAF2xTvEK8dbQeYOXqaKzwzLdP8O38A-PWny_YofGu0quazz7JdT4_gIWaydmcOA5dPwXnDBOOUTg5gjWEUHwa1UYD18QGwjuCK128gJ8PGag1g2T1fASPncTznRBOaIDD9gWdinoBMdNERoubezFBpjITtBR173znhG8nH15yQJGhDVzhPxPZQzmhTlmeSzW5xLF2u-xbyM1_zIriWB_7ia3hcPKvfPbIpG9gLd6CA9j2GlW3kqu4wgnYsr-YU5kdFwyDsA2-n_HNmDCWGKiSvyX4jLGOQ4ZC_82DTKTv6B9ZkDq5ZJjdW60pJxrC-tTvVP4bHF2wjaxxfnIlcSYG9KjpYcq34CGOStfK0YuAL6Yx2uFhqujiuf2va9n6joVYAhKFPXmqomHh1dtpBmMfLPcyz8ClOhzHQdZEAyfSjQZKQng3f7ki8_EjX6GeCtu187bLyQ5xAFfUFvxSx8tFH1mPFH2gMB1Xmfp1UsQWyWYMlE0v-DU4=w1613-h399-no">

요로코롬.

x1, x2, x3 3개의 변수를 가진 5개의 데이터 셋으로 각각의 가중치(w1,w2,w3)를 알아내기 위해서 위와 같은 행렬의 곱을 이용해 표현했다. 앞의 행렬을 X, 가중치 행렬을 W라고 하면 다음과 같이 표현 가능하다 :

<a href="https://www.codecogs.com/eqnedit.php?latex=H(x)=XW" target="_blank"><img src="https://latex.codecogs.com/gif.latex?H(x)=XW" title="H(x)=XW" /></a>

텐서플로우에서는 위와 같이, 가중치 행렬이 변량들의 뒤로 가게 된다.

위 그림의 경우에서, X는 [5, 3]이고 W는 [3, 1], 우변은 [5, 1]의 shape를 가진다. 요것이 일반화되면

<b>X가 [a, b]의 shape를 가지고 W가 [b, c]의 shape를 가지면 우변은 [a, c]의 shape를 가진다.</b>

shape 계산이 헷갈릴 때 요것만 머릿속에 넣고 있으면 안 헷갈린다.

## [텐서플로우에서 대용량의 파일을 읽어들이고 처리하는 방법]

파일이 용량이 클 땐 메모리에 모든 데이터를 올려서 처리가 불가능하다. 그래서 텐서플로우에서는 'Queue Runners' 라는 방법을 사용하는데, 이 방법은 밑의 그림처럼 file들을 queue로 나누어서 적절한 용량만큼 read, decode하여 example queue에 메모리 부하가 과도하게 걸리지 않을 만큼 잘라서 넣는 방법이다.

<img src = "https://lh3.googleusercontent.com/k6QWjGuKB2I-xOzQ9_f1DiFPAcbjSB8deWxUjoBtCkDVae7itHyHogEzUufmnaYjePcSIF0edtTVzAqbNLPzSmAEfSA97W1Hm0kBKDR0dgYsQ3GNrrgdisU1NjSk5DZDqskdVXzeGfTIzbgXyOL_vf91LovU0geYRvD8QE75Y9shM_svkvl2uTDw-fF7fPtnkvHrKn583QBxdVY70ToKgE-MSBOfQMV4rA4y4FONjoqSXCgBo54-HusZS5A1vFGXkIOv1Co2orDuvV-TuqMRiZfMqDFZSipc7k6_1nCa0jmBEwvHecnh4_4kT703SLyXG0i6okqQmAWpSzWvhA9wcQ1DVrR85B7q34AzBqSoQtBY4qCCm_Kuyeqg8Z3h8pJ-_1GQQPYCsVoco-JRUcbVUebzqu4mx8jwj-2NJhccey-YNFQsEcA61Scc7E5tjTuBWjJrXGRmQUvYYQrR1wkYexRYTSGYBYqmIvyDScE4xnKUTZy5Hp5DjkbLSIoP7Nx4WbbvBQgSW2kKJyGF0JuzM3UZICNdfx6UNaGdRV0-h2w3x-lbpiyR9FDyD7voLGZ7C7fL7BCZKm9lHHdd2EkNdomdQVXC1pFMsj-xy_sbv9hKlA6jzOT4-ZcvcDrJ9mgddTdDkORktL7nDl4_xMFYeLE=w1600-h398-no">

개념은 어려워 보이지만 막상 구현은 어렵지 않다. 이를 사용해서 다변량 회귀분석을 텐서플로우 코드로 만들어 보면,

```python
import tensorflow as tf
tf.set_random_seed(777)  # for reproducibility
```

텐서플로우 로딩하고

```python
filename_queue = tf.train.string_input_producer(
    ['data-01-test-score.csv'], shuffle=False, name='filename_queue')

reader = tf.TextLineReader()
key, value = reader.read(filename_queue)
```

csv 파일로부터 뭔가를 읽어들이는데,

먼저 filename_queue라는 파일 이름의 리스트를 만든다.

다음 reader로 어떻게 생긴 종류의 파일을 읽어들일 건지 정의하고,

key와 value 값을 통해 파일에 있는 데이터를 읽어들인다.

```python
record_defaults = [[0.], [0.], [0.], [0.]]
xy = tf.decode_csv(value, record_defaults=record_defaults)
```

recode_defaults는 읽어들일 행이 어떤 default값을 가질 건지를 정해준다. 지금 읽어들일 data-01-test-score.csv 파일에는 각 행이 4개의 열로 이루어져 있으므로 4개의 floating point 값을 저러한 이중 리스트 형식으로 넣어준다.

그리고 xy로 선언된 example queue에다가 reader에 들어있는 값들을 decode해주어 넣어준다. xy는 결과적으로 각 열들이 리스트로 들어있는 이중 리스트가 될 것이다. [[a,b,...], [c,d,...], [e,f,...], [g,h,...]]

```python
train_x_batch, train_y_batch = \
    tf.train.batch([xy[0:-1], xy[-1:]], batch_size=10)
```

이제 train_x_batch에는 xy의 첫 열~마지막 바로 전 열, train_y_batch에는  xy의 마지막 열을 넣고, batchsize(한 번에 세션에 올릴 데이터의 양)을 10으로 맞춘다. batch_size는 hyperparameter이다.

```python
X = tf.placeholder(tf.float32, shape=[None, 3])
Y = tf.placeholder(tf.float32, shape=[None, 1])

W = tf.Variable(tf.random_normal([3, 1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')
```

그 다음부터는 익숙한 코드가 나온다. 여기에서 X의 shape는 행에는 몇 개의 행이 올지 모르기 때문에 None으로 놓고, 열이 3개이기 때문에 [None, 3]으로 설정한다. 마찬가지로 Y는 [None, 1]로 설정한다. 이게 다변량 회귀분석의 전부다.

그리고 위에서 적어놓은 방법에 따라 W의 Shape는 [3,1]로, b는 [1]로 설정한다.

```python
hypothesis = tf.matmul(X, W) + b
cost = tf.reduce_mean(tf.square(hypothesis - Y))
optimizer = tf.train.GradientDescentOptimizer(learning_rate=1e-5)
train = optimizer.minimize(cost)
sess = tf.Session()
sess.run(tf.global_variables_initializer())
```

cost와 train을 설정하고, 세션을 초기화한다.

```python
coord = tf.train.Coordinator()
threads = tf.train.start_queue_runners(sess=sess, coord=coord)
```

이 부분은 queue runner을 사용할 때 사용 시작 전에 쓰는 코드이다. 그냥 통째로 갖다 쓰면 된다.

```python
for step in range(2001):
    x_batch, y_batch = sess.run([train_x_batch, train_y_batch])
    
    cost_val, hy_val, _ = sess.run(
        [cost, hypothesis, train], feed_dict={X: x_batch, Y: y_batch})
    if step % 100 == 0:
        print(step, "Cost: ", cost_val, "\nPrediction:\n", hy_val)
```

2001번 돌면서 학습을 진행한다.

```python
coord.request_stop()
coord.join(threads)
```

학습이 끝난 후 queue runner를 종료하는 구문을 다음과 같이 넣어준다.

```python
print("Your score will be ",
      sess.run(hypothesis, feed_dict={X: [[100, 70, 101]]}))

print("Other scores will be ",
      sess.run(hypothesis, feed_dict={X: [[60, 70, 110], [90, 100, 80]]}))
```

테스트 데이터를 넣고 예측을 해 본다.

생각보다 많이 간단하다.

아래는 전체 코드이다.

```python
import tensorflow as tf
tf.set_random_seed(777)  # for reproducibility

filename_queue = tf.train.string_input_producer(
    ['data-01-test-score.csv'], shuffle=False, name='filename_queue')

reader = tf.TextLineReader()
key, value = reader.read(filename_queue)

# Default values, in case of empty columns. Also specifies the type of the
# decoded result.
record_defaults = [[0.], [0.], [0.], [0.]]
xy = tf.decode_csv(value, record_defaults=record_defaults)

# collect batches of csv in
train_x_batch, train_y_batch = \
    tf.train.batch([xy[0:-1], xy[-1:]], batch_size=10)

# placeholders for a tensor that will be always fed.
X = tf.placeholder(tf.float32, shape=[None, 3])
Y = tf.placeholder(tf.float32, shape=[None, 1])

W = tf.Variable(tf.random_normal([3, 1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')

# Hypothesis
hypothesis = tf.matmul(X, W) + b

# Simplified cost/loss function
cost = tf.reduce_mean(tf.square(hypothesis - Y))

# Minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate=1e-5)
train = optimizer.minimize(cost)

# Launch the graph in a session.
sess = tf.Session()
# Initializes global variables in the graph.
sess.run(tf.global_variables_initializer())
# print(sess.run(xy))
# Start populating the filename queue.
coord = tf.train.Coordinator()
threads = tf.train.start_queue_runners(sess=sess, coord=coord)

for step in range(2001):
    x_batch, y_batch = sess.run([train_x_batch, train_y_batch])
    
    cost_val, hy_val, _ = sess.run(
        [cost, hypothesis, train], feed_dict={X: x_batch, Y: y_batch})
    if step % 100 == 0:
        print(step, "Cost: ", cost_val, "\nPrediction:\n", hy_val)

coord.request_stop()
coord.join(threads)

# Ask my score
print("Your score will be ",
      sess.run(hypothesis, feed_dict={X: [[100, 70, 101]]}))

print("Other scores will be ",
      sess.run(hypothesis, feed_dict={X: [[60, 70, 110], [90, 100, 80]]}))

'''
Your score will be  [[ 177.78144836]]
Other scores will be  [[ 141.10997009]
 [ 191.17378235]]
'''
```

\* 본 포스트는 김성훈 교수님의 "모두를 위한 딥러닝" 강좌를 참고하여 작성되었습니다.

<a href = "https://www.youtube.com/playlist?list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm"> 김성훈 교수님의 "모두를 위한 딥러닝"</a>

\* 이번 포스팅에 사용된 csv 파일은 아래 링크에 있습니다.

<a href = "https://drive.google.com/open?id=1pszUz1I-zam8QX8sJ8oqvsBZ3fhswp-a">파일 링크</a>

