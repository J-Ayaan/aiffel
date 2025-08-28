# 고객을 세그먼테이션하자

# 11-2. 데이터 불러오기

## 데이터 살펴보기

---

- **테이블에 있는 10개의 행만 출력하기**
    
    ```sql
    SELECT *
    FROM long-ceiling-470102-p4.moulabs_project.data
    LIMIT 10
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오전 11.28.15.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.28.15.png)
    

- **전체 데이터는 몇 행으로 구성되어 있는지 확인하기**
    
    ```sql
    SELECT COUNT(*)
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오전 11.27.24.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.27.24.png)
    

## 데이터 수 세기

---

- **COUNT 함수를 사용해서, 각 컬럼별 데이터 포인트의 수를 세어 보기**
    
    ```sql
    SELECT
      COUNT(InvoiceNo) as Count_InvoiceNo,
      COUNT(StockCode) as Count_StockCode,
      COUNT(Description) as Count_Description,
      COUNT(Quantity) as Count_Quantity,
      COUNT(InvoiceDate) as Count_InvoiceDate,
      COUNT(UnitPrice) as Count_UnitPrice,
      COUNT(CustomerID) as Count_CustomerID,
      COUNT(Country) as Count_Country
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오전 11.26.45.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.26.45.png)
    

# 11-4. 데이터 전처리 방법(1): 결측치 제거

## 컬럼 별 누락된 값의 비율 계산

---

- **각 컬럼 별 누락된 값의 비율을 계산**
    - **각 컬럼에 대해서 누락 값을 계산한 후, 계산된 누락 값을 UNION ALL을 통해 합치기**
    
    ```sql
    -- 사용자 정의 함수 만들어 쓰는게 나을지도..?
    SELECT
        'InvoiceNo' AS column_name,
        ROUND(SUM(CASE WHEN InvoiceNo IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    UNION ALL
    SELECT
        'StockCode' AS column_name,
        ROUND(SUM(CASE WHEN StockCode IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    UNION ALL
    SELECT
        'Description' AS column_name,
        ROUND(SUM(CASE WHEN Description IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    UNION ALL
    SELECT
        'Quantity' AS column_name,
        ROUND(SUM(CASE WHEN Quantity IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    UNION ALL
    SELECT
        'InvoiceDate' AS column_name,
        ROUND(SUM(CASE WHEN InvoiceDate IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    UNION ALL
    SELECT
        'UnitPrice' AS column_name,
        ROUND(SUM(CASE WHEN UnitPrice IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    UNION ALL
    SELECT
        'CustomerID' AS column_name,
        ROUND(SUM(CASE WHEN CustomerID IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    UNION ALL
    SELECT
        'Country' AS column_name,
        ROUND(SUM(CASE WHEN Country IS NULL THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS missing_percentage
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오전 11.39.24.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.39.24.png)
    

## **결측치 처리 전략**

---

- **`StockCode = '85123A'`의 `Description`을 추출하는 쿼리문을 작성하기**
    
    ```sql
    SELECT DISTINCT Description
    FROM long-ceiling-470102-p4.moulabs_project.data
    WHERE StockCode = '85123A'
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오전 11.43.15.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.43.15.png)
    

## **결측치 처리**

---

- **DELETE 구문을 사용하며, WHERE 절을 통해 데이터를 제거할 조건을 제시**
    
    ```sql
    DELETE FROM long-ceiling-470102-p4.moulabs_project.data
    WHERE CustomerID is null OR Description ='?'
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오전 11.46.38.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.46.38.png)
    

# **11-5. 데이터 전처리(2): 중복값 처리**

## **중복값 확인**

---

- **중복된 행의 수를 세어보기**
    - **8개의 컬럼에 그룹 함수를 적용한 후, COUNT가 1보다 큰 데이터를 세어보기**
    
    ```sql
    SELECT *
    FROM long-ceiling-470102-p4.moulabs_project.data
    GROUP BY InvoiceNo,StockCode,Description,Quantity,UnitPrice,InvoiceDate,CustomerID,Country
    HAVING COUNT(*) > 1
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오전 11.56.50.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.56.50.png)
    

## **중복값 처리**

---

- **중복값을 제거하는 쿼리문 작성하기**
    - **CREATE OR REPLACE TABLE 구문을 활용하여 모든 컬럼(*)을 DISTINCT 한 데이터로 업데이트**
    
    ```sql
    CREATE OR REPLACE TABLE `long-ceiling-470102-p4.moulabs_project.data` AS
    SELECT DISTINCT *
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.01.05.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.01.05.png)
    
    ![스크린샷 2025-08-28 오후 12.01.21.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.01.21.png)
    

# **11-6. 데이터 전처리(3): 오류값 처리**

## **`InvoiceNo` 살펴보기**

---

- **고유(unique)한 `InvoiceNo`의 개수를 출력하기**
    
    ```sql
    SELECT COUNT(DISTINCT InvoiceNo)
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.04.09.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.04.09.png)
    
- **고유한 `InvoiceNo`를 앞에서부터 100개를 출력하기**
    
    ```sql
    SELECT InvoiceNo
    FROM long-ceiling-470102-p4.moulabs_project.data
    LIMIT 100
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.06.47.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.06.47.png)
    
- **`InvoiceNo`가 'C'로 시작하는 행을 필터링 할 수 있는 쿼리문을 작성하기 (100행까지만 출력)**
    
    ```sql
    SELECT *
    FROM long-ceiling-470102-p4.moulabs_project.data
    WHERE InvoiceNo LIKE 'C%'
    LIMIT 100
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.18.58.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.18.58.png)
    
- **구매 건 상태가 `Canceled` 인 데이터의 비율(%) - 소수점 첫번째 자리까지**
    
    ```sql
    SELECT ROUND((SUM(CASE WHEN InvoiceNo LIKE 'C%'THEN 1 ELSE 0 END) / COUNT(*) * 100),1)
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.34.50.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.34.50.png)
    

## **`StockCode` 살펴보기**

---

- **고유한 `StockCode`의 개수를 출력하기**
    
    ```sql
    SELECT COUNT(DISTINCT StockCode)
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.36.38.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.36.38.png)
    
- **어떤 제품이 가장 많이 판매되었는지 보기 위하여 `StockCode` 별 등장 빈도를 출력하기**
    - 상위 10개의 제품들을 출력하기
    
    ```sql
    SELECT StockCode, COUNT(*) AS sell_cnt 
    FROM long-ceiling-470102-p4.moulabs_project.data
    GROUP BY StockCode
    ORDER BY sell_cnt DESC
    LIMIT 10
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.38.16.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.38.16.png)
    
- **`StockCode`의 컬럼에 있던 값 중에서 숫자를 제외한 문자만 남기고 문자가 몇 자리 수 인지 세고**
    - **숫자가 0~1개인 값**들에는 어떤 코드들이 들어가 있는지 출력하기
    
    ```sql
    SELECT DISTINCT StockCode, number_count
    FROM (
      SELECT StockCode,
        LENGTH(StockCode) - LENGTH(REGEXP_REPLACE(StockCode, r'[0-9]', '')) AS number_count
      FROM long-ceiling-470102-p4.moulabs_project.data
    ) 
    WHERE number_count in (0,1)
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 12.45.26.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.45.26.png)
    
- **`StockCode`의 컬럼에 있던 값 중에서 숫자를 제외한 문자만 남기고 문자가 몇 자리 수 인지 세고**
    - **숫자가 0~1개인 값들을 가지고 있는 데이터 수는 전체 데이터 수 대비 몇 퍼센트**인지 구하기 (소수점 두 번째 자리까지)
    
    ```sql
    SELECT DISTINCT StockCode, number_count
    FROM (
      SELECT StockCode,
        LENGTH(StockCode) - LENGTH(REGEXP_REPLACE(StockCode, r'[0-9]', '')) AS number_count
      FROM project_name.modulabs_project.data
    ) 
    WHERE # [[YOUR QUERY]];
    ```
    
    0.48%
    
- **제품과 관련되지 않은 거래 기록을 제거하기**
    
    ```sql
    DELETE FROM `long-ceiling-470102-p4.moulabs_project.data`
    WHERE StockCode IN (
      SELECT DISTINCT StockCode
      FROM (
        SELECT
          StockCode,
          (LENGTH(StockCode) - LENGTH(REGEXP_REPLACE(StockCode, r'[0-9]', ''))) AS number_count
        FROM long-ceiling-470102-p4.moulabs_project.data
        WHERE StockCode IS NOT NULL
      )
      WHERE number_count IN (0, 1)
    );
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 2.37.30.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.37.30.png)
    

## **`Description` 살펴보기**

---

- **고유한 Description 별 출현 빈도를 계산하고 상위 30개를 출력하기**
    
    ```sql
    SELECT Description, COUNT(*) AS description_cnt
    FROM long-ceiling-470102-p4.moulabs_project.data
    GROUP BY Description
    ORDER BY COUNT(*) DESC
    LIMIT 30
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 2.41.39.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.41.39.png)
    
- **서비스 관련 정보를 포함하는 행들을 제거하기**
    
    ```sql
    DELETE
    FROM long-ceiling-470102-p4.moulabs_project.data
    WHERE Description IN ('Next Day Carriage','High Resolution Image')
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 2.45.44.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.45.44.png)
    
- **대소문자를 혼합하고 있는 데이터를 대문자로 표준화 하기**
    
    ```sql
    CREATE OR REPLACE TABLE long-ceiling-470102-p4.moulabs_project.data AS
    SELECT
      * EXCEPT (Description),
      UPPER(Description) AS Description 
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 2.51.29.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.51.29.png)
    

## **`UnitPrice` 살펴보기**

---

- **`UnitPrice`의 최솟값, 최댓값, 평균을 구하기**
    
    ```sql
    SELECT MIN(UnitPrice) AS min_price, MAX(UnitPrice) AS max_price, AVG(UnitPrice) AS avg_price
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 2.55.45.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.55.45.png)
    
- **단가가 0원인 거래의 개수, 구매 수량(`Quantity`)의 최솟값, 최댓값, 평균 구하기**
    
    ```sql
    SELECT COUNT(*) AS cnt_quantity, MIN(UnitPrice) AS min_price, MAX(UnitPrice) AS max_price, AVG(UnitPrice) AS avg_price
    FROM long-ceiling-470102-p4.moulabs_project.data
    WHERE UnitPrice = 0
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 2.58.40.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.58.40.png)
    
- **`UnitPrice = 0`를 제거하고 일관된 데이터셋을 유지하기**
    
    ```sql
    CREATE OR REPLACE TABLE long-ceiling-470102-p4.moulabs_project.data AS 
    SELECT *
    FROM long-ceiling-470102-p4.moulabs_project.data
    WHERE UnitPrice != 0
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.03.35.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.03.35.png)
    

# **11-7. RFM 스코어**

## **Recency**

---

- **`InvoiceDate` 컬럼을 연월일 자료형으로 변경하기**
    
    ```sql
    SELECT DATE(InvoiceDate) as InvoiceDay, *
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.07.53.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.07.53.png)
    
- **가장 최근 구매 일자를 MAX() 함수로 찾아보기**
    
    ```sql
    SELECT 
      MAX(DATE(InvoiceDate)) OVER() as most_recent_date,
      DATE(InvoiceDate) as InvoiceDay,
      *
    FROM long-ceiling-470102-p4.moulabs_project.data
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.15.01.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.15.01.png)
    
- **유저 별로 가장 큰 InvoiceDay를 찾아서 가장 최근 구매일로 저장하기**
    
    ```sql
    SELECT
      CustomerID,
      MAX(DATE(InvoiceDate)) as InvoiceDay
    FROM long-ceiling-470102-p4.moulabs_project.data
    GROUP BY CustomerID
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.19.15.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.19.15.png)
    
- **가장 최근 일자(`most_recent_date`)와 유저별 마지막 구매일(`InvoiceDay`)간의 차이를 계산하기**
    
    ```sql
    SELECT
      CustomerID, 
      EXTRACT(DAY FROM MAX(InvoiceDay) OVER () - InvoiceDay) AS recency
    FROM (
      SELECT 
        CustomerID,
        MAX(DATE(InvoiceDate)) AS InvoiceDay
      FROM project_name.modulabs_project.data
      GROUP BY CustomerID
    );
    
    -- 위에 마이너스 연산한 뒤에 EXTRACT를 사용하는 방식보다는 DATE_DFF(END, START, DAY)가 더 안정적일 듯?
    -- 물론 두 개의 연산 결과는 모두 같긴 하지만 확장성 및 안정성 고려했을 경우 DATE_DIFF 사용하는 걸로...
    WITH per_customer AS (
      SELECT 
        CustomerID,
        MAX(DATE(InvoiceDate)) AS InvoiceDay
      FROM long-ceiling-470102-p4.moulabs_project.data
      WHERE CustomerID IS NOT NULL
      GROUP BY CustomerID
    )
    SELECT
      CustomerID,
      DATE_DIFF( MAX(InvoiceDay) OVER (), InvoiceDay, DAY ) AS recency
    FROM per_customer
    ORDER BY CustomerID;
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.26.10.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.26.10.png)
    
- **최종 데이터 셋에 필요한 데이터들을 각각 정제해서 이어붙이고 지금까지의 결과를 `user_r`이라는 이름의 테이블로 저장하기**
    
    ```sql
    CREATE OR REPLACE TABLE long-ceiling-470102-p4.moulabs_project.user_r AS
    WITH per_customer AS (
      SELECT 
        CustomerID,
        MAX(DATE(InvoiceDate)) AS InvoiceDay
      FROM long-ceiling-470102-p4.moulabs_project.data
      WHERE CustomerID IS NOT NULL
      GROUP BY CustomerID
    )
    SELECT
      CustomerID,
      DATE_DIFF( MAX(InvoiceDay) OVER (), InvoiceDay, DAY ) AS recency
    FROM per_customer
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.31.14.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.31.14.png)
    

## **Frequency**

---

- **고객마다 고유한 InvoiceNo의 수를 세어보기**
    
    ```sql
    SELECT
      CustomerID,
      COUNT(DISTINCT InvoiceNo)
    FROM long-ceiling-470102-p4.moulabs_project.data
    GROUP BY CustomerID
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.33.47.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.33.47.png)
    
- **각 고객 별로 구매한 아이템의 총 수량 더하기**
    
    ```sql
    SELECT
      CustomerID,
      SUM(Quantity) as item_cnt
    FROM long-ceiling-470102-p4.moulabs_project.data
    GROUP BY CustomerID
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.36.54.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.36.54.png)
    
- **전체 거래 건수 계산와 구매한 아이템의 총 수량 계산의 결과를 합쳐서 `user_rf`라는 이름의 테이블에 저장하기**
    
    ```sql
    CREATE OR REPLACE TABLE long-ceiling-470102-p4.moulabs_project.user_rf AS
    -- (1) 전체 거래 건수 계산
    WITH purchase_cnt AS ( 
      SELECT
        CustomerID,
        COUNT(DISTINCT InvoiceNo) as purchase_cnt
      FROM long-ceiling-470102-p4.moulabs_project.data
      GROUP BY CustomerID
    ),
    
    -- (2) 구매한 아이템 총 수량 계산
    item_cnt AS (
      SELECT
        CustomerID,
        SUM(Quantity) as item_cnt
      FROM long-ceiling-470102-p4.moulabs_project.data
      GROUP BY CustomerID
    )
    
    -- 기존의 user_r에 (1)과 (2)를 통합
    SELECT
      pc.CustomerID,
      pc.purchase_cnt,
      ic.item_cnt,
      ur.recency
    FROM purchase_cnt AS pc
    JOIN item_cnt AS ic
      ON pc.CustomerID = ic.CustomerID
    JOIN long-ceiling-470102-p4.moulabs_project.user_r AS ur
      ON pc.CustomerID = ur.CustomerID;
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.43.04.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.43.04.png)
    
    ![스크린샷 2025-08-28 오후 3.43.42.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.43.42.png)
    

## **Monetary**

---

- **고객별 총 지출액 계산 (소수점 첫째 자리에서 반올림)**
    
    ```sql
    SELECT
      CustomerID,
      ROUND(SUM(Quantity * UnitPrice)) AS user_total
    FROM long-ceiling-470102-p4.moulabs_project.data
    GROUP BY CustomerID
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.53.57.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.53.57.png)
    
- **고객별 평균 거래 금액 계산**
    - **고객별 평균 거래 금액을 구하기 위해 1) `data` 테이블을 `user_rf` 테이블과 조인(LEFT JOIN) 한 후, 2) `purchase_cnt`로 나누어서 3) `user_rfm` 테이블로 저장하기**
    
    ```sql
    CREATE OR REPLACE TABLE long-ceiling-470102-p4.moulabs_project.user_rfm AS   
    SELECT
      rf.CustomerID AS CustomerID,
      rf.purchase_cnt,
      rf.item_cnt,
      rf.recency,
      ut.user_total,
      ROUND(ut.user_total / rf.purchase_cnt) AS user_average
    FROM long-ceiling-470102-p4.moulabs_project.user_rf rf
    LEFT JOIN (
      -- 고객 별 총 지출액
      SELECT
        CustomerID,
        ROUND(SUM(Quantity * UnitPrice)) AS user_total
      FROM long-ceiling-470102-p4.moulabs_project.data
      GROUP BY CustomerID
    ) ut
    ON rf.CustomerID = ut.CustomerID;
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.57.22.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.57.22.png)
    

## **RFM 통합 테이블 출력하기**

---

- **최종 user_rfm 테이블을 출력하기**
    
    ```sql
    SELECT *
    FROM long-ceiling-470102-p4.moulabs_project.user_rfm
    ```
    
    [결과 이미지를 넣어주세요]
    
    ![스크린샷 2025-08-28 오후 3.58.11.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.58.11.png)
    

# **11-8. 추가 Feature 추출**

## **1. 구매하는 제품의 다양성**

---

- **1) 고객 별로 구매한 상품들의 고유한 수를 계산하기 
2) `user_rfm` 테이블과 결과를 합치기 
3) `user_data`라는 이름의 테이블에 저장하기**

```sql
CREATE OR REPLACE TABLE long-ceiling-470102-p4.moulabs_project.user_data AS  
WITH unique_products AS (
  SELECT
    CustomerID,
    COUNT(DISTINCT StockCode) AS unique_products
  FROM long-ceiling-470102-p4.moulabs_project.data
  GROUP BY CustomerID
)
SELECT ur.*, up.* EXCEPT (CustomerID)
FROM long-ceiling-470102-p4.moulabs_project.user_rfm AS ur
JOIN unique_products AS up
ON ur.CustomerID = up.CustomerID;
```

[결과 이미지를 넣어주세요]

![스크린샷 2025-08-28 오후 4.14.09.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.14.09.png)

![스크린샷 2025-08-28 오후 4.14.41.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.14.41.png)

## **2. 평균 구매 주기**

---

- **고객들의 쇼핑 패턴을 이해하는 것을 목표 (고객 별 재방문 주기 살펴보기)**
    - **균 구매 소요 일수를 계산하고, 그 결과를 `user_data`에 통합**

```sql
CREATE OR REPLACE TABLE long-ceiling-470102-p4.moulabs_project.user_data AS 
WITH purchase_intervals AS (
  -- (2) 고객 별 구매와 구매 사이의 평균 소요 일수
  SELECT
    CustomerID,
    CASE WHEN ROUND(AVG(interval_), 2) IS NULL THEN 0 ELSE ROUND(AVG(interval_), 2) END AS average_interval
  FROM (
    -- (1) 구매와 구매 사이에 소요된 일수
    SELECT
      CustomerID,
      DATE_DIFF(InvoiceDate, LAG(InvoiceDate) OVER (PARTITION BY CustomerID ORDER BY InvoiceDate), DAY) AS interval_
    FROM
      long-ceiling-470102-p4.moulabs_project.data
    WHERE CustomerID IS NOT NULL
  )
  GROUP BY CustomerID
)

SELECT u.*, pi.* EXCEPT (CustomerID)
FROM long-ceiling-470102-p4.moulabs_project.user_data AS u
LEFT JOIN purchase_intervals AS pi
ON u.CustomerID = pi.CustomerID;
```

[결과 이미지를 넣어주세요]

![스크린샷 2025-08-28 오후 4.17.07.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.17.07.png)

![스크린샷 2025-08-28 오후 4.17.53.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.17.53.png)

## **3. 구매 취소 경향성**

---

- **고객의 취소 패턴 파악하기
1) 취소 빈도(cancel_frequency) : 고객 별로 취소한 거래의 총 횟수
2) 취소 비율(cancel_rate) :  각 고객이 한 모든 거래 중에서 취소를 한 거래의 비율**
    - **취소 빈도와 취소 비율을 계산하고 그 결과를 `user_data`에 통합하기 
    (취소 비율은 소수점 두번째 자리)**

```sql
CREATE OR REPLACE TABLE project_name.modulabs_project.user_data AS

WITH TransactionInfo AS (
  SELECT
    CustomerID,
    # [[YOUR QUERY]] AS total_transactions,
    # [[YOUR QUERY]] AS cancel_frequency
  FROM project_name.modulabs_project.data
  # [[YOUR QUERY]]
)

SELECT u.*, t.* EXCEPT(CustomerID), # [[YOUR QUERY]] AS cancel_rate
FROM `project_name.modulabs_project.user_data` AS u
LEFT JOIN TransactionInfo AS t
ON # [[YOUR QUERY]];
```

[결과 이미지를 넣어주세요]

![스크린샷 2025-08-28 오후 4.26.23.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.26.23.png)

- **다양한 컬럼들을 활용하여 고객의 구매 패턴과 선호도를 보다 심층적으로 이해할 수 있도록 최종적으로 `user_data`를 출력하기**

```sql
SELECT *
FROM long-ceiling-470102-p4.moulabs_project.user_data
```

[결과 이미지를 넣어주세요]

![스크린샷 2025-08-28 오후 4.27.47.png](%EA%B3%A0%EA%B0%9D%EC%9D%84%20%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%85%8C%EC%9D%B4%EC%85%98%ED%95%98%EC%9E%90%2025d11c0ac54d804c97b6f9b0e6eb5fcf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.27.47.png)

# 회고

[회고 내용을 작성해주세요]

Keep : 마케팅 측면에서 실무에 가까운 데이터 처리과정을 경험할 수 있어서 재밌었음

Problem : 

- MySQL에 익숙하여 BigQuery와의 문법차이 때문에 조금 헥갈렸음.
- project_name.dataset_name.table_name으로 데이터 테이블을 불러오는 방식이 번거로움

Try : 

- MySQL과 BigQuery 간의 문법 차이를 비교 정리
- 아래 방법을 활용 예정

### 1) **기본 프로젝트 / 기본 데이터셋 설정하기**

- **쿼리 에디터 상단**에서 “기본 프로젝트”를 지정하면 `project_name`은 생략 가능
- **쿼리 실행 옵션 → 기본 데이터셋(default dataset)** 을 지정하면 `dataset_name`도 생략 가능

```sql
SELECT *
FROM table_name
```

---

### 2) **세션 단위 기본 데이터셋 설정 (`SET` 구문)**

BigQuery 콘솔 또는 쿼리에서:

```sql
-- 세션에 기본 데이터셋 설정
SET @@dataset_id = 'dataset_name';
```

→ 이후 쿼리에서는 `dataset_name` 생략 가능:

```sql
SELECT * FROM table_name;
```

---

### 3) **뷰(View) 또는 임시 뷰 생성**

자주 쓰는 긴 경로를 뷰로 감싸두고 짧게 불러오기:

```sql
CREATE OR REPLACE VIEW my_dataset.my_table_alias AS
SELECT * FROM `project_name.dataset_name.table_name`;
```

→ 사용 시:

```sql
SELECT * FROM my_dataset.my_table_alias;
```

---

### 4) **외부 도구/IDE 활용**

- **dbt, Looker, Dataform** 등에서는 프로젝트/데이터셋을 기본값으로 설정 가능
- **Python(BigQuery client)** 에서 `default_dataset` 옵션 지정 가능

---

정리:

- **빠른 실험/쿼리** → 기본 데이터셋 지정 (쿼리 에디터 설정 or `SET @@dataset_id`)
- **재사용/협업** → View 만들어두고 alias처럼 사용
