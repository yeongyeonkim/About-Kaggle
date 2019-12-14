# Scikit-learn 서브 패키지

* 자료 제공: sklearn.datasets : 샘플 데이터 세트 제공

# 가장 주요한 두개 메소드

* fit 메소드: 입력 데이터를 적합화
* predic 메소드: 새로운 데이터를 예측

<pre><code>
from sklearn import svm
clf = svm.SVC()
clf.fit(데이터, 레이블)

result = clf.predict(테스트_데이터)
</code></pre>

# 간단한 예제

두 정수가 모두 음수면 -1, 둘 다 양수면 1

[1, 2] --> 1
[-3, -2] --> -1
[-4, -2] --> -1
[5, 2] --> 1
[-6, -3] --> -1
[2, 3] --> 1

<pre><code>
in []:
from sklearn import svm
데이터 = [[1, 2], [-3, -2], [-4, -2], [5, 2], [-6, -3], [2, 3]]
레이블 = [1, -1, -1, 1, -1, 1]

clf = svm.SVC()
clf.fit(데이터, 레이블)

테스트_데이터 = [[1, 3], [-2, -1]]
clf.predict(테스트_데이터)

out []: 
array([ 1, -1])
</code></pre>

# 정확도 측정

<pre><code>
in []:
from sklearn import svm, metrics
데이터 = [[1, 2], [-3, -2], [-4, -2], [5, 2], [-6, -3], [2, 3]]
레이블 = [1, -1, -1, 1, -1, 1]

clf = svm.SVC()
clf.fit(데이터, 레이블)

테스트_데이터 =  [[1, 3], [-2, -1]]
테스트_레이블 = [1, -1]

결과 = clf.predict(데스트_데이터)
결과

out []: array([ 1, -1])
</code></pre>

<pre><code>
in []:
metrics.accuracy_score(테스트_레이블, 결과)
out []:
1.0
</code></pre>

# 데이터 준비 - 붓꽃 데이터

<pre><code>
in []:
import pandas as pd
url = 'http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'
df = pd.read_csv(url, header=None)
df.head(10)

out[]:
    0	  1	  2  	3	          4
0	5.1	3.5	1.4	0.2	Iris-setosa
1	4.9	3.0	1.4	0.2	Iris-setosa
2	4.7	3.2	1.3	0.2	Iris-setosa
3	4.6	3.1	1.5	0.2	Iris-setosa
4	5.0	3.6	1.4	0.2	Iris-setosa
5	5.4	3.9	1.7	0.4	Iris-setosa
6	4.6	3.4	1.4	0.3	Iris-setosa
7	5.0	3.4	1.5	0.2	Iris-setosa
8	4.4	2.9	1.4	0.2	Iris-setosa
9	4.9	3.1	1.5	0.1	Iris-setosa

</code></pre>

<pre><code>
in []:

out []:
</code></pre>

<pre><code>
in []:
df.columns = ['꽃받침_길이' ,'꽃받침_너비', '꽃잎_길이', '꽃잎_너비', '품종']
df.head(10)

out []:
꽃받침_길이	꽃받침_너비	꽃잎_길이	꽃잎_너비	품종
0	5.1	3.5	1.4	0.2	Iris-setosa
1	4.9	3.0	1.4	0.2	Iris-setosa
2	4.7	3.2	1.3	0.2	Iris-setosa
3	4.6	3.1	1.5	0.2	Iris-setosa
4	5.0	3.6	1.4	0.2	Iris-setosa
5	5.4	3.9	1.7	0.4	Iris-setosa
6	4.6	3.4	1.4	0.3	Iris-setosa
7	5.0	3.4	1.5	0.2	Iris-setosa
8	4.4	2.9	1.4	0.2	Iris-setosa
9	4.9	3.1	1.5	0.1	Iris-setosa
</code></pre>

<pre><code>
in []:
from sklearn import svm, metrics

clf = svm.SVC()
clf.fit( df[['꽃받침_길이' ,'꽃받침_너비', '꽃잎_길이', '꽃잎_너비']], df['품종'])

문제 =  [[6.2, 3.4,  5.4, 2.4], [5.1, 3.2, 1.0, 0.2]]

예측결과 = clf.predict(문제)
예측결과

out []:
array(['Iris-virginica', 'Iris-setosa'], dtype=object)
</code></pre>

# train_test_split

train_test_split 는 학습용과 테스트용으로 나누어 준다(레이블 = 정답)
(4개의 튜플로 나누어 준다)
학습_데이터, 테스트_데이터, 학습_레이블, 테스트_레이블 = train_test_split(데이터, 레이블)

<pre><code>
in []:
from sklearn.model_selection import train_test_split

데이터 = df[['꽃받침_길이' ,'꽃받침_너비', '꽃잎_길이', '꽃잎_너비']]
레이블 = df['품종']

학습_데이터, 테스트_데이터, 학습_레이블, 테스트_레이블 = train_test_split(데이터, 레이블)

print( '학습용 개수:', len(학습_데이터), len(학습_레이블) )
print( '테스트용 개수:', len(테스트_데이터), len(테스트_레이블))

out []:
학습용 개수: 112 112
테스트용 개수: 38 38
</code></pre>

<pre><code>
in []:
from sklearn import svm, metrics
from sklearn.model_selection import train_test_split

데이터 = df[['꽃받침_길이' ,'꽃받침_너비', '꽃잎_길이', '꽃잎_너비']]
레이블 = df['품종']

학습_데이터, 테스트_데이터, 학습_레이블, 테스트_레이블 = train_test_split(데이터, 레이블)

clf = svm.SVC()
clf.fit(학습_데이터, 학습_레이블)

예측결과 = clf.predict(테스트_데이터)

정확도 = metrics.accuracy_score(테스트_데이터, 예측결과)
정확도

out []:
1.0
</code></pre>

<pre><code>
# Code you have previously used to load data
import pandas as pd
from sklearn.metrics import mean_absolute_error
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeRegressor


# Path of the file to read
iowa_file_path = '../input/home-data-for-ml-course/train.csv'

home_data = pd.read_csv(iowa_file_path)
# Create target object and call it y
y = home_data.SalePrice
# Create X
features = ['LotArea', 'YearBuilt', '1stFlrSF', '2ndFlrSF', 'FullBath', 'BedroomAbvGr', 'TotRmsAbvGrd']
X = home_data[features]

# Split into validation and training data
# 학습 데이터, 테스트 데이터, 학습 레이블, 테스트 레이블
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state=1)

# Specify Model
iowa_model = DecisionTreeRegressor(random_state=1)
# Fit Model
iowa_model.fit(train_X, train_y)

# Make validation predictions and calculate mean absolute error
val_predictions = iowa_model.predict(val_X)
val_mae = mean_absolute_error(val_predictions, val_y)
print("Validation MAE: {:,.0f}".format(val_mae))

# Set up code checking
from learntools.core import binder
binder.bind(globals())
from learntools.machine_learning.ex5 import *
print("\nSetup complete")
</code></pre>
