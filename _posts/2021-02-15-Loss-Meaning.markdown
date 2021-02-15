---
layout: post
title: "머신러닝에서의 Loss개념 정리"
subtitle: "머신러닝에서의 개념 정리"
categories: notion
tags: loss
comments: true  
---


<h2 style="color: royalblue; font-weight: bold">Cross-Entropy Loss</h2>   

  
<img width ='500' src ='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fl2fVS%2FbtqxaDzMo7G%2FU9QHVyal8u1yLXZM6W0eAk%2Fimg.png'>    
  
$$Cross-Entropy Loss = - \sum_{i}^{C} t_{i}log(s_{i})$$   
  
$$t_{i}$$: Ground-Truth(정답)   
$$s_{i}$$: Neural Network의 마지막 층의 아웃풋인 score에서 i번째 요소.  

