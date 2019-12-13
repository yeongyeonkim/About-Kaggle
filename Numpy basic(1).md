# NumPy 특징

수치 데이터를 다루는데 효율적이고 높은 성능을 제공 한다.

* 대규모 데이터를 빠르게

# ndarray

* NumPy의 핵심.
* ndarray : n-dimensional array object, 다차원 배열 객체
* 모든 요소가 동일한 dayatype (기본은 float64)
* 다차원 데이터도 연속 할당: 높은 성능의 비밀 (파이썬 리스트는 비연속)

# NumPy 성능

* NumPy와 Pandas를 사용하면, 반복문을 거의 사용하지 않고도 데이터 처리
* 코드가 줄어들 뿐만 아니라, 성능도 최대 수 백배까지 빠르다.

<pre><code>import numpy as np
ls = range(1000)
%timeit [i**2 for i in ls]
a = np.arange(1000)
%timeit a**2
</code></pre>


