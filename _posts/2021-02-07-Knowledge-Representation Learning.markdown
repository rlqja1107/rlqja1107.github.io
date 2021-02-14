---
layout: post
title: "[논문]Knowledge Representation Learning with Entities, Attributes and Relations" 
subtitle: "Relation을 Relational과 Attribute로 나누어 지식 그래프 임베딩"  
categories: recommendation
tags: graph
comments: true
---



# Knowledge Representation Learning with Entities, Attributes and Relations   
> 본 논문을 개인의 해석 및 이해를 바탕으로 정리  





## 저자    
Yankai Lin, Zhiyuan Liu, Maosong Sun  

## 논문 원본  
[KR-EAR](https://linyankai.github.io/publications/ijcai2016_krear.pdf)     


## 논문의 목적  
> Relation을 Attribute와 Relational로 구분  


TransE, TransR, TransH 등은 지식 그래프 임베딩의 한 방법으로 Entity와 Entity 간에 Relation을 기준으로 Graph에 Embedding시킨다. 물론 TransE처럼 Entity간에 Relation을 나타내는데 Entity의 Type이 상관은 없지만, **Attribute**를 나타내는데 한계가 존재한다. 
> Attribute를 여기서는 Entity를 표현하는 하나의 특징이라고 이해하면 편하다. 즉, 김기범 작성자의 성별도 하나의 Attribute이고, 내가 사는 지역도 Attribute로 표현이 가능하다. Attribute의 경우는 one-to-many인 경우가 많은데, one-to-one과 one-to-many와의 차이는 아래에서 예를 들었다.  



즉, Attribute Type이라고 한다면 다음과 같다. (e,a,v)로 표현된다. e를 여기서 사람이라고 한다면 a는 Gender라고 한다면 v는 Male이 된다. 즉, '김기범(e)의 Gender(a)는 Male(v)이다' 라는 관계가 성립된다. 여기서 Male은 하나의 Entity라고 보지 않고 하나의 Abstract 대상이라고 생각하면 쉽다.  
## 논문의 핵심 Point   
---  

![image](https://user-images.githubusercontent.com/55014424/107148773-611a6f80-6998-11eb-8393-d74de3d8d23a.png)  
위와 그림과 같이 보면 이해가 더 쉽다. 노란 박스와 초록 박스는 각각의 **Attribute**를 나타낸다. 그리고 주황색 원은 각 **Entity**를 나타내고 선은 Entity간의 Relation을 나타낸다. 즉, '김기범의 친구는 OOO이다'가 선으로 표현된다.  
왼쪽의 그림은 일반 TransE, TransR처럼 모든 Entity와 Attribute를 하나의 Graph로 표현한 것이라면, 오른쪽 그림은 Attribute 공간을 따로 만들어 Entity 공간과의 관계로 표시된다. **즉, Attribute 공간과 Entity 공간을 따로 생각한다는 것이다**. 이후에 다시 설명하지만, Entity와 Attribute간의 관계 유지를 위해 Entity 공간을 Single Layer를 통해 Attribute 공간으로 보낸다.   
Embedding (**X**)라고 했을 때, 우리의 Objective Function은 다음과 같다.    
   
$$\mathrm{P(S,Y)|X)=P(S|X)P(Y|X)=\prod_{(h,r,t)\in S}P((h,r,t)|X)\prod_{e,a,v\in Y}P((e,a,v)|X)}$$     
    
$$S\subseteq E \times R \times  E, Y\subseteq E \times A \times V$$  
     
여기서 relational triple과 attributional triple은 서로 독립임을 가정한다. 해석하면, Embedding이 주어졌을 때, 우리는 S(Entity와 Entity 간의 Relation)과 Y(Entity와 Attribute간의 관계)의 확률을 최대화시키는 방향으로 진행한다.  
* 한 가지를 빠트려서 추가  
TransE에서는 1:1 즉, one-to-one의 Relation만 표현이 가능하다. one-to-one, one-to-many를 해석하는데 어려움이 있었지만, 예를 보면 쉽게 이해가 가능하다.  
> * one-to-one : 대한민국(head) 수도(relation)는 서울(tail)이다. 라고 하면, 하나의 relation으로 하나의 tail이 결정된다.  
> * one-to-many : relation을 gender라고 하면, 많은 사람(head)들은 남성(tail)이다. 라고 하면 head가 n이 되어 1:n의 관계를 형성한다.  
  
TransE에서의 one-to-many relation의 한계를 **Attribute**를 이용하면 표현이 가능하다.   
      
### Relational Triple Encoder(RTE)  
  
RTE는 Entity간의 Relation을 유지하며 graph에 임베딩하는데 목적이 있다.   
  
$$\mathit{P((h,r,t)|X)}$$      
     
학습 시에는 위의 식을 학습시키는 대신에, 우리는  
   
$$\mathit{P((h)|r,t,X)} or \mathit{P((r)|h,t,X)} or \mathit{P((t)|r,h,X)}$$   
    
를 학습시킨다. 여기서 TransE 또는 R를 유지시키며 다음과 같은 방법으로 학습시킨다.  
  
$$\mathit{P((h)|r,t,X)} = \mathit{P((h)|r,t,X) = \frac{e^{g(h,r,t)}}{\sum_{\hat{h}\in E} e^{g(\hat{h},r,t)}}}$$      
   
여기서, $$\mathit{g(h,r,t) = - \left \| h+r-t \right \|}_{L_{1}/L_{2}}+b_{1}$$ 로 정의된다.   
### Attributional Triple Encoder(ATE)   
ATE는 Entity와 Attribute 사이의 Relation을 유지시켜 임베딩한다. 여기서는 (e,a,v)의 triple이 존재한다고 한다면, 다음의 식을 통해 관계를 유지시킨다.  
    
$$\mathit{P(v\mid e,a,X)= \frac{e^{h(e,a,v)}}{\sum_{\hat{v}\in V_{a}}e^{h(e,a,\hat{v})}}}$$   
  
여기서 h()는 위에서 잠깐 언급했던 Single Layer의 Transform을 의미한다. 즉, Entity 임베딩을 Attribute 임베딩으로 바꿔주는 함수다. 여기서 내가 이해한 바로는 Entity 공간을 Attribute 공간으로 바꾸고 그 둘 사이의 거리를 최소화하는 방향으로 진행되는 것으로 이해했다. transform 함수는 다음과 같다.  
  
$$\mathit{h(e,a,v) = - \left \| f(eW_{a}+b_{a})-V_{av} \right \|_{L_{1}/L_{2}} +b_{2}}$$     
   
여기서 $\mathit{V_{av}}$는 attribute value v의 임베딩을 의미하고 $b_{2}$는 bias 상수를 의미한다.  
### Attribute 간의 상관성  
우리는 Attribute 간에 강한 상관성이 존재함을 알 수 있다. 예를 들면, 한국에 사는 사람들은 한국어를 사용하고, 원 단위의 돈을 사용한다 의 강한 상관성을 나타낸다. 여기에서의 핵심은 Attribute사이의 상관성을 고려한다.    
  
$$\mathit{Y(e)=\left \{ (e,\hat{a},\hat{v})\in Y \right \}}$$     
  
우리는 다음 위의 새로운 Dention을 세운다. 이는 (e,a,v)를 제외한 나머지 Attribute를 의미한다. 여기서는 Y(e)가 주어졌을 때, 즉, (e,a,v)를 제외한 다른 Attribute를 통해 (e,a,v)를 예측한다는 의미로 받아들였다. 식은 다음과 같다.  
    
$$\mathit{P((e,a,v)\mid Y(e))= \frac{e^{z(e,a,v,Y(e))}}{\sum_{\hat{v}\in V_{a}}e^{z(e,a,\hat{v},Y(e))}}}$$  
    
$$\mathit{z(e,a,v,Y(e))\propto \sum_{(e,\hat{a},\hat{v}\in Y(e))}P((a,v)|(\hat{a},\hat{v}))(A_{a}\cdot  A_{\hat{a}})}$$    
    
    
여기서 $(A_{a}\cdot A_{\hat{a}})$은 두 Attribute를 곱하는 것으로 relatedness를 의미한다. 여기서는 코드를 짜보면서 $A_{a}$가 무엇을 의미하는지 파악할 필요가 있다. 그리고 $P((a,v)\mid (\hat{a},\hat{v}))$는 $(\hat{a},\hat{v})$ 가 주어졌을 때, $(a,v)$를 구하는 확률이다. 이는 두 Attribute의 상관성을 의미한다.  
  
마지막에 최적화를 할 때는 loglikelihood를 이용한다. 근데 뒤에 regularization factor를 곱해서 정규화를 시킨다. 정규화 식은 논문 (13),(14)를 참고하면 된다.  
### Negative Sampling  
지금까지 논문을 읽으면서 감을 잡았던 것이 모든 Edge 또는 모든 Node가 이용되는 식이 있으면, Negative Sampling으로 Time Complexity를 줄인다는 것이다. 여기서도 위의 식을 보면 모든 V를 이용하게 된다. 따라서, 여기서도 Negative Sampling이 이용된다. 다만, 궁금한 점은 다른 논문(LINE)에서는 Negative Sampling할 때 Sigmoid에 log를 씌우지만 여기서는 씌우지 않는다. 
![image](https://user-images.githubusercontent.com/55014424/107151053-f0795000-69a3-11eb-9145-dc9ebcd6752f.png)  
를 보면 log를 씌우지 않고 Negative Sampling을 진행한다.  
## 정리    


TransE, TransR을 읽을 때는 몰랐지만, Entity 간에 Relation에 집중을 했던 것 같다. 하지만, Network를 보면 Attribute를 나타내는 Network도 존재한다. 특히, 지식 그래프를 임베딩할 때는 Attribute가 많이 사용된다. 당장에 하나은행 프로젝트에서도 대부분이 Attribute로 구성되어있는 것을 보면, 이 논문은 지식 그래프 임베딩 할 때, Attribute도 고려할 때 도움이 많이 될 것이다.  


---  



[[Kaist Winter Internship](http://dsail.kaist.ac.kr)] 
<img width="300" alt="DSAIL" src="https://user-images.githubusercontent.com/55014424/107319695-ad71c680-6ae2-11eb-8d78-3fcc3bfdce95.png">   

---









