---
title:  "제주도 전력 사용량 예측"
categories: 
- Machine Learning
tags:
toc: true
toc_sticky: true
date: 2021-01-10
last_modified_at: 2021-01-10 16:45
---
제주도의 전력 사용량을 예측해보았습니다.

### 데이터 수집
제주도 전력 수요량 데이터 - 공공데이터 포털
수요량에 영향을 미치는 기상 정보 데이터 - 기상청

**수집 데이터 구성**

**전력량 데이터셋**

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/electronicdata.png)

행 - 년월일

열 – 해당 날짜의 시간

데이터 – 해당 날짜의 시간별 전력량

데이터 범위 – 400000 ~ 1000000 (단위 : mwh)

**기상 데이터셋**

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/weatherdata.png){: .align-center}

행 – 년월일시

열 – 기온, 강수량, 풍속, 습도, 적설, 지면온도 

기온 - -3 ~ 36 (단위 °C) 

강수량 0 ~ 95 (단위 mm)

풍속 – 0 ~ 12 (단위 m/s)

습도 – 0 ~ 100 (단위 %)

지면온도 – -4 ~ 65 (단위 °C)

### 데이터 구축

기존 전력량 데이터셋의 시간이 열에 존재해서

기상 정보와의 통합하려면 열과 행을 합치는 작업이 필요했습니다.(VLOOKUP 함수로 해결)

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/pretreatmentdata.png){: .align-center}

파이썬으로 저 작업을 하는 방법을 찾지 못해 엑셀로 진행하였으나 파이썬으로 전처리하는 것이 좋았을겁니다.
{: .notice--warning}



또한 강수량과 적설 특성이 전력사용량과 연관이 적어 제거했고

특성의 양이 부족하다고 판단해서 년월일시에서 월과 시간을 추출

월을 기준으로 계절 특성 생성, 시간을 기준으로 시간대 특성을 생성했습니다.

```python
weather = pd.read_csv('C:/DataSet/Data/jeju/2017~2018.csv') # 기온 데이터
electronic = pd.read_csv('C:/DataSet/Data/jeju/final17-20.csv') # 전력량 데이터
df = pd.DataFrame({'년월일시': pd.to_datetime(weather['일시'])}) # 일시를 이용해서 시간대와 계절을 나눔 / 데이터 프레임 형태 생성
df['month'] = df['년월일시'].dt.strftime('%m') # 년월일시 중 월만 추출
df['month'] = pd.to_numeric(df['month']) # 추출한 월 데이터 숫자 형태로 변경
df['hour'] = df['년월일시'].dt.strftime('%H')
df['hour'] = pd.to_numeric(df['hour'])
df = df[['month', 'hour']]
weather = pd.concat([weather, df], axis=1) # 기온데이터에 월, 시간 컬럼 추가
weather['시간대'] = weather.apply(lambda x: hour(x['hour']), axis=1) # 시간 컬럼을 이용해 hour함수 적용 (임의로 만든)
weather['계절'] = weather.apply(lambda x: weat(x['month']), axis=1) # 월 컬럼을 이용해 weat함수 적용
weather = weather[['기온', '풍속', '습도', '지면온도', '시간대', '계절']]
pfwd = pd.get_dummies(weather) # 원핫인코딩이 수행된(계절, 시간대에 관한 특성이 추가된) 기온 데이터(plus feature weather data)
electronic = electronic[['전력량']]
pfwdtrain = pd.concat([pfwd, electronic], axis=1)
```

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/integrateddata.png){: .align-center}

[30633 rows x 13 columns]

총 30633개의 데이터와 13개의 특성 데이터셋 구축

### 적합한 머신러닝 모델 선정

기온, 풍속, 습도, 지면 온도 같은 여러 특성을 이용해서 전력량을 예측하는 것이기 때문에 다항 회귀, 앙상블 모델이 적합하다고 판단했습니다.

또한 시간, 날짜의 진행에 따라 예측을 진행하기 때문에 시계열 모델 또한 모델 후보에 들어갔습니다.

모델의 평가 기준은 교차 검증을 통한 RMSE 점수입니다. 평균 제곱근 오차이기 때문에 평균이 작을수록 좋은 결과를 보일 것이라 예측할 수 있었습니다.

```python
def display_scores(scores):
print('점수 : ', scores)
print('평균 : ', scores.mean())
print('표준 편차 : ', scores.std())

def measureRMSE(model): # 평균 제곱근 오차 (rmse) 측정
scores = cross_val_score(model, pfwdX, pfwdY, scoring='neg_mean_squared_error', cv=5)
rmse = np.sqrt(-scores)
display_scores(rmse)
```

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/gradient.png){: .align-center}

그레디언트 부스트 모델을 선정했습니다. 시계열 모델도 해보고 싶었으나 정해진 시간내에 제가 습득하지 못해 진행을 못해보게 되었습니다.

### 모델 훈련

```python
# 그레디언트부스트
from sklearn.ensemble import GradientBoostingRegressor
FA_GBR = GradientBoostingRegressor(random_state=seed, n_estimators=100, learning_rate=0.05)
measureRMSE(FA_GBR) # 튜닝 전
```

위의 RMSE 점수와 같이 모델이 좋지 않았습니다.

**파라미터 튜닝**

적합한 파라미터를 찾지 못했기 때문이라 생각해 gridsearch를 이용해 파라미터를 튜닝했습니다.

```python
# 파라미터 튜닝 gridsearchcv
param_grid = [
{'n_estimators': [100, 200], 'max_features': ["auto", "log2", "sqrt"], 'max_depth': [3, 5, 8]
, 'min_samples_leaf': [1, 3, 5], 'learning_rate': [0.1, 0.05, 0.01]}
]
HTFA_GBR = GradientBoostingRegressor(random_state=seed)
grid_search = GridSearchCV(HTFA_GBR, param_grid, cv=5,
scoring='neg_mean_squared_error',
return_train_score=True)
grid_search.fit(pfwdX, pfwdY) # 튜닝 *오래걸림
grid_search.best_params_
grid_search.best_estimator_ # 최적의 파리미터
```

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/gridsearch.png){: .align-center}

결과로 주어진 조건 중에서의 최적의 파라미터 값이 나왔습니다.

### 최종 모델

```python
final_model = grid_search.best_estimator_
final_model = GradientBoostingRegressor(learning_rate=0.01, max_depth=8, max_features='log2',min_samples_leaf=5, n_estimators=200, random_state=777)
final_model.fit(pfwdX, pfwdY)
final_predictions = final_model.predict(testX)

final_mse = mean_squared_error(testY, final_predictions)
r2 = r2_score(testY, final_predictions)
final_rmse = np.sqrt(final_mse)
final_rmse # 튜닝 후 그레디언트부스트 오차
r2
measureRMSE(final_model)
```

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/finalgradient.png){: .align-center}

하지만 튜닝을 하기 전의 결과가 크게 다르지 않았습니다.

최종 모델을 테스트 데이터셋에 적용한 결과 

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/finalrmse.png){: .align-center}

RMSE가 검증 데이터에 적용한 결과보다 낮게 나와 검증보다 좋은 결과를 보여주었습니다.

### 평가와 결론

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/evaluation.png){: .align-center}

평균적으로 65만의 전력량을 사용하는데 5만의 오차는 적지 않은 양이었습니다.

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/r2.png){: .align-center}

R2의 점수가 0.65가 나왔기 때문에 아쉬운 성능을 보여줬습니다.



데이터의 전력량에 영향을 미치는 정확한 특성들을 생성하는 것이 중요한 것 같습니다.

현재 모델은 오차가 적지 않아 실제 사용할 수는 없습니다.

전력량과 가장 연관관계가 있을 것이라 생각한 기온 특성을 이용하지 못해 아쉬운 결과 

지금은 모델이 기온의 특성을 정확하게 이용하지 못하는 것으로 추정되는데 기온이 매우 낮을 때, 혹은 매우 높을 때 전력량이 상승한다는 특징을 이용한 특성을 만든다면 더좋은 결과를 얻을 수 있다고 생각합니다.

모델을 개선해 오차를 줄인다면 분명 전력량의 낭비를 줄일 수 있을 것입니다. 