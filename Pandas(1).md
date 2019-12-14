# Pandas : 파이썬에서 가장 강력한 데이터 분석 도구

# Creating Data

* DataFrame과 Series라는 두 개의 핵심 object가 있다.

### DataFrame

* 데이터 프레임은 테이블이다. 각 항목은 행과 열에 해당한다.

<pre><code>
in []:
import pandas as pd
pd.DataFrame({'Bob' : ['I liked it.', 'It was awful.'],
              'Sue' : ['Pretty good.', 'Bland.']},
              index=['Product A', 'Product B'])
out[]:
          Bob        	Sue
Product A	I liked it.	Pretty good.
Product B	It was awful.	Bland.
</code></pre>

### Series

* 데이터 값의 순서이다. DataFrame이 테이블이면, Series는 목록이다.
* 시리즈는 본질적으로 DataFrame의 단일 컬럼.

# Reading data files

shape 속성으로 DataFrame의 크기를 알 수 있다.

<pre><code>
in []:
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv")
wine_reviews.shape
out[]:
(129971, 14)
</code></pre>

* DataFrame을 CSV file로 저장하는 방법. ex) DataFrame_name.to_csv("CSV_name")
