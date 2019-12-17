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
import pandas as pd
from sklearn.model_selection import train_test_split

// Read the data
X_full = pd.read_csv('../input/train.csv', index_col='Id')
X_test_full = pd.read_csv('../input/test.csv', index_col='Id')
// Remove rows with missing target, separate target from predictors
X_full.dropna(axis=0, subset=['SalePrice'], inplace=True) // missing value를 없애주고
y = X_full.SalePrice // y에 SalePrice를 담고
X_full.drop(['SalePrice'], axis=1, inplace=True) // X엔 이제 쓸모가 없어진 SalePrice를 drop
// Break off validation set from training data
X_train_full, X_valid_full, y_train, y_valid = train_test_split(X_full, y, 
                                                                train_size=0.8, test_size=0.2,
                                                                random_state=0)
// 학습 데이터,레이블 / 테스트 데이터,레이블 생성

// "Cardinality" means the number of unique values in a column
// Select categorical columns with relatively low cardinality (convenient but arbitrary)
categorical_cols = [cname for cname in X_train_full.columns if
                    X_train_full[cname].nunique() < 10 and 
                    X_train_full[cname].dtype == "object"]
// object 타입의 10개 미만의 열을 포함한 categorical list를 만든다.

// Select numerical columns
numerical_cols = [cname for cname in X_train_full.columns if 
                X_train_full[cname].dtype in ['int64', 'float64']]
// int형 열을 포함한 numerical list를 만든다.


// Keep selected columns only
my_cols = categorical_cols + numerical_cols
// 선별한 열들로 이루어진 리스트를 만들고 그것에 해당하는 테스트와 학습 데이터를 만든다.
X_train = X_train_full[my_cols].copy()
X_valid = X_valid_full[my_cols].copy()
X_test = X_test_full[my_cols].copy()
</pre></code>
 
<pre><code>
in []: 
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder

// Preprocessing for numerical data
numerical_transformer = SimpleImputer(strategy='constant')
// 숫자형이므로 onehot은 필요가 없음. imputer만 해준다.
// imputer는 각 속성의 중앙값. 구해줌.(누락된 NaN 값을 평균 값으로 채워주는 것.)
// strategy='constant'일 때 fill_value 매개변수에 채우려는 값을 지정합니다.

// Preprocessing for categorical data
categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='most_frequent')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))
])
//imputer로 널값을 없애고 onehot으로 숫자형으로 변환까지 시켜준다.


// Bundle preprocessing for numerical and categorical data
preprocessor = ColumnTransformer(
    transformers=[
        ('num', numerical_transformer, numerical_cols),
        ('cat', categorical_transformer, categorical_cols)
    ])
    
// 위에서 생성된 transformer들을 가지고 preprocessor 전처리기를 만든다.
</code></pre>

### Step 2: Define the Model

<pre><code>
in []:
from sklearn.ensemble import RandomForestRegressor

model = RandomForestRegressor(n_estimator=100, random_state=0)
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
// 만약 변경이 되더라도 model만 바꿔주면서.?

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

<hr/>
