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
우유와 요거트를 동시에 구입한 장바구니의 아이디를 조회하시오.\
(장바구니 아이디 기준 오름차순 정렬)\
<span style="background-color:#f5f0ff">\#SELF JOIN #WHERE #조합</span>

<br>

💻 **문제 풀이**

```sql
-- 1st 시도
SELECT DISTINCT A.CART_ID
FROM CART_PRODUCTS A JOIN CART_PRODUCTS B
ON A.CART_ID = B.CART_ID
WHERE A.NAME IN ("Milk", "Yogurt") AND B.NAME IN ("Milk", "Yogurt")
```
<span style="font-size: 12px ;> 결과 : 우유와 우유, 요거트와 요거트가 있는 행을 걸러내는 데 실패

<br>

```sql
-- 2nd 시도
SELECT DISTINCT A.CART_ID
FROM CART_PRODUCTS A JOIN CART_PRODUCTS B
ON A.CART_ID = B.CART_ID
WHERE A.NAME = "Milk" AND B.NAME = "Yogurt"
```
###### 한 명이 우유와 요거트를 장바구니에 담아놨을 때 셀프조인을 하면, milk-yogurt, yogurt-milk ... 등등의 조합들이 출력
###### 즉, 조합이 순서만 바뀐 채로 중복되어서 출력되기 때문에, milk-yogurt or yogurt-milk 중 하나의 조합을 조건으로 지정해주면 된다.

<br>

1. `FROM` : 자기 자신의 테이블을 조인시켜, 한 장바구니에 담아놓은 상품들의 조합을 불러온다. 
2. `WHERE` : 장바구니에 우유와 요거트가 동시에 담긴 데이터를 필터링한다. 

<br>

💡 **새로 배운 것**\
  ![image](https://github.com/wonness/wonness.github.io/assets/141399098/63015b34-50b3-40d4-8769-821378610f3e)
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

💻 **문제 풀이**

```sql
-- 1st 시도
SELECT C.AUTHOR_ID, AUTHOR_NAME, CATEGORY, (SALES*PRICE) AS TOTAL_SALES
FROM BOOK_SALES A JOIN BOOK B JOIN AUTHOR C
ON A.BOOK_ID = B.BOOK_ID AND B.AUTHOR_ID = C.AUTHOR_ID
WHERE DATE_FORMAT(SALES_DATE,"%Y-%m") = "2022-01"
GROUP BY C.AUTHOR_ID, CATEGORY
ORDER BY C.AUTHOR_ID, CATEGORY DESC
```
결과 : GROUP BY를 해줘서, `SALES*PRICE`는 첫 행에서 계산된 것을 출력함\ 
-> 집계함수 필요

<br>

```sql
-- 2nd 시도
SELECT C.AUTHOR_ID, AUTHOR_NAME, CATEGORY, SUM(TOTAL_SALES) AS TOTAL_SALES
FROM(SELECT *, (SALES*PRICE) AS TOTAL_SALES
    FROM BOOK_SALES A JOIN BOOK B JOIN AUTHOR C
    ON A.BOOK_ID = B.BOOK_ID AND B.AUTHOR_ID = C.AUTHOR_ID
    WHERE DATE_FORMAT(SALES_DATE,"%Y-%m") = "2022-01") G
GROUP BY C.AUTHOR_ID, CATEGORY
ORDER BY C.AUTHOR_ID, CATEGORY DESC
```
서브쿼리 테이블 : WHERE절까지 조건을 준 테이블에 `SALES X PRICE`를 계산\
기본 테이블 : GROUP BY, ORDER BY를 하고 `SUM(SALES X PRICE)`를 계산\
GROUP BY를 하면, `SALES X PRICE`가 행별로 계산이 안될 것이라 생각하여 위와 같은 쿼리를 짬\
결과 : 컬럼명 중복으로 실패

<br>

```sql
-- 3rd 시도
SELECT C.AUTHOR_ID, AUTHOR_NAME, CATEGORY, SUM(SALES*PRICE) AS TOTAL_SALES
FROM BOOK_SALES A JOIN BOOK B JOIN AUTHOR C
ON A.BOOK_ID = B.BOOK_ID AND B.AUTHOR_ID = C.AUTHOR_ID
WHERE DATE_FORMAT(SALES_DATE,"%Y-%m") = "2022-01"
GROUP BY C.AUTHOR_ID, CATEGORY
ORDER BY C.AUTHOR_ID, CATEGORY DESC
```
FROM절에 서브쿼리를 넣는 방법은 잘못됐다고 생각하여, 1st 쿼리에 `SUM(SALES X PRICE)`만 수정함\
결과 : 집계함수를 사용하니, 그룹별 `SALES X PRICE` 계산이 가능

<br>

1. `FROM` : `BOOK_SALES` 테이블, `BOOK` 테이블, `AUTHOR` 테이블을 INNER JOIN한다.
2. `WHERE` : 2022년 1월 판매한 도서 판매 데이터를 필터링한다. 
3. `GROUP BY` : 데이터를 작가ID, 카테고리 별로 그룹화한다.
4. `SELECT` : 그룹화한 작가ID, 카테고리와 작가 이름을 출력하고, SUM 집계함수를 사용하여 작가별 카테고리별 TOTAL_SALES를 출력한다. 

<br>

💡 **새로 배운 것**\
  GROUP BY로 그룹화하고 그 그룹에 속하는 여러 데이터를 집계하고 출력하려면, 집계함수를 사용해야 한다 !

<br>

## 🏁 그룹 별 조건에 맞는 식당 목록 출력하기

|테이블|내용|
|:---|:---|
|MEMBER_PROFILE|고객의 정보|
|REST_REVIEW|식당의 정보|

`MEMBER_PROFILE`와 `REST_REVIEW` 테이블에서 리뷰를 가장 많이 작성한 회원의 리뷰들을 조회하시오.\
(리뷰 작성일 기준 오름차순 정렬, 리뷰 작성일이 같다면 리뷰 텍스트 기준 오름차순 정렬)\
<span style="background-color:#f5f0ff">\#WHERE #Sub-Query #ORDER BY </span>

<br>

💻 **문제 풀이**

```sql
-- 1st 시도
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
LIMIT을 사용해서 리뷰를 가장 많이 남긴 회원을 찾으려 함\
결과 : ORDER BY절에서 지정한 집계함수가 SELECT 절에 없어서 오류 발생

<br>

```sql
-- 2nd 시도
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
리뷰 수로 필터링 하기 위해, WHERE절 서브쿼리의 HAVING절에 또 서브쿼리를 추가

<br>

1. `FROM` : `MEMBER_PROFILE`과 `REST_REVIEW` 테이블을 INNER JOIN한다.
2. `WHERE` : `MEMBER_PROFILE`과 `REST_REVIEW` 테이블을 INNER JOIN한 테이블에서 가장 높은 리뷰 수로 데이터를 필터링 한다. 그리고, 그 리뷰 수를 가진 회원을 조회하여 필터링한다. 

<br>

💡 **새로 배운 것**\
  ORDER BY에서 정렬 지정한 컬럼은 SELECT절에서도 지정해 주어야 한다.
  
<br>

## 🏁 상품을 구매한 회원 비율 구하기

|테이블|내용|
|:---|:---|
|USER_INFO|의류 쇼핑몰에 가입한 회원 정보|
|ONLINE_INFO|온라인 상품 판매 정보|

USER_INFO 테이블과 ONLINE_SALE 테이블에서 2021년에 가입한 전체 회원들 중 상품을 구매한 회원수와 상품을 구매한 회원의 비율(=2021년에 가입한 회원 중 상품을 구매한 회원수 / 2021년에 가입한 전체 회원 수)을 년, 월 별로 출력하시오.\
(상품을 구매한 회원의 비율은 소수점 두번째자리에서 반올림, 년 기준 오름차순 정렬, 년이 같다면 월 기준오름차순 정렬)\
<span style="background-color:#f5f0ff">\#JOIN #Sub-Query</span>

<br>

💻 **문제 풀이**

```sql
-- 1st 시도
SELECT YEAR(SALES_DATE) AS YEAR, MONTH(SALES_DATE) AS MONTH,
    COUNT(DISTINCT B.USER_ID) AS PUCHASED_USERS,
		(SELECT COUNT(DISTINCT B.USER_ID) / COUNT(DISTINCT A.USER_ID)
		FROM USER_INFO A LEFT JOIN ONLINE_SALE B
		ON A.USER_ID = B.USER_ID
		WHERE YEAR(JOINED) = "2021"
		GROUP BY YEAR, MONTH) AS PUCHASED_RATIO
FROM USER_INFO A LEFT JOIN ONLINE_SALE B
ON A.USER_ID = B.USER_ID
WHERE YEAR(JOINED) = "2021"
GROUP BY YEAR, MONTH
ORDER BY YEAR, MONTH
```
판매 년도, 월에 따라 출력해야 하므로, GROUP BY문에 년,도를 지정\
결과 : 그룹핑때문에 2021년도에 가입한 회원들(A.USER_ID)이 COUNT에 잡히지 않고, 서브쿼리절이 밖의 쿼리와 다를 게 없음 -> 실패

<br>

```sql
-- 2nd 시도
SELECT YEAR(SALES_DATE) AS YEAR, MONTH(SALES_DATE) AS MONTH,
    COUNT(DISTINCT B.USER_ID) AS PUCHASED_USERS,
		ROUND(COUNT(DISTINCT B.USER_ID) / (SELECT COUNT(DISTINCT USER_ID) 
						FROM USER_INFO WHERE YEAR(JOINED) = "2021"), 1) AS PUCHASED_RATIO
FROM USER_INFO A LEFT JOIN ONLINE_SALE B
ON A.USER_ID = B.USER_ID
WHERE YEAR(JOINED) = "2021" AND SALES_DATE IS NOT NULL
GROUP BY YEAR, MONTH
ORDER BY YEAR, MONTH
```
비율을 계산할 때, 분모인 2021년에 가입한 전체 회원 수를 구하려면 서브쿼리를 만들어야 함 (GROUP BY로 상품 구매 년, 월을 그룹핑했기 때문)\
2021년도에 가입한 유저 수는 변동이 없기 때문에 FROM, WHERE절로만 구성해서 단일값 출력 

<br>

1. `FROM` : 회원가입을 하고 구매하지 않은 회원이 있을 것이라고 판단하여, `USER_INFO`와 `ONLINE_SALE`테이블을 LEFT JOIN했다. 
2. `WHERE` : 2021년에 가입한 회원 데이터를 필터링한다. 그리고, LEFT JOIN을 하여 값이 비어있는 `ONLINE_SALE` 테이블의 NULL값을 처리한다.
3. `GROUP BY` : 판매 년, 월 별로 그룹화한다.
4. `SELECT` : 판매 년, 월, 구매 회원 수를 출력한다. 그리고, (2021년 가입한 회원 중 구매 회원 수 / 2021년 가입한 전체 회원 수)를 구하기 위해, 분모에 가입 회원 정보를 담은 테이블에서 가입일이 2021년인 데이터를 필터링하고, 회원 수를 출력한다.

<br>
