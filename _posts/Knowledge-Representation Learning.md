---
layout: post
title: Knowledge Representation Learning with Entities, Attributes and Relations 
subtitle: 논문 요약
categories: Recommendation System
tags: Graph
comments: true
---

# Knowledge Representation Learning with Entities, Attributes and Relations   
## Author  
Yankai Lin, Zhiyuan Liu, Maosong Sun  
## 읽은 날짜   
2021-02-07   
## 논문의 목적  
TransE, TransR, TransH 등은 지식 그래프 임베딩의 한 방법으로 Entity와 Entity 간에 Relation을 기준으로 Graph에 Embedding시킨다. 물론 TransE처럼 Entity간에 Relation을 나타내는데 Entity의 Type이 상관은 없지만, **Attribute**를 나타내는데 한계가 존재한다. 즉, Attribute Type이라고 한다면 다음과 같다. (e,a,v)로 표현된다. e를 여기서 사람이라고 한다면 a는 Gender라고 한다면 v는 Male이 된다. 즉, '김기범(e)의 Gender(a)는 Male(v)이다' 라는 관계가 성립된다. 여기서 Male은 하나의 Entity라고 보지 않고 하나의 Abstract 대상이라고 생각하면 쉽다.  
## 논문의 핵심 Point    
![image](https://user-images.githubusercontent.com/55014424/107148773-611a6f80-6998-11eb-8393-d74de3d8d23a.png) 
위와 그림과 같이 보면 이해가 더 쉽다. 노란 박스와 초록 박스는 각각의 Attribute를 나타낸다. 그리고 주황색 원은 각 Entity를 나타내고 선은 Entity간의 Relation을 나타낸다. 즉, '김기범의 친구는 OOO이다'가 선으로 표현된다.  
왼쪽의 그림은 일반 TransE, TransR처럼 모든 Entity와 Attribute를 하나의 Graph로 표현한 것이라면, 오른쪽 그림은 Attribute 공간을 따로 만들어 Entity 공간과의 관계로 표시된다. 즉, Attribute 공간과 Entity 공간을 따로 생각한다는 것이다. 이후에 다시 설명하지만, Entity와 Attribute간의 관계 유지를 위해 Entity 공간을 Single Layer를 통해 Attribute 공간으로 보낸다.   
Embedding **(X)**라고 했을 때, 우리의 Objective Function은 다음과 같다.    
\mathrm{P(S,Y)|X) = P(S|X)P(Y|X) = \prod_{(h,r,t)\in S}P((h,r,t)|X)\prod_{e,a,v\in Y}P((e,a,v)|X)}, S\subseteq E \times R \times  E, Y\subseteq E \times A \times V       
여기서 relational triple과 attributional triple은 서로 독립임을 가정한다. 즉 해석하면, Embedding이 주어졌을 때, 우리는 S, Entity와 Entity 간의 Relation,과 Y, Entity와 Attribute간의 관계의 확률을 최대화시키는 방향으로 진행한다.  
* 참고  


### Relational Triple Encoder(RTE)  
RTE는 Entity간의 Relation을 유지하며 graph에 임베딩하는데 목적이 있다. 학습 시에는 \mathit{P((h,r,t)|X)} 로 학습하는 대신에, 우리는 \mathit{P((h)|r,t,X)} 또는 \mathit{P((r)|h,t,X)} 또는 \mathit{P((t)|r,h,X)}를 학습시킨다. 여기서 TransE 또는 R를 유지시키며 다음과 같은 방법으로 학습시킨다.  
\mathit{P((h)|r,t,X)} = \mathit{P((h)|r,t,X) = \frac{e^{g(h,r,t)}}{\sum_{\hat{h}\in E} e^{g(\hat{h},r,t)}}}  
여기서, \mathit{g(h,r,t) = - \left \| h+r-t \right \|}_{L_{1}/L_{2}}+b_{1} 로 정의된다.   
### Attributional Triple Encoder(ATE)   
ATE는 Entity와 Attribute 사이의 Relation을 유지시켜 임베딩한다. 여기서는 (e,a,v)의 triple이 존재한다고 한다면, 다음의 식을 통해 관계를 유지시킨다.  
\mathit{P(v|e,a,X)= \frac{e^{h(e,a,v)}}{\sum_{\hat{v}\in V_{a}}e^{h(e,a,\hat{v})}}}  
여기서 h()는 위에서 잠깐 언급했던 Single Layer의 Transform을 의미한다. 즉, Entity 임베딩을 Attribute 임베딩으로 바꿔주는 함수다. 여기서 내가 이해한 바로는 Entity 공간을 Attribute 공간으로 바꾸고 그 둘 사이의 거리를 최소화하는 방향으로 진행되는 것으로 이해했다. transform 함수는 다음과 같다.  
\mathit{h(e,a,v) = - \left \| f(eW_{a}+b_{a})-V_{av} \right \|_{L_{1}/L_{2}} +b_{2}}   
여기서 \mathit{V_{av}}는 attribute value v의 임베딩을 의미하고 \b_{2}는 bias 상수를 의미한다.  
### Attribute 간의 상관성  
우리는 Attribute 간에 강한 상관성이 존재함을 알 수 있다. 예를 들면, 한국에 사는 사람들은 한국어를 사용하고, 원 단위의 돈을 사용한다 의 강한 상관성을 나타낸다. 여기에서의 핵심은 Attribute사이의 상관성을 고려한다. 





