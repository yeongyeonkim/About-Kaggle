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
