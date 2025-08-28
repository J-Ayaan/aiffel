# Main Quest 03
# Ecommerce Data Segmentation Project

## 데이터 소개
본 프로젝트에서 사용한 데이터는 [Kaggle - Ecommerce Data](https://www.kaggle.com/datasets/carrie1/ecommerce-data/data)  
에서 제공된 **UK 온라인 리테일 거래 데이터셋**입니다.

- **데이터 기간**: 2010-12-01 ~ 2011-12-09  
- **데이터 출처**: 영국 기반 온라인 소매업체(주로 선물용 상품 판매)  
- **데이터 용량**: 약 50만 건의 트랜잭션 로그  
- **파일 형식**: CSV (`data.csv`)  

---

## 데이터 컬럼 설명
| 컬럼명       | 설명 |
|--------------|------|
| **InvoiceNo** | 고유 거래 번호. `C`로 시작하면 취소(Refund)를 의미 |
| **StockCode** | 제품 코드 |
| **Description** | 제품 설명 |
| **Quantity** | 주문 수량 (음수일 경우 취소된 수량) |
| **InvoiceDate** | 거래 발생 시각 (YYYY-MM-DD HH:MM:SS) |
| **UnitPrice** | 단가 (GBP, 영국 파운드 기준) |
| **CustomerID** | 고객 식별자 (익명 처리, 일부 Null 존재) |
| **Country** | 고객의 국가 |

---

## 분석 목적
- 고객을 **세그멘테이션(Segmentation)** 하여 마케팅 전략 수립에 활용  
- **RFM 분석 (Recency, Frequency, Monetary)** 을 기반으로 고객 군집화  
- K-Means, Hierarchical Clustering 등 **비지도 학습 모델**을 통한 고객 세분화  
- 국가별/고객군별 구매 패턴 파악 및 시각화  
