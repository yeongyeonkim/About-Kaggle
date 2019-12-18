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

| got_pneumonia |	age |	weight |	male |	took_antibiotic_medicine |
| False | 65 | 100 | False | False |
| False | 72 | 130 | True | False |
| True | 58 | 100 | False | True |

* 사람들은 회복하기 위해 폐렴에 걸린 후 항생제를 복용한다.
  원시 데이터는 이러한 열 간의 강한 관계를 보여주지만
  got_pneumonia값이 결정된 후에는 took_antibiotic_medicine이 자주 변경된다. => 이것이 target leakage !
  
  
