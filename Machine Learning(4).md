# Pipelines

* 데이터 사전 처리 및 모델링 코드를 체계적으로 유지하는 간단한 방법
* 특히, 파이프라인 번들은 처리 전 단계와 모델링 단계를 묶어서 전체 번들을 하나의 단계처럼 사용 가능

파이프라인 생성자는 일련의 단계를 정의하는 이름(name)/추정량(estimator) 쌍 리스트를 가집니다.
마지막 추정량을 제외한 모든 것이 트랜스포머여야 합니다.(fit_transform() 메서드를 가져야 합니다.)

### Pipeline의 이점
1. Cleaner Code
2. Fewer Bugs
3. Easier to Productionize
4. More options for Model Validation

### Step 1: Define Preprocessing Steps (사전 처리 단계 정의)

사전 처리 및 모델링 단계를 결합하는 방법과 유사하게,
ColumnTransformer 클래스를 사용하여 서로 다른 사전 처리 단계를 결합한다.

 - 숫자 데이터에서 누락된 값을 해석한다.
 - 누락된 값을 무효화하고 범주형 데이터에 단일 핫 인코딩 적용
 
<pre><code>
in []: 
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder

// Preprocessing for numerical data
numerical_transformer = SimpleImputer(strategy='constant')
// imputer는 각 속성의 중앙값. 구해줌.(누락된 NaN 값을 평균 값으로 채워주는 것.)
// strategy='constant'일 때 fill_value 매개변수에 채우려는 값을 지정합니다.
// Preprocessing for categorical data
categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='most_frequent')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))
])

// Bundle preprocessing for numerical and categorical data
preprocessor = ColumnTransformer(
    transformers=[
        ('num', numerical_transformer, numerical_cols),
        ('cat', categorical_transformer, categorical_cols)
    ])
</code></pre>

### Step 2: Define the Model

<pre><code>
in []:
from sklearn.ensemble import RandomForestRegressor
model = RnadomForestRegressor(n_estimator=100, random_state=0)
</code></pre>

### Step 3: Create and Evaluate the Pipeline

Pipeline 클래스를 사용하여 사전 처리 및 모델링 단계를 묶는 파이프라인을 정의한다.
파이프라인을 통해 교육 데이터를 사전 처리하고 모델을 단일 코드 라인에 맞게 조정한다
파이프라인을 사용하여 X_valid의 처리되지 않은 feature를 predict() 명령에 제공하고,
파이프라인은 predictions을 생성하기 전에 자동으로 사전 처리한다.

<pre><code>
in []:
from sklearn.metrics import mean_absolute_error

// Bundle preprocessing and modeling code in a pipeline
my_pipeline = Pipeline(steps=[('preprocessor', preprocessor),
                              ('model', model)
                             ])

// Preprocessing of training data, fit model 
my_pipeline.fit(X_train, y_train)

// Preprocessing of validation data, get predictions
preds = my_pipeline.predict(X_valid)

// Evaluate the model
score = mean_absolute_error(y_valid, preds)
print('MAE:', score)

out[]:
MAE: 160679.18917034855
</code></pre>

# Conclusion

* 파이프라인은 기계 학습 코드를 청소하고 오류를 방지하는 데 유용하며, 특히 정교한 데이터 사전처리가 있는 워크플로우에 유용하다
