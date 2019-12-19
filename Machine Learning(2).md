# Intermediate

<hr/>

# Random Forest

* 다수의 결정 트리로부터 분류 혹은 회귀(Regressor) 분석
* 앙상블 학습 방법의 일종

<pre><code>
in []:
from sklearn.ensemble import RandomForestClassifier
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

<hr/>

# Missing Values (결 측값)

# Three Approach (세 가지 접근법)

1. 간단한 옵션 : 결 측값이 있는 열 삭제
-> 결 측값이있는 열을 삭제하는 것.

2. 더 나은 옵션 : 대체
-> 대치가 결 측값을 일부 숫자로 채 웁니다. 예를 들어, 각 열을 따라 평균값을 채울 수 있습니다.

3. 대치의 확장(An Extension To Imputation)
-> 이전과 같이 결 측값을 무시합니다. 또한 원래 데이터 세트에서 누락 된 항목이있는 
   각 열에 대해 대치 된 항목의 위치를 표시하는 새 열을 추가합니다.
  
# Approach 1

* 훈련 및 유효성 검사 세트를 모두 사용하고 있으므로 두 DataFrame에서 동일한 열을 삭제하는 데 주의.

* drop 명령어를 통해 컬럼 전체를 삭제할 수 있다. axis=1은 컬럼을 뜻한다. axis=0인 경우, 로우를 삭제하며 이것이 디폴트이다. 
* inplace의 경우 drop한 후의 데이터프레임으로 기존 데이터프레임을 대체하겠다는 뜻이다. 
* inplace=True는 df = df.drop('A', axis=1)과 같다.

<pre><code>
in []:
cols_with_missing = [col for col in X_train.columns if X_train[col].isnull().any()]

reduced_X_train = X_train.drop(cols_with_missing, axis=1)
reduced_X_valid = X_valid.drop(cols_with_missing, axis=1)

print(score_dataset(reduced_X_train, reduced_X_valid, y_train, y_valid))

out []:
183550.22137772635
</code></pre>

# Approach 2

* SimpleImputer를 사용하여 결 측값을 각 열의 평균값으로 바꿉니다.

<pre><code>
in []:
from sklearn.impute import SimpleImputer

my_imputer = SimpleImputer()
imputed_X_train = pd.DataFrame(my_imputer.fit_transform(X_train))
imputed_X_valid = pd.DataFrame(my_imputer.transform(X_valid))

imputed_X_train.columns = X_train.columns
imputed_X_valid.columns = X_valid.columns

print(score_dataset(imputed_X_train, imputed_X_valid, y_train, y_valid))

out []:
178166.46269899711
</code></pre>

# Approach 3

* 결 측값을 대치하고 어떤 값이 대치되었는지 추적합니다.

<pre><code>
in []:
// 원본 데이터가 변경되지 않도록 복사합니다 
X_train_plus = X_train.copy()
X_valid_plus = X_valid.copy()

// 대치 될 내용을 나타내는 새 열을 만듭니다.
for col in cols_with_missing:
    X_train_plus[col + '_was_missing'] = X_train_plus[col].isnull()
    X_valid_plus[col + '_was_missing'] = X_valid_plus[col].isnull()

// 대치
my_imputer = SimpleImputer()
imputed_X_train_plus = pd.DataFrame(my_imputer.fit_transform(X_train_plus))
imputed_X_valid_plus = pd.DataFrame(my_imputer.transform(X_valid_plus))

// 열 이름을 제거하여 다시 넣기
imputed_X_train_plus.columns = X_train_plus.columns
imputed_X_valid_plus.columns = X_valid_plus.columns

print(score_dataset(imputed_X_train_plus, imputed_X_valid_plus, y_train, y_valid))

out []:
178927.503183954
</code></pre>

# 결론

* 일반적으로 결 측값 (접근 2 및 접근 3)을 대치하면 결 측값이있는 열을 단순히 삭제했을 때 (접근 1)보다 더 나은 결과를 얻을 수 있다.
