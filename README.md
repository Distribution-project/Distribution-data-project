<h1> <제 2회 유통데이터 활용 경진대회> </h1>
산업통상자원부와 한국전자정보통신산업진흥회가 주최한 '제 2회 유통데이터 활용 경진대회' 코드 파일 정리 및 기록입니다.
<br>
<br>

**기간** : 2023.08.18 ~ 2023.09.05
  <br>
  <br>

# 🙋🏻‍♀️ 팀 소개
|[공유경](https://github.com/yOukyonG)|[김수정](https://github.com/sugenre)|[민수민](https://github.com/Su-Min57)|[박채나](https://github.com/chaena7)|
|:---:|:---:|:---:|:---:|
|<img src="https://github.com/yOukyonG.png" width="100px" height="100" >|<img src="https://github.com/sugenre.png" width="100px" height="100" >|<img src="https://github.com/Su-Min57.png" width="100px" height="100" >|<img src="https://github.com/chaena7.png" width="100px" height="100">|
|Data Analysis|Data Analysis|Data Analysis|Data Analysis|

<br/>
<br>

# 🏆️ 프로젝트 소개

<h3> 배경 및 필요성 </h3>
물류 서비스 산업은 빠르게 성장하고 변화하므로 AI와 데이터 분석을 활용한 수요 예측과 운송 및 인력 공급 계획을 효율적으로 수립하고, 환경변수를 고려한 운영의 필요성 존재
<br>
<br>

<h3> 목표 </h3>

```
1. 전략적 의사 결정 및 경쟁 우위 확보
2. 고객 서비스 향상
3. e커머스 풀필먼트
```

<br>

<h3> 제공 데이터 </h3>

```
- 상품 데이터
- 결제 데이터
- 구매 데이터
```

<br>


# 🗒 분석 방법 및 절차

<h3> 데이터 특성 파악 </h3>

- '판매 수량' 예측을 위해 반품 데이터를 제외한 매출 데이터만 사용
- 우편번호를 확인하여 대부분의 소매점 위치가 경북지역인 것 확인
- 상품 바코드 및 상품명을 확인하여, 총 8716개의 상품 중 같은 제품을 묶음과 낱개로 나누어 판매하는 것 확인
<br>

<h3> EDA </h3>
  <p align="center">
<img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/5bd72a3c-fcc0-4509-8725-283a8e5fdf43" width="80%" height="80%">
<img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/463e899d-d83f-4da5-bdb6-5d3cffdc9b58" width="80%" height="80%">
  </p>
  
- 일별 판매수량과 주별 판매수량 모두 7-8월에 급증하는 것 확인
- 일별 판매수량은 변동성이 크기에 주별 판매수량으로 분석을 진행하는 것이 더 유의미하다고 판단

<br>

<h3> 결측치 처리 </h3>

- 판매 수량 데이터에 결측치를 갖는 누락된 주차에 대해 이전 주차와 다음 주차 판매량 평균으로 결측치를 채움 

<br> 

<h3> 범주형 데이터 처리</h3>

- 머신러닝에서는 범주형 변수를 사용할 수 없기 때문에 옵션코드에 수치형 변수 (0,1)로 변환하는One hot encoding 적용
  
<br>

<h3> 연속형 데이터 처리 </h3>

- 변수들의 값의 범위가 일치하도록 조정하여, 연속형 데이터에 대해 정규화 기법인 Min-max Scaling 사용

<br>

<h3> 내부 변수 및 외부 변수 </h3>

  **내부 변수**
  <br>

  <p align="center">  
<img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/8e7426a9-6077-4227-854f-34b988867ab6" width="80%" height="80%">
  </p>
  
  - 내부 변수의 중요도는 상관분석과 Feature Importance를 이용하여 판단

  <br>
  <br>

  **외부 변수**

  <p align="center">
    <img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/121f4acf-f551-46b3-8fb2-49d047d4e652" width="60%" height="60%">
  </p>
  
  - 유통 판매량은 다양한 요인에 영향을 받기 때문에 유통 과정의 이해관계자인 소비자, 생산자, 소매업체와 고나련된 지표를 외부 변수로 사용
  - 해당 데이터의 탐색 결과를 반영하여 경북지역 데이터를 수집
  - 주가에 지연값(lag)를 추가하여 예측 상품과의 상관분석을 진행한 후 가장 높은 상관관계를 보인 3주로 lag 값 설정

<br>

<h3> 가중치 부여 </h3>

- 품목별 판매수량에 따른 영향도를 반영하기 위해 상관계수를 가중치로 부여
- 가중치 부여 방법
  <br> : (품목별 변수) * (상관 계수) = 가중치 적용된 변수

<br>

<h3> Trend(추세) 변수 </h3>

<p align="center">
<img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/283eebfa-263a-4625-bc61-bc787faa83aa" width="80%" height="80%">
</p>

- 예측해야 하는 6개월의 기간에 대한 실제 판매수량 정보를 모르기 때문에, 시계열 예측 모델에 변수로 사용되는 판매 수량의 이동 평균이나 lag값 등을 사용하기에는 적합하지 않다고 판단
- 따라서, 상품별 과거의 판매수량을 바탕으로 예측이 필요한 기간의 추세를 다항 회귀선을 통해 예측하여 모델이 함께 반영하도록 함
- 각 상품별로 2차, 3차, 4차 등 다항 회귀 모델을 활용하여 추세를 예측한 뒤,
 ‘trend’ 라는 변수로 추가하여 학습에 활용

<br>
<br>

# 모델

<p align="center">
<img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/1d0c8c12-3e78-47c3-979e-9f8139a8b86e" width="80%" height="80%">
  <br>
<img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/2b894fc3-0c0d-442d-aafe-012fbd33e431">
  <br>
<img src="https://github.com/Distribution-project/Distribution-data-project/assets/138547408/ffd4c51c-3534-4ebc-8203-120e6ce0cfae" width="80%" height="80%">
</p>


: LGBM, Randomforest, XGboost, Gradient Boosting 4가지 모델에 대해 feature importance를 진행한 결과, 선정한 변수에 LGBM과 Random Forest Model이 가장 적합
<br>
(추가적으로 RMSE를 성능지표로 사용했으며, 각 품목 별로 추세를 반영하는 정도를 확인)

<br>

<h3> 추가 조정 </h3>

**하이퍼 파라미터 조정**
<br>
최적의 예측 모델을 도출하기 위해, Pycaret 라이브러리를 활용하여 하이터 파라미터를 조정
-> 조정한 결과, Random Forest의 RMSE 값이 74에서 65로 개선됨

<br>
<br>

# 활용 방안
 - 풀필먼트 센터 내 최적화된 작업 인력 확보 및 비즈니스 의사 결정을 통해 안정적인 상품 출고 가능
 - 오차 개선을 통해 물류센터 내 다양한 운영 비용 절감 가능
 - 고객사 별 주문량 예측 서비스를 마케팅/영업 신규 상품 개발 및 프로모션에도 활용 가능
