# Data leackage (데이터 누출)

# Introduction

* 데이터 유출은 교육 데이터에 대상(target)의 정보가 포함되어 있을 때 발생하지만,
  예측에 모델을 사용할 때는 유사한 데이털르 사용할 수 없다.
  따라서 training set는 물론 validation data도 우수하지만, 이 모델의 생산성은 떨어질 것이다.
  
* 즉, 누출로 인해 모델에 대한 의사 결정이 시작될 때까지 모델이 정확해 보이게 되고, 그 후 모델이 매우 부정확해 집니다.

* traget leakage와 train-test contamination 두 가지 누출이 있다.

### Target leakage

* 대상 누출은 예측 변수에 예측을 수행할 때 사용할 수 없는 데이터가 포함되어 있을 때 발생.
* 데이터를 이용할 수 있게 되는 타이밍이나 시간 순서로 목표 누출에 대해 생각하는 것이 중요.

ex) 누가 폐렴에 걸릴지 예측하고 싶을 때, 

* 사람들은 회복하기 위해 폐렴에 걸린 후 항생제를 복용한다.
  원시 데이터는 이러한 열 간의 강한 관계를 보여주지만
  got_pneumonia값이 결정된 후에는 took_antibiotic_medicine이 자주 변경된다. => 이것이 target leakage !
  
### Train-Test Contamination

* validation은 이전에 고려하지 않았던 데이터에 대해 모델이 어떻게 작동하는지를 측정하기 위한 것임
* 유효성 검사 데이터가 사전 처리 동작에 영향을 미치는 경우 이 프로세스를 미묘한 방법으로 손상시킬 수 있다.    이것을 Train-Test Contamination이라고 부른다.

예를 들어, train_test_split()를 호출하기 전에(예: 누락된 값에 대해 imputer를 적합시키는 것)전처리를 실행한다고 가정해 보자
=> 모델은 우수한 검증 점수를 받아 신뢰도를 높일 수 있지만, 결정을 내리기 위해 구현할 때는 성능이 떨어진다.

