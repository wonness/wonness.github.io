---
title: "[Programmers] SQL 난이도1,2,3 문제 풀이"
excerpt: ""

categories:
  - SQL
tags:
  - [sql, Programmers, MySQL]

permalink: /SQL/programmers1/

toc: true
toc_sticky: true

date: 2023-09-20
last_modified_at: 2023-09-20
---
안녕하세요!\
Programmers의 SQL 난이도 1,2,3 문제를 풀어보았습니다.\
제가 헷갈렸던 문제들만 정리해두었으니 참고 부탁드립니다 😄

--------------------------
## 🏁 조건에 부합하는 중고거래 댓글 조회하기

|테이블|내용|
|:---|:---|
|USED_GOODS_BOARD|중고거래 게시판 정보|
|USED_GOODS_REPLY|중고거래 게시판 첨부파일 정보|

`USED_GOODS_BOARD`와 `USED_GOODS_REPLY` 테이블에서 2022년 10월에 작성된 게시글 제목, 게시글 ID, 댓글 ID, 댓글 작성자 ID, 댓글 내용, 댓글 작성일을 조회하시오.\
(댓글 작성일 기준 오름차순 정렬, 댓글 작성일이 같다면 게시글 제목 기준 오름차순 정렬)\
<span style="background-color:#f5f0ff">\#DATE #WHERE</span>

<br>

- **문제 풀이**
```sql
/* 1st 시도
between으로 날짜 범위 구하기 성공
but, date함수 사용 불가능 */
SELECT A.TITLE, A.BOARD_ID,
        B.REPLY_ID, B.WRITER_ID, B.CONTENTS, 
        DATE(B.CREATED_DATE) # MySQL 4.1.1 버전부터 사용 가능
FROM USED_GOODS_BOARD A RIGHT JOIN USED_GOODS_REPLY B
ON A.BOARD_ID = B.BOARD_ID
WHERE A.CREATED_DATE BETWEEN "2022-10-01" AND "2022-10-31"
ORDER BY B.CREATED_DATE, A.TITLE ;
```

```sql
/* 2nd 시도
DATE_FORMAT() 함수 사용해서 날짜 포매팅 */
SELECT A.TITLE, A.BOARD_ID,
        B.REPLY_ID, B.WRITER_ID, B.CONTENTS, 
        DATE_FORMAT(B.CREATED_DATE, "%Y-%m-%d") as CREATED_DATE
FROM USED_GOODS_BOARD AS A JOIN USED_GOODS_REPLY AS B
ON A.BOARD_ID = B.BOARD_ID
WHERE A.CREATED_DATE LIKE "2022-10%"
# WHERE A.CREATED_DATE BETWEEN "2022-10-01" AND "2022-10-31"
# WHERE DATE_FORMAT(A.CREATED_DATE, "%Y-%m") = "2022-10"
ORDER BY B.CREATED_DATE, TITLE
```

1. `FROM` : `USED_GOODS_BOARD` 테이블과 `USED_GOODS_REPLY` 테이블을 INNER JOIN 한다.
2. `WHERE` : 2022년 10월 작성된 게시글을 필터링하기 위해 `USED_GOODS_BOARD` 테이블의 CREATED_DATE 컬럼에 조건을 둔다.
3. `SELECT` : 게시글 제목, 게시글 ID, 댓글 ID, 댓글 작성자 ID, 댓글 내용, 댓글 작성일을 조회한다. 댓글 작성일은 연-월-일의 형식으로 출력되어야 하므로 DATE_FORAMT() 함수를 사용한다.

<br>

- **새로 배운 것**
  - DATE_FORAMT(날짜, "형식") : 날짜를 지정 형식으로 출력
    - 형식
      |기호|역할|기호|역할|
      |:---|:---|:---|:---|
      |%Y|4자리 연도|%y|2자리 연도|
      |%M|영어 월|%m|숫자 월|
      |%D|영어 일|%d|숫자 일|
      |%H|시간(24시간)|%h|시간(12시간)|
      |%i|분|%S|초|

<br>

## 🏁 자동차 대여 기록에서 대여중/ 대여 가능 여부 구분하기
`CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블에서 2022년 10월 16일에 대여 중인 자동차인 경우 '대여중' 이라고 표시하고, 대여 중이지 않은 자동차인 경우 '대여 가능'을 표시하는 컬럼(컬럼명: AVAILABILITY)을 추가하여 자동차 ID와 AVAILABILITY 리스트를 출력하시오.
(자동차 ID 기준 내림차순 정렬)\
<span style="background-color:#f5f0ff">\#CASE WHEN #Sub-Query</span>

<br>

- **문제 풀이**
```sql
SELECT CAR_ID,
    CASE WHEN CAR_ID IN 
        (SELECT CAR_ID FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
        WHERE "2022-10-16" BETWEEN START_DATE AND END_DATE)
        THEN "대여중" ELSE "대여 가능" END AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
ORDER BY CAR_ID DESC
```

1. `GROUP BY` : 데이터를 CAR_ID별로 그룹화한다.
2. `CASE WHEN` : 대여 시작 날짜와 반납 날짜 사이에 2022-10-16 날짜가 속해있는 경우 "대여중"을 표시, 그렇지 않은 경우 "대여 가능"을 표시하게 하고, 그 결과를 컬럼으로 정의한다.

- **새로 배운 것**
GROUP BY를 하면 하나의 CAR_ID에 속하는 데이터 하나만을 가지고 집계할 수 있다.\
예를 들어, CAR_ID = 3의 대여 기록이 여러 건 있다고 할 때, 첫번째 튜플만 집계에 고려한다.\
<span style="text-decoration: underline;">즉, 여러 건의 대여 기록에 "2022-10-16"이 속해있어도, 첫번째 튜플이 아니면 "대여 가능"을 출력한다.</span>\
→ 이 문제의 경우, SELECT 절에서 서브쿼리를 이용해 모든 데이터를 검색해서 집계할 수 있도록 한다.

<br>

## 🏁 오랜 기간 보호한 동물 (2)

|테이블|내용|
|:---|:---|
|ANIMAL_INS|동물 보호소에 들어온 동물의 정보|
|ANIMAL_OUTS|동물 보호소에서 입양 보낸 동물의 정보|

입양을 간 동물 중, 보호 기간이 가장 길었던 동물 두 마리의 아이디와 이름을 조회하시오. (보호 기간 기준 내림차순 정렬)\
<span style="background-color:#f5f0ff">\#LEFT JOIN #DATE</span>

<br>

- **문제 풀이**
```sql
SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_INS A LEFT JOIN ANIMAL_OUTS B
ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.ANIMAL_ID IN (SELECT B.ANIMAL_ID FROM ANIMAL_OUTS)
ORDER BY DATEDIFF(B.DATETIME, A.DATETIME) DESC
LIMIT 2
```

1. `FROM` : 보호소에 들어왔지만 아직 입양가지 못한 동물이 있을 것이라 생각해서 LEFT JOIN 했다. ("입양을 간 동물 중"이라고 범위를 한정지었으니, INNER JOIN하는게 더 효율적일 듯 하다.)
2. `WHERE` : 입양 보낸 동물과 보호소에 들어온 동물의 ID가 같은 데이터를 필터링한다.
3. `ORDER BY` : DATEDIFF()를 이용하여 보호 시작일과 입양일의 차이를 구하고, 보호기간을 내림차순으로 정렬한다.

<br>
