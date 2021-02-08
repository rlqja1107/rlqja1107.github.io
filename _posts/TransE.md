---
layout: post
title: TransE 
subtitle: 지식 그래프로 Entity와 Relation을 임베딩하는 일반적인 방법
categories: recommendation system
tags: graph
comments: true
---
  
# TransE
## 읽은 날짜  
2021-02-08  

## 논문 목적  
지식 그래프의 한 Approach로 Entity와 relation을 하나의 Space에 Embedding하기 위한 목적을 갖는다. TransE나 TransR 모두 지식 그래프 임베딩의 기초로서 작용한다. 다른 Network Embedding의 방법으로 Structure와 Connection을 유지하며 Graph에 Embedding시킨다. 여기서도 Connection 형태를 그래프에 임베딩하기 위해 Triplet형태의 Connection을 유지하며 이를 Graph에 적용시킨다. 그래프 임베딩 방법 중 하나인 지식 그래프 임베딩 방법을 익힌다.  
## 논문의 핵심 Point  
### 지식그래프  
지식 그래프는 Triple 형태로 관계를 유지한다. Triple은 주어, 서술어, 목적어 형태로 이루어져있다. 주어, 목적어는 Entity로 현실에서 예를 들면 사람, 동물의 한 개체를 나타낸다. 여기서 예를 들어, 옆에 사진에 나와있는 웰시코기를 들어보겠다. 웰시코기(주어)는 강아지(목적어)의 한 종류(서술어)이다 라고 표현이 된다. 이제 주어는 'Head(h)'라고 하고, 목적어는 'Tail(t)'이고 서술어는 'Relation(l)'라고도 표시한다. 기존에 알고 있던 네트워크를 그래프에 임베딩 할 때는 GCN, LINE과 같이 자신과 연결되어 있는 Entity는 누구인지, 즉 Neightborhood가 누구인지와 같은 outdegree의 개념으로 임베딩