---
layout: post
title: "Pandas 정리 - 하나은행 데이터 전처리를 위함" 
subtitle: "Pandas 전처리"  
categories: tip 
comments: true
---




# 데이터 전처리를 위한 pandas 정리  




## 목적  
Pandas로 전처리 또는 분석에 앞서 함수를 익히기 위한 목적  

## pandas Dataframe  
pd.DataFrame(data,columns=['차례대로 col이름 적기'],index=['적는 것에 따라서])  
data에서 dictionary 형태로 key값(column 이름) : index에 맞는 값 적기  
### 행 방향의 index 확인  
df.index : 행 방향의 index 
### data 얻기  
df.values 하면 array 형태로 나온다.  
e.x) array([['Beomwoo', 2013, 1.5],  
 ['Beomwoo', 2014, 1.7],  
 ['Beomwoo', 2015, 3.6],   
 ['Kim', 2016, 2.4],   
 ['Park', 2015, 2.9]], dtype=object)  
## 열과 관련하여 추가 또는 삭제  
### 특정한 열의 data 얻기  
df['열 이름'] or df.'열 이름'  
### 열 중에 일부만 가져오기  
df[['year','points']]
### 특정 열에 추가하기  
df['열 이름'] = [관련 데이터 값]
### 열과 관련하여 연산  
df['열 이름'] = df['열1'] - df['열2']  
### 열 삭제  
del df['열 이름']  
## 행과 관련하여 추가 또는 삭제  
### 행 가져오기  
df[0:3] 이런 식으로 특정 행을 가져올 수 있다.  
df.loc['행 이름'] - loc는 안에 조건을 달아서 가져올 수도 있고, 번호가 아닌 이름으로 가져오기  
df.iloc['행 index 번호']  
df.iloc['행 번호','열을 번호로 해서 가져오기'] - iloc는 행과 열을 번호로 가져오기  
### 특정 조건을 만족하는 행 가져오기  
df.loc[df['열 이름'] > 값,:]   
df.loc[(df['열 이름'] > 값 & df['열 이름2'] > 값), :]  
대입도 가능   
df.loc[df['열 이름'] > 값,'year'] = 0  
### 특정 조건을 만족하는 행 삭제  
df.drop(df.index[df['열']>0])  
index 안에 조건 넣기  
### Index명 또는 순서로 행 삭제  
df.drop([df.index[22], df.index[23]])
## NaN관련    
### Nan drop 행 없애기  
df.dropna(how='any') : Nan이 하나라도 있으면 삭제하기  
df.dropna(how='all') : 모든 행의 값이 Nan이면 행 삭제하기  
drop하고난 DataFrame을 반환  
### Nan에 값 넣기  
df.fillna(value=0.4) : Nan에 0.4 모두 넣기  
### 특정 열에서 Nan 값을 포함하는 행만 추출하기  
df.loc[df.isnull()['특정 열'],:]  
### Nan이 있는 열 삭제  
df.drop('열 이름', axis=1)  
## 분석용 함수들
### 열(아래) 방향으로의 합    
df.sum(axis=0) : 열 방향으로 합  
### 행(옆) 방향으로의 합  
df.sum(axis=1) : 행 방향으로의 합  
여기에 df.sum(axis=1, skipna=False)를 하게 되면 Nan이 배제되지 않는다.  
### 상관계수 구하기  
df['열1'].corr(df['열2'])  
### 공분산 구하기  
df['열1'].cov(df2['열2'])  
### 값을 기준으로 정렬하기  
df.sort_values(by='열이름')  
열 이름에 해당하는 값을 오름차순으로 정렬  
df.sort_values(by='열이름', ascending=False) : 내림차순으로 정렬하기  
## DataFrame에 적용되는 함수  
### 지정한 행 또는 열에서 중복 값 제외하고 Unique 값만 얻기  
df['열 이름'].unique()  
### 지정한 행 또는 열의 값 갯수 얻기  
df['열 이름'].value_counts()  
### 지정한 행 또는 열에서 입력한 값이 존재하는지 확인하기  
df['열 이름'].isin(['값1', '값2'])  
### 사용자가 지정한 함수 Dataframe에 적용하기   
func = lambda x : x.max() - x.min()    
df.apply(func, axis=0)   
## 이외의 함수들  
### 그룹별 통계정보를 위함  
df.groupby(['열 이름']).age.mean() : 열에서 age의 mean을 출력해내기  


### Pandas Data Type 확인  
obj.dtypes 하면 Series의 자료형 확인 가능  
