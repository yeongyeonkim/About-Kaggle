# indexing-selecting-assigning

* 파이썬에서의 데이터 작업에서 가장 먼저 배워야 할 것 중 하나는 관련된 데이터 포인트를 빠르고 효과적으로 선택하는 방법이다.

# Naive accessors

* 네이티브 파이썬 객체는 데이터를 인덱싱하는 좋은 방법을 제공한다. 판다는 이 모든 것을 가지고 다니는데, 이것은 시작하기에 편리하도록 도와준다.

review.country // 개체의 속성으로 접근가능.

reviews['country'] // Python 사전이 있으면 인덱싱([]) 연산자를 사용하여 그 값에 접근할 수 있다.

reviews['country'][0] // 단일 특정 값으로 드릴다운하려면 인덱싱 연산자 []를 한 번만 더 사용하면 된다.

드릴다운(Drill Down). 어느 하나의 차원을 기준으로 데이터 조회를 상위 수준에서 하위 수준으로 하는 방식을 의미합니다.

# Indexing in pandas

* Index-based selection

판다 인덱싱은 두 가지 패러다임 중 하나로 작용한다. 
첫 번째는 지수 기반 선택: 데이터에서 숫자 위치를 기준으로 데이터를 선택하는 것이다. iloc은 이 패러다임을 따른다.

<pre><code>
in []:
reviews.iloc[:, 0]

out[]:
0            Italy
1         Portugal
            ...   
129969      France
129970      France
Name: country, Length: 129971, dtype: object
</code></pre>

* Label-based selection

위치가 아닌 데이터 인덱스 값이 중요.



<pre><code>
in []:
out[]:
</code></pre>

<pre><code>
in []:
out[]:
</code></pre>

<pre><code>
in []:
out[]:
</code></pre>
