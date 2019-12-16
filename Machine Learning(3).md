# Categorical-variaables (범주형 변수)

* 범주형 변수를 먼저 사전 처리하지 않고 Python의 대부분의 기계 학습 모델에 연결하려고 하면 오류가 발생한다.

# Three Approaches

### 1) Drop Categorical Variables

### 2) Label Encoding

* 레이블 인코딩은 각 고유 값을 다른 정수에 할당한다.

### 3) One-Hot Encoding

* 원본 데이터에 가능한 각 값의 존재(또는 부재)를 나타내는 새 열을 생성한다.
* 범주형 변수가 많은 수의 값을 갖는 경우 원핫 인코딩은 일반적으로 잘 수행되지 않는다.

# Example

* dtype은 칼럼에 텍스트가 있음을 나타낸다.

<pre><code>
in []:
// Get list of categorical variables
s = (X_train.dtypes == 'object')
object_cols = list(s[s].index)

print(object_cols)
out[]:
['Type', 'Method', 'Regionname']
</code></pre>

### Score from Approach 1 (Drop Categorical Variables)

<pre><code>
in []:
drop_X_train = X_train.select_dtypes(exclude=['object'])
drop_X_val = X_val.select_dtypes(exclude=['object'])

print(score_dataset(drop_X_train, drop_X_val, y_train, y_val)
out[]:
175703.48185157913
</code></pre>

### Score from Approach 2 (Label Encoding)

<pre><code>
in []:
from sklearn.preprocessing import LabelEncoder

// Make copy to avoid changing original data
label_X_train = X_train.copy()
label_X_val = X_val.copy()

// Apply label encoder to each column with categorical data
label_encoder = LabelEncoder()
for col in object_cols:
  
out[]:
</code></pre>

<pre><code>
in []:
out[]:
</code></pre>
