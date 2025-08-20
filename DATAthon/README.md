# DATA-thon - Mercari Price Suggestion Challenge
상품 텍스트 + 메타 데이터를 활용한 가격 예측 파이프라인.  
LightGBM 기반 회귀 모델을 중심으로, 텍스트 임베딩(TF-IDF+SVD)과 범주형 신호(스무딩 타깃 인코딩, 등급화)를 결합합니다.

---

## 문제 설정
- **목표**: 상품 정보(텍스트+메타)로 `price` 예측  
- **타깃**: `log1p(price)`  
  - 가격 분포의 긴 꼬리를 완화 → 안정적인 회귀 학습 가능  
  - 예측 후 `expm1` 변환으로 실가격 복원

---

## 전처리 단계

### 1. 기본 세팅
- 라이브러리: `numpy`, `pandas`, `scikit-learn`, `lightgbm`, `HistGradientBoostingRegressor` 등
- 경로:  
  - `mercari_data/train.tsv`  
  - `mercari_data/test.tsv`  
- 상수:  
  - `RANDOM_STATE = 42`  
  - `VALID_SIZE = 0.30`

---

### 2. 텍스트 정규화
- `normalize_text`:  
  - 소문자화 → Unicode NFKD 정규화(악센트 제거) → 특수문자/이모지/비영문 제거 → 공백 정리
- 목적: 잡음 많은 상품명/설명을 기계가 읽기 쉬운 형태로 세척

---

### 3. 데이터 로딩 & 클린업
- TSV 로딩 → 결측 처리:
  - `category_name` 결측 제거
  - `item_description` → `"No description yet"`
- `category_name` → `cat1`, `cat2`, `cat3` 분리
- `brand_name`: 문자열 정리만 우선
- 타깃: `y = log1p(price.clip(lower=1))`

---

### 4. 이상치 처리
- **IQR 기준 제거**: `cat2` 그룹별로 IQR 범위를 벗어난 `price` 제거  
- 대안: winsorize(상하위 분위수 클리핑)

---

### 5. 브랜드 보강
- 상위 빈도 브랜드 사전 구축
- 상품명/설명 내 정규식 매칭으로 브랜드 추출 → `brand_name_filled`
- 아이디어:  
  - 액세서리 키워드(case, charger, cable…)  
  - `"for/compatible with"` 패턴 구분  
- (실제 코드 일부는 주석/스켈레톤 상태 → 필요 시 단순 `fillna('no_brand')` 적용)

---

### 6. 수치 신호 추출
- 상품 상태(`item_condition_id`), 배송 여부(`shipping`) 등 수치 피처 구성
- 추후 타깃 인코딩(TE), 등급화(5분위)와 결합

---

### 7. 스무딩 타깃 인코딩 (TE) & 등급화
- **스무딩 TE**: 그룹별 평균과 전체 평균을 빈도 기반으로 혼합 → 과적합 방지  
- **5분위 등급화**: 스무딩 값 분포를 1~5 등급으로 변환

---

### 8. 텍스트 피처 (TF-IDF → SVD)
- 텍스트: `name + item_description`
- 벡터화:  
  - `TfidfVectorizer(max_features=20000, ngram_range=(1,3), min_df=3, stop_words="english")`
- 차원 축소:  
  - `TruncatedSVD(n_components=256)`
- 데이터 분리 원칙:  
  - `train`에만 `fit`, `valid`는 `transform`만 (데이터 누수 방지)

---

## 모델 학습 & 검증

- **주 모델**: `LightGBM (LGBMRegressor)`  
  - 예: `learning_rate=0.05, num_leaves=64, feature_fraction=0.8 ...`
- **평가 지표**:  
  - RMSE / MAE (로그 스케일 & 실제 가격 스케일)
- **검증 과정**:  
  - 잔차 히스토그램, QQ Plot 시각화  
  - 피처 중요도: 상위 30개 바 차트 (SVD 축 + TE 피처 혼합 확인)

---

## 테스트셋 추론 & 배포
- 동일 파이프라인을 `test.tsv`에 적용 → 예측 산출
- `joblib.dump`로 모델/전처기 저장 → `./artifacts` 디렉토리

---

## ⚠실행 팁

- **경로 수정**:  
  - `TRAIN_PATH='mercari_data/train.tsv'`  
  - 본인 환경에 맞게 수정 (예: `/mnt/data/train.tsv`)
- **LightGBM 설치 필요**:  
  ```bash
  pip install lightgbm

## 포인트
### 왜 로그 타깃?
- 가격 분포의 긴 꼬리를 줄여 안정화 & 오차 해석 용이

### 왜 SVD?
- 수만 차원 TF-IDF를 200~300차원으로 압축 → 속도/성능 균형

### TE 과적합 방지?
- k-스무딩 + 저빈도 그룹 가중 증가 + (옵션) K-fold TE

### 이상치 처리 효과?
- 극단 가격 영향 억제, RMSE 개선

### 환경 문제 발생 시?
- 경로 수정 + LightGBM 설치 확인
