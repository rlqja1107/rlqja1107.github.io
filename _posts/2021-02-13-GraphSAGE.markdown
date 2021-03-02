---
layout: post
title: "[논문]GraphSAGE"
subtitle: "Unseen node를 Embedding시키는 Inductive framework"
categories: paper
tags: graph
comments: true
---




# Inductive Representation Learning on Large Graph  
> 본 논문을 개인의 해석 및 이해를 바탕으로 정리  


<!-- <h2 span style="color: royalblue; font-weight: bold">알파벳 맞추기 게임</span></h2>  

<sup>1</sup>&frasl;<sub>9</sub>  
카드에 쓰인 알파벳($s_ {i}$) -->

## 저자  
William L.Hamilton, Rex Ying, Jure Leskovec  

## 논문 원본  
[GraphSAGE](https://arxiv.org/pdf/1706.02216.pdf)  
  


  
## 논문 목적   
---  
논문 목적을 설명하기에 앞서 **Transductive**와 **Inductive**의 차이점을 살펴보자.  

> **Transductive**  

Transductive는 지금까지의 GNN 방식이라고도 할 수 있다. Transductive는 관측한 데이터 중에서 train과 test set으로 나누어서 train시킨다. 이런 방식으로 우리는 train set을 통해 **fit**시키고 fit시킨 모델을 통해서 test를 통해 performance를 내게된다. test set의 경우에는 label를 알 수 없지만, dataset에서 유의미한 패턴 또는 정보를 추출할 수 있다.  
   
만약 이미 fit시킨 상황에서 새로운 노드(**unseen node**)를 embedding시키고 싶을 때, 처음부터 다시 train시켜야 하는 **computational cost**가 발생하게 된다.   
Transductive의 경우에는 **Predictive Model**은 별도로 존재하지 않는다.  
> **Inductive**  
  
  
Transductive의 경우에는 이미 관측된 데이터를 train과 test로 나눈 것을 보면 이미 test를 알고 있다는 전제 하에 나눈다. Inductive방식은 **Test Set**이 전혀 보지도 못했던 새로운 데이터 set을 이용한다는 것이다. 관측한 data(node, edge)는 train set으로 구성하여 먼저 모델을 학습시킨다. 그리고 trained된 모델을 이용하여 완전 새로운 test set의 label를 예측한다.   
Inductive방식을 통해 **Predictive Model**를 추출 할 수 있다. 이 Model를 이용하면 우리는 Unseen Node를 Embedding하려고 할 때, 처음부터 다시 계산할 필요가 없어진다. Inductive Representation의 가장 큰 장점이라고 생각한다.  
> **Inductive Framework** 소개    


대부분의 존재하는 방법론들은 **Transductive Framework**이다. 하나의 Graph에 대해서, 각 Node들을 모두 Optimize 하여 직접적으로 Embedding하려고 한다. **Transductive**에서 설명한 것처럼 대부분의 방법론들로는 Unseen Node를 Embedding하는데 어려움이 존재한다.  
이 논문에서는 각 Node를 직접적으로 Optimize하지 않고, Aggregate Function을 learning하며 Unseen Node를 Embedding할 수 있는 Inductive Framwork를 소개하고자 한다.   

## 논문 요약  
---  

다른 논문들의 Node를 저차원으로 Embedding하는 방식으로는 각 Node를 **직접적으로** Optimize하여 Embedding시키려고 한다. 이 논문에서는 신기하게 단지 Neighborhood Node의 feature, Aggregate function을 이용하여 **Generic Model**를 만들려고 한다. Unseen Node가 있을 때 이를 Graph에 Embedding시키는 Node Generator를 learning시킨다.   

지금까지의 Framework와는 새로운 **Framework**라는 점에서 흥미롭다. 이해할 때, 각 노드를 graph에 Embedding시키는 것이 아니라 input으로 노드를 넣으면 output으로 Embedding Vector가 나오는 **black box**를 learning시키는 방식으로 이해했다.    
### Aggregate Function   
    
Aggregate function은 그림으로 보면 이해하기 쉽다.  
<img width="550" src="https://user-images.githubusercontent.com/25279765/76823847-bc131780-6858-11ea-956f-0daf94342329.png">    

가운데 빨간 Node가 Main Node라고 생각하자. 우리는 k=1(1-hop) Neighborhood Node의 feature information을, k=2(2-hop, 이웃의 이웃 노드) node의 feature information을 모아서 Main Node를 표현하고 싶다. 이 때, Main Node의 이웃으로부터 feature 정보를 모으게 하는 함수가 **Aggregate Function**이다. Aggregate Function의 종류로는 이 논문에서는 3가지를 소개하고 있다.   
 
### Algorithm 1 - Mini Batch사용 x   
    
<img width ="600" src="https://user-images.githubusercontent.com/25279765/76822513-a7cd1b80-6854-11ea-9336-ea3133cd60f1.png">  
위의 알고리즘을 이해한 방식은 다음과 같다.   

1. k = 1 : 먼저 Main Node의 이웃 노드들의 **feature정보**를 aggregate하고, 자신의 feature정보를 concat해서 single layer network를 이용하여 Main Node를 표현한다.  
2. k = 2 : 1.을 통해 Main Node의 이웃 노드(k=1)들은 Main Node의 이웃 노드의 이웃 노드(k=2)로부터 정보를 이미 Aggregate하여 계산을 해놓았다. 따라서, Main Node는 이웃 노드(k=1)들의 $h_{v}^{1}$ 정보를 이용하여 이웃의 이웃노드들의 정보를 취합한다.  
3. k=3....., 이웃 노드들은 계속해서 k-번째 이웃의 node 정보를 갖고 있게 된다. 따라서, Main Node는 자신의 이웃 노드로부터 k-번째 이웃의 Node 정보를 취합할 수 있게 된다.  
   
   
### Unsupervised 방식을 이용한 Learning  

     
**Unsupervised** 상황에서 위의 알고리즘의 마지막 output인 $z_{v}$ graph기반으로 하는 loss function을 이용하여 Training시킬 수 있다.  

$$J_\mathcal{G}(\mathbf{z}_u) = - \log(\sigma(\mathbf{z}_u^\intercal \mathbf{z}_v)) - Q \cdot \mathbb{E}_{v_n \sim P_n(v)}\log(\sigma(-\mathbf{z}_u^\intercal \mathbf{z}_{v_n}))$$   
   
위의 **Negative Sampling**은 근처에 있는 노드는 비슷하게 표현하게 하고, 멀리 있는 노드들은 다르게 표현되도록 유도시킨다.  

$$v$$: $$u$$근처에 고정된 길이의 Random Walk한 Node  
$$Q$$: Negative Sampling의 수  
$$z_{u}$$: 이웃 노드의 Feature로부터 유도된 vector  
     
### Aggregate Function의 종류    
    
    
- **Mean Aggregator**   
  
$$ h_v^k \gets \sigma(\mathbf{W} \cdot \text{MEAN}(\{\mathbf{h}_v^{k-1}\}  \cup \{\mathbf{h}^{k-1}_u, \forall u \in \mathcal{N}(v) \}) $$   

Node $$v$$가 있을 때, 이웃 노드의 k-1th Representation인 $$h_{u}^{k-1}$$를 모두 평균으로 vector를 구한다. 중요한 것은, Node $$v$$와 **Concat**을 하지 않는다는 것이다.  
Concat을 시키지 않고, 평균으로 vector를 구한 뒤 바로, Single layer에 보낸다.  
> 의문점: 논문에서는 Mean Aggregator가 GCN에서 convolutional propagation과 유사하다고 하지만, GCN을 읽은 입장에서 어떤 부분이 유사한지 이해하지 못함..  


- **LSTM Aggregator**   


LSTM(Long Short-Term Memory)은 **RNN**의 특수한 Case다. 'a monkey is an animal'의 경우에는 RNN에서 다른 문장말고 이 문장만 보면되겠지만, 여러 문장에서 걸쳐 나온 주제와 같은 경우에는 뒷 문장도 모두 살펴보아야한다. 후자의 경우에는 긴 의존기간을 필요로 하여 학습이 진행되는데, 이를 **LSTM**이라고 한다.  
LSTM을 이용하여 Aggregate하는데 문제가 한 가지 있다. 바로 Input이 Sequential하게 처리된다는 것이다. 논문에서는 이를 node의 Neighbor를 random하게 섞어서 Input으로 넣어서 Sequential의 의미를 없애도록 했다.  

- **Pooling Aggregator**  

$$ \text{AGGREGATE}_k^\text{pool} = \max( \{ \sigma(\mathbf{W}_\text{pool}\mathbf{h}^k_{u_i} + \mathbf{b}), \forall u_i \in \mathcal{N}(v) \}) $$  
  
$$h_{u}^{k}$$, 각 Neighbor노드의 k-th representation을 개별적으로 Single Layer에 넣는다. Neighbor 노드의 수가 t개라고 한다면 $$h_{u}^{k}$$는 t개가 나올 것이다. 여기서 element-wise로 max값만을 추출한다.  
Max pooling방식을 이용하면 Neighbor 노드의 다양한 Aspect를 반영시킬 수 있다. 왜냐하면 Max값들은 서로 다른 Neighbor의 Information으로부터 추출되기 때문이다.  
Mulit-Layer Perceptron을 이용하여 정보를 취합할 수도 있었지만, 논문에서는 Single Layer를 이용한다.  
    
     
### Algorithm2 - Mini Batch 사용      
      
    
<img width = "600" src="https://user-images.githubusercontent.com/25279765/76823398-77d34780-6857-11ea-88ab-e2cfa187b226.png">    

Mini Batch를 방식을 이용해서 구하는 방식이다. $$B$$는 node를 Representation으로 나타내고 싶은 Node들이 모여있는 **Mini Batch**다. 여기서 주의해야 할 것은 $$B^{1}$$과 $$B$$는 서로 다른 개념이다.  
Line 2~7을 통해서 Mini-batch에서의 Neighbor를 먼저 Sampling한다. 개인적으로 해석하면, $B^{0}$의 경우에는 K번째 이웃의 Node들의 집합이라고 생각했다. $$B^{0}$$에서 0이 하나씩 증가할 때마다, K는 하나씩 감소한다.  

Line 9~15에서 K번째 이웃의 노드들의 feature정보를 K-1번 이웃의 노드들이 Aggregate하여 갖고 있고, iteration이 한 번 더 돌면, K-1번째 이웃의 노드들은 K-2번째 이웃의 정보를 갖고 있는데, K-2번째 이웃은 K-1번째 이웃의 정보를 Aggregate하여 Represent한다.   

Main Node로부터 제일 멀리 있는(K-th) Neighborhood Node의 정보를 K-1th Neighborhood Node로 취합되고, 또, K-2th Neighborhood Node는 K-1 th Neighborhood의 정보를 취합하면서 Main Node까지 전달하게 된다.   


---   


[[Kaist Winter Internship](http://dsail.kaist.ac.kr)]   
<img width="300" alt="DSAIL" src="https://user-images.githubusercontent.com/55014424/107319695-ad71c680-6ae2-11eb-8d78-3fcc3bfdce95.png">   
---








