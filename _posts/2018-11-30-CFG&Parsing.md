---
layout: post
title: CFG(Context Free Grammer), Parsing
categories : [NLP]
tags : [NLP]
---

---

<span style = "line-height:50%"><br></span>

[포스트 시작 전]

Chomsky의 Hierarchy에 의한 언어의 종류 - 

1) Unrestricted Grammer(type 0) 

2) Context Sensitive Grammer(type 1)

3) Context Free Grammer(type 2)

4) Regular Grammer(type 3)

4가지로 나눌 수 있는데, 이 중 type 0과 1은 Natural Language(자연 언어)에 속하고, type 2 와 3은 Programming Language에 속한다.(인공 언어)

<span style = "line-height:50%"><br></span>

### [CFG(Context Free Grammer, 문맥 자유 문법)]

CFG는 1950년대 중반, Noam Chomsky에 의해서 개발되었다.

---

<b>[문맥 자유 문법의 정의]</b>

문법 G = (V, T, S, P) 에서 모든 생성규칙이

A → x

의 형태를 가지면 G 를 문맥-자유 문법 (context-free grammar : CFG) 이라고 한다. 여기서 A ∈ V 이고, x ∈ (V∪T)* 이다.

또한 언어 L 에 대해서 L = L(G) 를 만족하는 문맥-자유 문법 G 가 존재하고 오직 그럴 때에만 L 을 문맥-자유 언어 (context-free language : CFL) 라고 한다.

---

위의 G 구성 요소 중 V는 Non-terminal의 기호 집합, T는 Terminal 기호 집합, S는 Non-terminal 시작 기호, P는 문장을 생성하기 위한 생성 규칙이다. 

### [Parsing(파싱)] ###

파싱이란,  일련의 문자열을 의미있는 토큰 (token) 으로 분해하고 이들로 이루어진 파스 트리 (parse tree)를 만드는 과정을 말한다. 이 때 분해하는 방법은 주어진 문법을 따른다.

<b>[문맥 자유 문법을 이용한 파싱의 예시]</b>

다음과 같은 문맥 자유 문법이 있다고 하자.

R1 : S -> NP VP

R2 : NP -> N

R3 : NP -> Det N

R4 : VP -> V NP

R5 : VP -> V

R6 : N -> Person Name \| He \| She \| boy \| Girl \| It \|cricket \| song \| book

R7 : V -> likes \| reads \| dogs

이 문법을 통하여 "He likes cricket"를 파싱하면,

<img src = "https://lh3.googleusercontent.com/wEYm-_N0Xie1pT4jiRVpG0ES-dPgYn3ugqZH93WHtzqqlmlveS7035vv0GgfWmdyUIHJJEtrMyI2l7hjfstvgfvQEIyyAOavHtLzrx_YIkGdIfQPzv-1XpoosJBnSRiwm8Kfsf7Od95BeISFqwx3cI3U6HZwcVUPlfFIOhcKkCqaBCp78ZgFGxHZQat55svOxcvmB9KiGxkN2-fKlbOOc0zEIXcm7ZeoO409i_MxQMRuTBadT8GB6ZeWNnud1_5HVL0EMLmZGVpZnfutwvyVpXW_mOJUauMOoBStWcm6TSVWShsq4n_ojf5yvrqu1aunBXCqkEs8txxL8Bp-jZueok42aXL_s9idyjnxu-kTUXCz3lBEi4LUITnee-8eGKf-hIfthmSC3T7wva8HWldnDsNNgR_0-JriH0VtVm87Y-CGiC7LoStdW5ItLP0Yc1ewpKi1ofuY02Y_qiQi1NXft-N9e_t_kM5R78Ank2siJt9x-q-fjXJEvQuakVUj75eVmd2DRVNecYLn8AaH62nVxdK4N3vLf2G4BFZvUrMcrZJa7nChrikzk2bWbS_aTYOkfWH5zvmBNDbxLWqc5UIA5fXscaSCPVUUXk5EMBY_NR-hlRT52vMbAusiqyh-kaNAL-RzRZOjXnMOy24ZXrh2-9U=w386-h508-no">



와 같이 된다.

