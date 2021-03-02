---
layout: post
title: "[논문]Attention Is All You Need"
subtitle: "Transformer"
categories: paper
tags: others
comments: true  
---



# Attention Is All You Need  
> 본 논문을 개인의 해석 및 이해를 바탕으로 정리  


## 논문 원본 
[Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf)  

## 논문 요약   

<h3 style="color: royalblue; font-weight: bold">논문 목적</h3>  
  
**Sequence to Sequence(Seq2Seq)** Model로 recurrent layer를 사용하거나 convolution을 사용하여 Input의 Sequence를 Output Sequence로 Representation하도록 했다.  
위 Model의 문제점  
1. Sequence별로 처리하기 때문에 Parallelization의 한계가 존재.  
2. Input과 Output Sequence에서 멀리 떨어져 있는 것의 Dependency가 약하게 작용.  

Seq2Seq Model로는 Recurrent Work가 많았지만, 이 논문에서는 Recurrent를 제외시키고 오로지 Attention Mechanism으로만 표현하고자 한다. Attention을 이용하며 위의 두 문제를 해결하는데 초점을 두고 있다.   

<h3 style="color: royalblue; font-weight: bold">기존 Seq2Seq Model의 문제점</h3>  

<img width = "600" src ="https://user-images.githubusercontent.com/55014424/109517859-0be00280-7aed-11eb-8242-04ee9c97dd2e.png">   

guten, abend 등의 단어 정보를 하나의 context vector로 요약하여 표현하려고 한다. context vector로 표현하는데에 있어, 한 단어가 큰 비중을 차지하여 context vector를 표현하게 되는 병목현상의 문제가 발생하게 된다.  

위의 예시에서 guten으로 hidden state($$h_{2}$$)로 나타낼 때 갱신해야 한다. 이처럼, 매 단어마다 hidden state를 갱신하며 **previous hidden state**를 참고하여 이전 정보를 계속해서 담게 된다. 단어가 많아지게 된다면 갱신도 많이 해야된다는 문제가 존재한다.  
**논문 목적**에서도 언급했듯이 hidden state가 많아지게 되면, 즉 단어가 많아지게 되면 맨 처음의 단어와 맨 마지막 단어의 dependency가 약해지게 되는 문제점이 존재하게 된다.  

<h3 style="color: royalblue; font-weight: bold">Attention</h3>  

Attention 메커니즘에는 Query, Key, 그리고 Key에 대응되는 Value가 존재한다. 예시를 들면 다음과 같다.  
* E.x) I live in Seoul  

I를 기준으로 다른 단어와의 연관성을 보려고 한다. I는 **Query**, (live, in, Seoul)은 Key, 이에 대응되는 값을 Value로 보면 이해가 쉽다.  
논문을 참고하면, "I"의 Query와 "(live, in, Seoul)"의 Key를 output으로 mapping하는 것을 **Attention Function**이라 한다.  

" Scaled Dot-Product Attention"  

$$ Attention(Q,K,V) = softmax(\frac {QK^{T}} {\sqrt{d_{k}}})V$$  

Query가 각 Key 중에서 어느 key의 정보가 중요한지 계산한다. $$QK^{T}$$를 통해 




