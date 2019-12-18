# XGboost

* 경사로 상승을 통해 모델을 만들고 최적화 하는 방법.
* 많은 Kaggle Competition에서 우위를 차지하여 다양한 데이터 셋에서 최첨단 결과를 달성한다.

# Introduction

* RandomForest method를 사용하여 예측한다. (a single decision tree 보다 더 나은 성능을 얻을 수 있음.)

* 우리는 RandomForest method를 "ensemble method"라고 부른다.
* 다음으로 Gradient Boosting이라고 불리는 또 다른 앙상블 방법에 대해 배워보자.

# Gradient Boosting

- Cycle을 거쳐 모델을 반복적으로 앙상블에 추가하는 방법이다.
- 이것은 예측이 매우 단순할 수 있는 단일 모델로 앙상블을 초기화하는 것으로 시작한다.

그런 다음 cycle을 시작한다 :
  1. 먼저 현재 앙상블을 사용하여 데이터 집합의 각 observation에 대한 예측을 생성한다.
     예측을 위해 앙상블의 모든 모델에서 예측을 추가한다.
  2. 이러한 예측은 손실 함수(예: mean squared error)를 계산하는 데 사용. 
  3. 새 모델을 앙상블에 추가하면 손실이 감소하도록 모델 파라미터를 결정합니다.
  4. 마지막으로, 앙상블에 새로운 모델을 추가하고
  5. 반복한다.

<pre><code>
in []:
import pandas as pd
from sklearn.model_selection import train_test_split

data = pd.read_csv('melb_data.csv')

cols_to_use = ['Rooms', 'Distance', 'Landsize', 'BuildingArea', 'YearBuilt']

X = data[cols_to_use]

y = data.Price

X_train, X_valid, y_train,  y_valid = train_test_split(X, y)

from xgboost import XGBRegressor

my_model = XGBRegressor()
my_model.fit(X_train, y_train)

from sklearn.metrics from mean_absolute_error

predictions = my_model.predict(X_valid)
print(mean_absolute_error(y_valid, predictions))

out[]:
269553.99606958765
</code></pre>

# Parameter Tuning

* XGBoost에는 정확도와 교육 속도에 큰 영향을 미칠 수 있는 몇 가지 매개 변수가 있다.

### 1. n_estimators

모델링 주기를 수행할 횟수를 지정한다. 이것은 우리가 앙상블에 포함하는 모델의 수와 같음.
  * 값이 너무 낮으면 성능이 저하되어 교육 데이터와 테스트 데이터에 대한 예측이 부정확
  * 값이 너무 높으면 과충족(overfitting)이 발생하여 교육 데이터에 정확한 예측을 유발하지만
    테스트 데이터(우리가 관심이 있는)에 대한 부정확한 예측을 유발합니다.

<pre><code>
in []:
my_model = XGBRegressor(n_estimators=500)
my_model.fit(X_train, y_train)
</code></pre>

### 2. early_stopping_rounds

* early_stopping_rounds는 n_estimators의 이상적인 값을 자동적으로 찾아준다.
* 유효성 검사 점수가 향상되지 않을 때 조기 정지로 모델의 반복을 중지한다.
* eval_est 매개 변수를 설정하여 검증 점수를 계산하기 위한 일부 데이터를 예약.

<pre><code>
in []:
my_model = XGBRegressor(n_estimators=500)
my_model.fit(X_train, y_train, early_stopping_rounds=5, eval_set=[(X_valid, y_valid)], verbose=False)
</code></pre>

### 3. learning_rate

* 각 요소 모델에서 단순히 예측 값을 더함으로써 예측 값을 얻는 대신에,
* 우리는 예측 값을 추가하기 전에 각 모델의 예측 값을 작은 수(learning rate)로 곱할 수 있다.

<pre><code>
in []:
my_model = XGBRegressor(n_estimators=500, learning_rate=0.05)
my_model.fit(X_train, y_train, early_stopping_rounds=5, eval_set=[(X_valid, y_valid)], verbose=False)
</code></pre>

### 4. n_jobs

* 런타임이 고려되는 대규모 데이터 셋에서는 병렬 처리를 사용하여 모델을 더 빠르게 구축할 수 있다.
* 일반적으로 n_job 파라미터를 컴퓨터의 코어 수와 동일하게 설정한다.
* fit 명령 중에 오랜 시간을 기다리는 대규모 데이터 셋에서는 유용합니다.

<pre><code>
in []:
my_model = XGBRegressor(n_estimators=500, learning_rate=0.05, n_jobs=4)
my_model.fit(X_train, y_train, early_stopping_rounds=5, eval_set=[(X_valid, y_valid)], verbose=False)
</code></pre>

# Conclusion

* XGBoost는 표준 표 데이터로 작업하기 위한 선도적인 소프트웨어 라이브러리.
* 매개변수를 주의 깊게 조정하면 매우 정확한 모델을 교육할 수 있다.
