# Cross-validation(교차검증)

* 일반적으로 데이터의 약 20%를 유효성 검사 데이터 세트로 만든다. ex) (test_size=0.2 ,train_size=0.8)
* 데이터셋의 크기가 작은 경우 테스트셋에 대한 성능 평가의 신뢰성이 떨어지게 된다.
* 이를 해결하기 위해 교차 검증으로 모든 데이터가 최소 한 번은 테스트셋으로 쓰이도록 한다.

<pre><code>
in []:
import pandas as pd

data = pd.read_csv('input.csv')

cols_to_use = ['Rooms', 'Distance', 'Landsize', 'BuildingArea', 'YearBuilt']
X = data[cols_to_use]

y = data.Price
</code></pre>

* 그런 다음, 우리는 결측 값을 채우기 위해 imputer를 사용하고 
* prediction을 위해 RandomForestModel을 사용하는 pipeline을 정의한다.

<pre><code>
in []:
from sklearn.ensemble import RandomForestRegressor
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer

my_pipeline = Pipeline(steps=[
    ('preprocessor', SimpleImputer()),
    ('model', RandomForestRegressor(n_estimators=50, random_state=0))
])
</code></pre>

* 우리는 scikit-learn에서 cross_val_score() 함수로 cross-validation을 얻는다.
* cv 파라미터로 fold의 개수를 설정했다.

<pre><code>
in []:
from sklearn.model_selection import cross_val_score

# Multiply by -1 since sklearn calculates *negative* MAE
scores = -1 * cross_val_score(my_pipeline, X, y,
                              cv=5,
                              scoring='neg_mean_absolute_error')
print(scores)

out[]:
[301628.7893587  303164.4782723  287298.331666  236061.84754543  260383.45111427]
</code></pre>

* Scikit-learn은 모든 지표를 정의하여 높은 숫자가 더 낫도록 하는 협약을 가지고 있다.
* 여기서 부정적인 것을 사용하면 비록 부정적인 MAE가 다른 곳에서는 거의 들어본 적이 없지만 그 관습과 일치하도록 할 수 있다.

# Conclusion

* 교차 검증을 사용하면 모델 품질을 훨씬 더 잘 측정할 수 있으며, 코드를 정리할 때의 이점도 추가된다.
* 즉, 더 이상 별도의 교육 및 검증 세트를 추적할 필요가 없다. 
* 특히 소규모 데이터셋의 경우 좋은 개선 효과를 얻을 수 있다.
