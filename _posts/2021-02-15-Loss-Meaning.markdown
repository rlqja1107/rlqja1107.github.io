---
layout: post
title: "머신러닝에서의 Loss개념 정리"
subtitle: "머신러닝에서의 개념 정리"
categories: notion
tags: loss
comments: true  
---



<h2 style="color: royalblue; font-weight: bold">Multi-Class, Multi-Label 정리</h2>  

Multi-Class인 경우에는 각 Sample이 여러 Class 중 하나로 분류될 수 있음을 의미한다. **Sample**은 Node로 생각해도 무방하다.  
* 예시)   
Node 1이 사람, 동물, 곤충, 사물, 강아지 등 여러 Class 중 하나로 **분류**하는 문제  

Multi-Label은 Multi-Class와는 다른 개념이다. 하나의 Sample(Node)이 여러 Label를 갖을 수 있음을 의미한다.  

* 예시)  
Multi-Label의 경우에는 위의 예시에서 Node 1이 강아지라면, (사람, 강아지)의 여러 Label를 갖을 수 있다.  



<h2 style="color: royalblue; font-weight: bold">1. Cross-Entropy Loss</h2>   

### Multi-Label Classification  
  
<img width ='500' src ='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fl2fVS%2FbtqxaDzMo7G%2FU9QHVyal8u1yLXZM6W0eAk%2Fimg.png'>    
  
$$Cross-Entropy Loss = - \sum_{i}^{C} t_{i}log(s_{i})$$   
  
$$t_{i}$$: Ground-Truth(정답)   
$$s_{i}$$: Neural Network의 마지막 층의 아웃풋인 score에서 i번째 요소.  
score에 (0,1) 값을 맞추기 위해 sigmoid를 붙여서 사용하는 경우가 많다.   

### Pytorch 함수  
``` python  
loss_function = torch.nn.CrossEntropyLoss()
loss = loss_function(score, label)
```  
$$score$$: list형태로 Neural Network의 마지막 output인 score가 Label 별로 담겨있다. e.x)[0.245, 0.231, 0.787, ...]  
$$label$$: 어느 Label에 속해있는지의 값 또는 Label  

<h2 style="color: royalblue; font-weight: bold">1-2. Binary Cross-Entropy Loss</h2>  

Cross-Entropy에서 C=2가 되면 그것이 Binary-Cross Entropy loss로 정의된다. 대부분의 경우에, $$s_{i}$$에 sigmoid 함수를 이용하여 (0,1)을 이용하는 경우가 많다.   

$$BCE = -\sum_{i=1}^{2}t_{i}log(f(s_{i}))= -t_{1}log(f(s_{1}))-(1-t_{1})log(1-f(s_{1}))$$  

$$ f(s_{i}) = \frac{1}{1+exp(-s_{i})}$$  

### Pytorch 함수  
``` python  
loss_function = torch.nn.BCELoss()  
loss = loss_function(input, target)  
```  
Sigmoid Layer와 위의 BCELoss가 하나의 Class로 정의된 함수  
``` python  
loss_function = torch.nn.BCEWithLogitsLoss()  
loss = loss_function(input, target)   
```  
target은 어느 Label에 속해있는지의 List($$y_{n}$$)  
E.x) [1, 0]  

input은 $$x_{n}$$에 들어가는 값, Shape는 (N,*), N은 Batch 크기     
E.x) [0.67, 0.45]

(계속해서 정리) 
<!-- <h2 style="color: royalblue; font-weight: bold">2. Binary </h2>   -->
 
---
