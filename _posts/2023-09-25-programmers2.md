---
title: "[Programmers] SQL 난이도4,5 문제 풀이"
excerpt: ""

categories:
  - SQL
tags:
  - [sql, Programmers, MySQL]

permalink: /SQL/programmers2/

toc: true
toc_sticky: true

date: 2023-09-25
last_modified_at: 2023-09-25
---
안녕하세요!\
Programmers의 SQL 난이도 4,5 문제를 풀어보았습니다.\
제가 헷갈렸던 문제들을 정리해두었으니 참고 부탁드립니다 😄

--------------------
## 🏁 우유와 요거트가 담긴 장바구니
우유와 요거트를 동시에 구입한 장바구니의 아이디를 조회하시오. (장바구니 아이디 기준 오름차순 정렬)\
<span style="background-color:#f5f0ff">\#SELF JOIN #WHERE #조합</span>

<br>

- **문제 풀이**

```sql
/* 1st 시도
우유와 우유, 요거트와 요거트가 있는 행을 걸러내는 데 실패 */
SELECT DISTINCT A.CART_ID
FROM CART_PRODUCTS A JOIN CART_PRODUCTS B
ON A.CART_ID = B.CART_ID
WHERE A.NAME IN ("Milk", "Yogurt") AND B.NAME IN ("Milk", "Yogurt")
```

```sql
/* 2nd 시도
한 명이 우유와 요거트를 장바구니에 담아놨을 때 셀프조인을 하면, milk-yogurt, yogurt-milk ... 등등의 조합들이 출력
즉, 조합이 순서만 바뀐 채로 중복되어서 출력되기 때문에, milk-yogurt or yogurt-milk 중 하나의 조합을 조건으로 지정해주면 된다. */
SELECT DISTINCT A.CART_ID
FROM CART_PRODUCTS A JOIN CART_PRODUCTS B
ON A.CART_ID = B.CART_ID
WHERE A.NAME = "Milk" AND B.NAME = "Yogurt"
```

1. `FROM` : 자기 자신의 테이블을 조인시켜, 한 장바구니에 담아놓은 상품들의 조합을 불러온다. 
2. `WHERE` : 장바구니에 우유와 요거트가 동시에 담긴 데이터를 필터링한다. 

<br>

- **새로 배운 것**
  - SELF JOIN : 조합 생성에 유용\
    SELF JOIN은 순서가 바뀐 데이터가 중복되어 출력된다는 것을 간과하지 않기 !

<br>

## 🏁 저자 별 카테고리 별 매출액 집계하기

|테이블|내용|
|:---|:---|
|BOOK|각 도서의 정보|
|AUTHOR|도서의 저자의 정보|
|BOOK_SALES|각 도서의 날짜 별 판매량 정보|

2022년 1월의 도서 판매 데이터를 기준으로 저자 별, 카테고리 별 매출액(TOTAL_SALES = 판매량 * 판매가) 을 구하여, 저자 ID, 저자명, 카테고리, 매출액 리스트를 출력하시오.\
(저자 ID 기준 오름차순 정렬, 저자 ID가 같다면 카테고리 기준 내림차순 정렬)\
<span style="background-color:#f5f0ff">\#GROUP BY #집계함수 </span>

<br>

- **문제 풀이**

```sql
/* 1st 시도
결과 : GROUP BY를 해줘서, SALES*PRICE는 첫 행에서 계산된 것을 출력함 -> 집계함수 필요 */
SELECT C.AUTHOR_ID, AUTHOR_NAME, CATEGORY, (SALES*PRICE) AS TOTAL_SALES
FROM BOOK_SALES A JOIN BOOK B JOIN AUTHOR C
ON A.BOOK_ID = B.BOOK_ID AND B.AUTHOR_ID = C.AUTHOR_ID
WHERE DATE_FORMAT(SALES_DATE,"%Y-%m") = "2022-01"
GROUP BY C.AUTHOR_ID, CATEGORY
ORDER BY C.AUTHOR_ID, CATEGORY DESC
```

```sql
/* 2nd 시도
서브쿼리 테이블 : WHERE절까지 조건을 준 테이블에 SALES*PRICE를 계산
기본 테이블 : GROUP BY, ORDER BY를 하고 SUM(SALES*PRICE)를 계산
GROUP BY를 하면, SALES*PRICE가 행별로 계산이 안될 것이라 생각하여 아래의 쿼리를 짬
결과 : 컬럼명 중복으로 실패*/
SELECT C.AUTHOR_ID, AUTHOR_NAME, CATEGORY, SUM(TOTAL_SALES) AS TOTAL_SALES
FROM(SELECT *, (SALES*PRICE) AS TOTAL_SALES
    FROM BOOK_SALES A JOIN BOOK B JOIN AUTHOR C
    ON A.BOOK_ID = B.BOOK_ID AND B.AUTHOR_ID = C.AUTHOR_ID
    WHERE DATE_FORMAT(SALES_DATE,"%Y-%m") = "2022-01") G
GROUP BY C.AUTHOR_ID, CATEGORY
ORDER BY C.AUTHOR_ID, CATEGORY DESC
```

```sql
/* 3rd 시도
FROM절에 서브쿼리를 넣는 방법은 잘못됐다고 생각하여, 1st 쿼리에 SUM(SALES*PRICE)만 수정함
GROUP BY를 해도 행별로 SALES*PRICE계산이 가능했음 */
SELECT C.AUTHOR_ID, AUTHOR_NAME, CATEGORY, SUM(SALES*PRICE) AS TOTAL_SALES
FROM BOOK_SALES A JOIN BOOK B JOIN AUTHOR C
ON A.BOOK_ID = B.BOOK_ID AND B.AUTHOR_ID = C.AUTHOR_ID
WHERE DATE_FORMAT(SALES_DATE,"%Y-%m") = "2022-01"
GROUP BY C.AUTHOR_ID, CATEGORY
ORDER BY C.AUTHOR_ID, CATEGORY DESC
```

1. `FROM` : `BOOK_SALES` 테이블, `BOOK` 테이블, `AUTHOR` 테이블을 INNER JOIN한다.
2. `WHERE` : 2022년 1월 판매한 도서 판매 데이터를 필터링한다. 
3. `GROUP BY` : 데이터를 작가ID, 카테고리 별로 그룹화한다.
4. `SELECT` : 그룹화한 작가ID, 카테고리와 작가 이름을 출력하고, SUM 집계함수를 사용하여 작가별 카테고리별 TOTAL_SALES를 출력한다. 

<br>

- **새로 배운 것**
  - GROUP BY\
    그룹화를 하고 그 그룹에 속하는 여러 데이터를 집계하고 출력하려면, 집계함수를 사용해야 한다 !

<br>

## 🏁 그룹 별 조건에 맞는 식당 목록 출력하기

|테이블|내용|
|:---|:---|
|MEMBER_PROFILE|고객의 정보|
|REST_REVIEW|식당의 정보|

`MEMBER_PROFILE`와 `REST_REVIEW` 테이블에서 리뷰를 가장 많이 작성한 회원의 리뷰들을 조회하시오.\
(리뷰 작성일 기준 오름차순 정렬, 리뷰 작성일이 같다면 리뷰 텍스트 기준 오름차순 정렬)\
<span style="background-color:#f5f0ff">\# </span>

<br>

- **문제 풀이**

```sql
/* 1st 시도
리뷰를 가장 많이 남긴 회원을 찾으려 함
결과 : WHERE절 서브쿼리에서 LIMIT을 못써서 인지, SELECT 절에 없는 집계함수가 ORDER BY절에 있어서 그런건지 모르겠지만 오류 발생 */
SELECT MEMBER_NAME, REVIEW_TEXT, 
    DATE_FORMAT(REVIEW_DATE,"%Y-%m-%d") AS REVIEW_DATE
FROM MEMBER_PROFILE A JOIN REST_REVIEW B
ON A.MEMBER_ID = B.MEMBER_ID
WHERE A.MEMBER_ID IN (SELECT A.MEMBER_ID
                      FROM MEMBER_PROFILE A JOIN REST_REVIEW B
                      ON A.MEMBER_ID = B.MEMBER_ID
                      GROUP BY A.MEMBER_ID
                      ORDER BY COUNT(*) DESC
                      LIMIT 1)
ORDER BY REVIEW_DATE, REVIEW_TEXT
```

```sql
/* 2nd 시도
리뷰 수로 필터링 하기 위해, where절 서브쿼리의 having절에 또 서브쿼리를 추가 */
SELECT MEMBER_NAME, REVIEW_TEXT, 
    DATE_FORMAT(REVIEW_DATE,"%Y-%m-%d") AS REVIEW_DATE
FROM MEMBER_PROFILE A JOIN REST_REVIEW B
ON A.MEMBER_ID = B.MEMBER_ID
WHERE A.MEMBER_ID IN (SELECT A.MEMBER_ID
                     FROM MEMBER_PROFILE A JOIN REST_REVIEW B
                      ON A.MEMBER_ID = B.MEMBER_ID
                     GROUP BY A.MEMBER_ID
                     HAVING COUNT(*) = (SELECT COUNT(*)
                                       FROM MEMBER_PROFILE A 
                                        JOIN REST_REVIEW B 
                                        ON A.MEMBER_ID = B.MEMBER_ID
                                       GROUP BY A.MEMBER_ID
                                       ORDER BY COUNT(*) DESC
                                       LIMIT 1))
ORDER BY REVIEW_DATE, REVIEW_TEXT
```

1. `FROM` :
2. `WHERE` :
3. `SELECT` :

<br>

- **새로 배운 것**


## 🏁 입양 시각 구하기 (2)



## 🏁 상품을 구매한 회원 비율 구하기


