# Intermediate

# Random Forest

* 다수의 결정 트리로부터 분류 혹은 회귀(Regressor) 분석
* 앙상블 학습 방법의 일종

<pre><code>
in []:
from sklearn.ensemble import RandomForestRegressor
import numpy as np

X = np.array([[-3,-2],[-4,-5],[3,4],[4,5]])
y = np.array([1,1,2,2])

model = RandomForestClassifier(n_estimators=3)
model.fit(X,y)

out []:
RandomForestClassifier(bootstrap=True, class_weight=None, criterion='gini',
            max_depth=None, max_features='auto', max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, n_estimators=3, n_jobs=1,
            oob_score=False, random_state=None, verbose=0,
            warm_start=False)
</code></pre>

<pre><code>
in []:
model.predict([[0,0]])

out []:
array([1])
</code></pre>
