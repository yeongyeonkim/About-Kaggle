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
