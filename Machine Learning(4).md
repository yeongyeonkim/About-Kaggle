# Pipelines

* 데이터 사전 처리 및 모델링 코드를 체계적으로 유지하는 간단한 방법
* 특히, 파이프라인 번들은 처리 전 단계와 모델링 단계를 묶어서 전체 번들을 하나의 단계처럼 사용 가능

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
 
