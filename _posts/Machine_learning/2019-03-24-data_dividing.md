---
layout : post
title : train/dev/test data 나누기
categories : [Machine learning]
tags : [Deep learning, Machine learning]
---

---

<span style = "line-height:50%"><br></span>

딥 러닝 / 머신 러닝 모델을 만들면서 참 중요한 부분 중 하나가 훈련/테스트 데이터를 나누는 일이다. Robust한 모델을 만들기 위해서 이 부분을 정확히 알고 구현해야 한다. 

데이터셋이 train set, dev set, test set으로 나누어져서 존재하면 더할 나위 없이 고마운 일이겠지만, 그런 경우보다 그렇지 않은 경우가 훨씬 많다.

이 포스팅에서는 데이터 셋이 나누어져 있지 않을 때, 10-fold validation을 어떻게 하는지에 대하여 적는다.

## [Train set, Test set과 Dev set(Validation set)]

데이터는 총 train set, dev set(development set; validation set)과 test set 세 가지로 나누어 주어야 한다. test set은 모델이 잘 만들어졌는지를 평가할 때에만 쓰이고, 나머지 데이터를 train data와 dev set으로 나누어서 cross over validation을 진행한다.

cross over validation이란, dev set을 데이터의 일정 부분만큼 떼내고, 나머지 데이터로 train을 진행하고, 일정 step마다 dev set로 모델을 test하며 가장 dev set의 성능을 좋게 할 때 끊는(early stopping) 방법을 모든 데이터에 대해서 반복하는 것을 말한다.

문장으로 쓰면 이해하기 힘들기 때문에 step-by-step으로 설명한다.

## [Original Data]

기계학습을 위한 데이터셋들은 대부분 x : data와 y : label 형식으로 되어 있을 것이다. x의 경우는 다루고자 하는 데이터마다 천차만별일 것이고, y의 경우는 [0,0,1,0,0,0] 등으로 one-hot encoding 되어 있는 list의 list일 것이다 (즉 이중 list일 것이다.)

자 이제, 데이터의 총 개수를 n개, 전체 중 train set의 비율을 0.1, 전체 중 dev set(validation set)의 비율을 0.1이라고 하자.

## [Test set 나누기]

먼저, 전체 data의 10%만큼을 test data로 쓸 것이므로 미리 잘라낸다.

<img src = "https://lh3.googleusercontent.com/6-VC2QGFIgTlvECwhzuxSoDn2-neK2oRoQSBU4vyHyXWIpNrubL9OWC7GGTpe1l0iNT_TENV2rpWHWQetFBsQ5epfZXqNH1tuPWiPLv8qYKuUbkvCOMcI_W0Ym44lSmNlpKlJkfCqd8F1m6oNyHT5F5mrwYSYTEVPYtbV1rTglQpnq6Ueaw8BgkRz409XU2YsnhqOzDsJRwTUyl96BvceVr_fheondeAeSDmImAg1NlPfmsF_gCKyp6cE98g6RcAUqPCNvlTOQxpQqsb9WJZwTxWDiTSG1TKymQuaj3aAkef_quELX29r0Wv2TcNJMdQGeTzAqU4eudpw4M3uJ6Yb99DTFQ5UUQEX18HepFnUxzA7FD00T3l1uInxaS6f51d2wtHbtOcd4eEbYlW09GGxqezPxbQ95jGFp62gKfbO6yKxrNFiMoVpjFh1q4FLO9LlDmC4olfxRrO9mlGWqtl72zHOzr4sDPjVeAJPzEV_uFgmVzOJ3b0rjKd0vNH9DxSjDJW6cQmsdKwl0jBHxZji8aPpVxBH1AUCsVoKAempcO5hRxrMGQnd3N7s6jS2bDS5nr7in8vp7DExnzwQyUucMN4MszDOD71YZ2cSYrMdB66J4CyndnDHWbL6Dhjh6sAT0c3QDoAhtsUvEXsuVBhgLyAHLPpNQ=w1415-h147-no" width = "400">

이 original data를

<img src = "https://lh3.googleusercontent.com/OEh3zZk6jmZjcGfus-4jDb_uVzjwY6g9SElaX7vbeLBIQ5evxWywbMIqrzYhZFe34l-jSwphzF-pnouCH4W5HhCbMwGnAkD_G4A3MPBdnHiHoHmOBavQktM202VdC5WO0UM0-x4ZSjDgaqvBKK0rbxXo7LEWVsCPVbfE9519716kIvyRUkI8jb79L08K2T49vKtjb_SKyeBXk0Kmi5u9mxXbbK5Hz9A6xStgWIFSbi3pBw45SIGtjwvp6-ocj6_f6VYpHIEUbo2eXPEwj82JJv4GWady9K8zR7XNysdSxtruxHERhd3rzFFGG1yAHjvhGy4WeF5Vo-q2_COC2MTHVmn008UneajZnZvQTW3vsbkF50zWMwmOqaS52EH2hQBip049r8N9XUBw5dHdPeXk7I6yrwuXHaKxE8_94ox8bXKuLugU5K4dNfb_-KpiTAc-jcCBb18mrDb9sQZ7GJZsXQUK1Tc8ijhLB0ALH6_VXa5P-fP0HzQ7EEG3iaAyRjp7Qwtzvv2wu94oTJpuySb8FWVRE0hDA3Z86O-d_2sSJy51_UiTxBOExCw0Gd8a8eWwsG5aYzcJsIyDEeZ9wkByASJrOkTTZE0Z0jiGAG0hQMwtL5tgnWImpVKJ0ENTWKqPEvVZOrloN8LtPLYJgGamUll-PkrTbg=w1437-h147-no" width = "400">

요렇게 잘라낸다. 이 잘라낸 test data는 나중에 쓸 것이다.

이제, 이 잘라낸 데이터에서 dev set을 또 잘라낸다.

<img src = "https://lh3.googleusercontent.com/5Xf6jm0awykfqMUBl9kR4-Fe_JydONd5kgXeierF3ksFveQQmsyfL4Taqu0OM7ouT4gLyYv1neiqnMtZChye180U5KYD3Czvl_UXl2GTfqeIF--RDaTqRWC3Ut6VDdhf0ZE2tqkEm_1UzJ0mdV8cMC9kjroGdymrWJCi58Pwdx_1HL_HJ4BvdPWHkUlnJzVVnxI7r-47JB6cmrRBQYhXkJ2rbwssHj9oWckXIJn_Qs-UPLycitQGW8oxV898DdVS1SvR5nAzU2PbbPc48X16aJ9M1mBzJkjfbA_o4IcjgVINKI8OSN5H9e6opjv1_XZhxEgw0zGBPa4oHpBO9q1pmQ5Fl_SJFGVxnZzK7sFdZQF5yQEMohEIk-ey14s3BT2sSlQgw3dDioLY90CVtYfTUHbDvE9nrMta188OndWPGI3M7e1nDhl9l6btWmi10fRL7dw0To67jswgT_JbQ4ijEJFYfjf5WUv9W0xZ0EnwK_3lP23IxBUB2X4TIfmpDNXOHq_8ZxWZlPJEAbZVLkyolFVesNKz8dCZh_yT2lPb-XDZmjO4zrrmBNZwIbOBZU9vJ27CFhUmCdN9eJAbY0kQrwrQiT6YlNNrAKZX15spAn4tcSWm0qtPyDWiaaAwdWXl__RLzrsv37itK7RIgHTvrlsR6yhtxA=w1186-h148-no" width="400">

요로코롬. dev set은 "data after test data cutting"의 10%만큼 잘라낸다.

자 이제, "Data after dev set cutting" 데이터를 가지고 모델을 훈련시킨다. 훈련시키며 여러분이 설정한 특정 step 마다 해당 모델로 dev set을 테스트해 본다. 

dev set의 accuracy가 최대치를 찍고 다시 내려가려는 순간, 학습을 종료시킨다. 이를 <b>Early stopping</b> 이라고 한다. 이는 모델의 성능을 최대화하기 위한 weight tuning 기법이다.

("Early stopping을 정확히 언제 해야 하느냐" : "어느 시점부터가 Overfitting이 되는 순간이냐" 의 문제는 그리 쉬운 문제가 아니다. 본 포스팅에서는 dev set의 accuracy가 다시 내려가기 시작할 때라고 적었지만, 다른 곳에서는 train loss와 dev loss를 비교하란 말도 있고, 사람마다 조금씩 다른 것 같다. 일단 가시적인 성과가 가장 눈에 띄는 accuracy로 먼저 진행해 봅시다) 

이제 이 모델로 test data를 test해 본다. 그리고 그 때의 모델의 성능을 기록한다.

이제 dev set을 "data after test data cutting"의 다른 10%만큼 잘라낸다. 그리고 나머지를 모델의 훈련에 사용한다. 그리고 dev set의 성능이 최대가 될 때 stop하고 test data로 모델의 성능을 기록한다.

우리는 dev ratio를 0.1로 설정했으므로 이 과정을 10번 반복하면 된다. 그러면 dev set에는 모든 "Data after test data cutting"이 한 번씩 들어갈 것이다.

이러한 방법을 <b>Cross-validation</b>이라 한다. 이 경우는 10번 반복하므로 <b>10-fold Cross-Validation</b>이라고 부른다.

10번을 다 했으면? 그 때는 우리가 test data로 기록해 놓은 10개의 모델의 성능을 평균낸다, 이 값이 진짜 모델의 성능이 되는 것이다.