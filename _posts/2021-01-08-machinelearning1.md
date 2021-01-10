---
title:  "Pycaret을 이용한 선거 분류"
categories: 
- Maching Learning
tags:
toc: true
toc_sticky: true
date: 2021-01-08
last_modified_at: 2021-01-08 01:30
---

# Pycaret을 이용한 선거 분류

## AutoML 패키지인 PyCaret

- Python의 오픈 소스 머신 러닝 라이브러리
- 결측값 대치
- 범주형 데이터 변환
- 기능 엔지니어링
- 하이퍼 파라미터 튜닝 등 모든 것을 자동화

## 경로 설정

In [1]:

```
path = 'C:/DataSet/Data/open data/open data/' # 경로 설정
```

## 데이터 불러오기

In [2]:

```
import pandas as pd
from sklearn.model_selection import train_test_split

import pandas as pd
train = pd.read_csv(path + 'train.csv', index_col='index')
test = pd.read_csv(path + 'test_x.csv', index_col='index')
submission = pd.read_csv(path + 'sample_submission.csv', index_col='index')
```

In [4]:

```
print(train.race.value_counts()) # 속성 안에 값 확인
White                    31248
Asian                     6834
Other                     4330
Black                     2168
Native American            548
Arab                       351
Indigenous Australian       53
Name: race, dtype: int64
```

## 데이터 구조 확인

In [5]:

```
print(train.shape)
print(test.shape)
(45532, 77)
(11383, 76)
```

## pycaret 설치 및 분류 함수 불러오기

In [ ]:

```
!pip install pycaret
```

In [6]:

```
from pycaret.classification import *
```

## 실험 환경 구축

- pycaret에서는 모델 학습 전 setup 함수를 이용한 실험 환경 구축이 필요
- 처음 입력칸은 컬럼의 데이터 타입을 올바르게 분류했는지 확인하는 Enter 입력
- 두번째 입력은 모델에 훈련할 데이터의 %를 입력 (가진 데이터를 모두 사용할 것이기 때문에 Enter 입력) 자동으로 훈련, 검증 데이터 7:3 분할

In [8]:

```
# 정답 레이블인 voted 컬럼을 target 지정
clf = setup(data = train, target = 'voted')
Setup Succesfully Completed!
```

|      |                  Description |        Value |
| ---: | ---------------------------: | -----------: |
|    0 |                   session_id |         7820 |
|    1 |                  Target Type |       Binary |
|    2 |                Label Encoded |   1: 0, 2: 1 |
|    3 |                Original Data |  (45532, 77) |
|    4 |               Missing Values |        False |
|    5 |             Numeric Features |           41 |
|    6 |         Categorical Features |           35 |
|    7 |             Ordinal Features |        False |
|    8 |    High Cardinality Features |        False |
|    9 |      High Cardinality Method |         None |
|   10 |                 Sampled Data |  (45532, 77) |
|   11 |        Transformed Train Set | (31872, 201) |
|   12 |         Transformed Test Set | (13660, 201) |
|   13 |              Numeric Imputer |         mean |
|   14 |          Categorical Imputer |     constant |
|   15 |                    Normalize |        False |
|   16 |             Normalize Method |         None |
|   17 |               Transformation |        False |
|   18 |        Transformation Method |         None |
|   19 |                          PCA |        False |
|   20 |                   PCA Method |         None |
|   21 |               PCA Components |         None |
|   22 |          Ignore Low Variance |        False |
|   23 |          Combine Rare Levels |        False |
|   24 |         Rare Level Threshold |         None |
|   25 |              Numeric Binning |        False |
|   26 |              Remove Outliers |        False |
|   27 |           Outliers Threshold |         None |
|   28 |     Remove Multicollinearity |        False |
|   29 |  Multicollinearity Threshold |         None |
|   30 |                   Clustering |        False |
|   31 |         Clustering Iteration |         None |
|   32 |          Polynomial Features |        False |
|   33 |            Polynomial Degree |         None |
|   34 |         Trignometry Features |        False |
|   35 |         Polynomial Threshold |         None |
|   36 |               Group Features |        False |
|   37 |            Feature Selection |        False |
|   38 | Features Selection Threshold |         None |
|   39 |          Feature Interaction |        False |
|   40 |                Feature Ratio |        False |
|   41 |        Interaction Threshold |         None |
|   42 |                Fix Imbalance |        False |
|   43 |         Fix Imbalance Method |        SMOTE |

## 모델 학습 및 비교

- compared_models 함수를 통해 15개의 기본 모델을 학습하고 성능을 비교
- AUC 기준으로 성능이 가장 좋은 3개의 모델을 추려내어 저장

In [9]:

```
best_3 = compare_models(sort = 'AUC', n_select = 3)
```

|      | Model                           | Accuracy | AUC    | Recall | Prec.  | F1     | Kappa   | MCC     | TT (Sec) |
| :--- | :------------------------------ | :------- | :----- | :----- | :----- | :----- | :------ | :------ | :------- |
| 0    | CatBoost Classifier             | 0.6946   | 0.7662 | 0.6572 | 0.7529 | 0.7018 | 0.3918  | 0.3956  | 32.58    |
| 1    | Light Gradient Boosting Machine | 0.6928   | 0.7657 | 0.6433 | 0.7582 | 0.696  | 0.3897  | 0.3951  | 1.803    |
| 2    | Gradient Boosting Classifier    | 0.6962   | 0.765  | 0.6385 | 0.7668 | 0.6968 | 0.3974  | 0.4041  | 57.61    |
| 3    | Linear Discriminant Analysis    | 0.6915   | 0.761  | 0.6609 | 0.746  | 0.7009 | 0.3848  | 0.3878  | 2.153    |
| 4    | Extra Trees Classifier          | 0.6896   | 0.7601 | 0.6433 | 0.7531 | 0.6938 | 0.3829  | 0.3879  | 4.806    |
| 5    | Ada Boost Classifier            | 0.69     | 0.7576 | 0.6529 | 0.7483 | 0.6973 | 0.3827  | 0.3865  | 14.47    |
| 6    | Extreme Gradient Boosting       | 0.6791   | 0.7489 | 0.6639 | 0.7259 | 0.6935 | 0.3582  | 0.3598  | 10.09    |
| 7    | Random Forest Classifier        | 0.6596   | 0.7168 | 0.6064 | 0.726  | 0.6608 | 0.3248  | 0.3301  | 0.7242   |
| 8    | Decision Tree Classifier        | 0.6117   | 0.6083 | 0.6439 | 0.6452 | 0.6445 | 0.2166  | 0.2167  | 4.202    |
| 9    | Naive Bayes                     | 0.4554   | 0.5549 | 0.0168 | 0.5672 | 0.0326 | 0.0013  | 0.0055  | 0.2524   |
| 10   | K Neighbors Classifier          | 0.5155   | 0.5094 | 0.5593 | 0.5567 | 0.558  | 0.022   | 0.022   | 4.554    |
| 11   | Quadratic Discriminant Analysis | 0.4532   | 0.5001 | 0.0002 | 0.2833 | 0.0005 | -0.0001 | -0.0011 | 1.202    |
| 12   | Logistic Regression             | 0.5467   | 0.4808 | 0.9985 | 0.5468 | 0.7066 | -0      | -0.0011 | 1.703    |
| 13   | SVM - Linear Kernel             | 0.5019   | 0      | 0.5643 | 0.544  | 0.5503 | -0.0094 | -0.0096 | 0.7253   |
| 14   | Ridge Classifier                | 0.6914   | 0      | 0.6612 | 0.7456 | 0.7008 | 0.3845  | 0.3874  | 0.3522   |

- CatBoost Classfier, Gradient Boosting Classifer, LGBM이 가장 좋은 3개의 모델입니다. 해당 모델은 best_3 변수에 저장

## 모델 앙상블

- 학습된 3개의 모델을 앙상블

In [10]:

```
blended = blend_models(estimator_list = best_3, fold = 5, method = 'soft')
```

|      | Accuracy |    AUC | Recall |  Prec. |     F1 |  Kappa |    MCC |
| ---: | -------: | -----: | -----: | -----: | -----: | -----: | -----: |
|    0 |   0.6925 | 0.7686 | 0.6469 | 0.7557 | 0.6971 | 0.3888 | 0.3936 |
|    1 |   0.6936 |  0.764 | 0.6371 | 0.7635 | 0.6946 | 0.3923 | 0.3987 |
|    2 |   0.7013 | 0.7688 | 0.6499 | 0.7681 | 0.7041 | 0.4067 | 0.4125 |
|    3 |   0.6977 | 0.7687 | 0.6387 | 0.7692 | 0.6979 | 0.4005 | 0.4075 |
|    4 |   0.6988 | 0.7697 |  0.646 | 0.7665 | 0.7011 | 0.4019 | 0.4079 |
| Mean |   0.6968 |  0.768 | 0.6437 | 0.7646 |  0.699 |  0.398 |  0.404 |
|   SD |   0.0033 |  0.002 | 0.0049 | 0.0048 | 0.0033 | 0.0066 | 0.0069 |

## 모델 예측

- 구축된 앙상블 모델을 통해 예측을 해보겠습니다.
- setup 환경에 이미 hold-out set이 존재하므로 해당 데이터에 대해 예측을 하여 모델 성능을 확인

In [11]:

```
pred_holdout = predict_model(blended)
```

|      |             Model | Accuracy |    AUC | Recall |  Prec. |     F1 |  Kappa |    MCC |
| ---: | ----------------: | -------: | -----: | -----: | -----: | -----: | -----: | -----: |
|    0 | Voting Classifier |   0.6932 | 0.7669 | 0.6388 | 0.7617 | 0.6949 | 0.3911 | 0.3972 |

- AUC가 0.7669로 꽤 준수한 성능을 보이는 것을 알 수 있습니다.

## test set에 대한 예측

- predict_model 함수를 통해 학습된 모델을 test set에 대해 예측

In [13]:

```
predictions = predict_model(blended, data = test)
```

In [14]:

```
predictions
```

Out[14]:

|       |  QaA |  QaE |  QbA |  QbE |  QcA |  QcE |  QdA |  QdE |  QeA |  QeE |  ... | wr_06 | wr_07 | wr_08 | wr_09 | wr_10 | wr_11 | wr_12 | wr_13 | Label |  Score |
| ----: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ----: | ----: | ----: | ----: | ----: | ----: | ----: | ----: | ----: | -----: |
|     0 |  3.0 |  736 |  2.0 | 2941 |  3.0 | 4621 |  1.0 | 4857 |  2.0 | 2550 |  ... |     0 |     0 |     1 |     0 |     1 |     0 |     1 |     1 |     2 | 0.6588 |
|     1 |  3.0 |  514 |  2.0 | 1952 |  3.0 | 1552 |  3.0 |  821 |  4.0 | 1150 |  ... |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     2 | 0.8742 |
|     2 |  3.0 |  500 |  2.0 | 2507 |  4.0 |  480 |  2.0 |  614 |  2.0 | 1326 |  ... |     0 |     1 |     1 |     0 |     1 |     0 |     1 |     1 |     1 | 0.4702 |
|     3 |  1.0 |  669 |  1.0 | 1050 |  5.0 | 1435 |  2.0 | 2252 |  5.0 | 2533 |  ... |     1 |     1 |     1 |     1 |     1 |     1 |     1 |     1 |     1 | 0.1940 |
|     4 |  2.0 |  499 |  1.0 | 1243 |  5.0 |  845 |  2.0 | 1666 |  2.0 |  925 |  ... |     0 |     1 |     1 |     0 |     1 |     1 |     1 |     1 |     2 | 0.8048 |
|   ... |  ... |  ... |  ... |  ... |  ... |  ... |  ... |  ... |  ... |  ... |  ... |   ... |   ... |   ... |   ... |   ... |   ... |   ... |   ... |   ... |    ... |
| 11378 |  5.0 |  427 |  5.0 | 1066 |  5.0 |  588 |  1.0 |  560 |  2.0 | 1110 |  ... |     0 |     1 |     1 |     0 |     1 |     0 |     1 |     1 |     1 | 0.3734 |
| 11379 |  1.0 |  314 |  5.0 |  554 |  5.0 |  230 |  1.0 |  956 |  2.0 | 1173 |  ... |     1 |     1 |     1 |     1 |     1 |     1 |     1 |     1 |     2 | 0.8797 |
| 11380 |  1.0 |  627 |  2.0 |  799 |  1.0 |  739 |  2.0 | 1123 |  1.0 |  829 |  ... |     0 |     1 |     1 |     0 |     1 |     0 |     1 |     1 |     1 | 0.2216 |
| 11381 |  2.0 |  539 |  1.0 | 2090 |  2.0 | 4642 |  1.0 |  673 |  2.0 | 1185 |  ... |     0 |     1 |     1 |     0 |     1 |     1 |     1 |     0 |     1 | 0.3006 |
| 11382 |  2.0 |  541 |  4.0 |  900 |  5.0 |  691 |  2.0 | 1951 |  1.0 | 2317 |  ... |     1 |     0 |     1 |     0 |     0 |     1 |     0 |     0 |     2 | 0.6263 |

11383 rows × 78 columns

In [15]:

```
submission['voted'] = predictions['Score']
```

In [16]:

```
submission.to_csv('submission_proba.csv', index = False)
```

In [17]:

```
submission
```

Out[17]:

|       |  voted |
| ----: | -----: |
| index |        |
|     0 | 0.6588 |
|     1 | 0.8742 |
|     2 | 0.4702 |
|     3 | 0.1940 |
|     4 | 0.8048 |
|   ... |    ... |
| 11378 | 0.3734 |
| 11379 | 0.8797 |
| 11380 | 0.2216 |
| 11381 | 0.3006 |
| 11382 | 0.6263 |

11383 rows × 1 columns