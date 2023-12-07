---
title: "[태블로 신병훈련소] Day1 - 막대 차트, 트리맵, 산점도, 대시보드"
excerpt: ""

categories:
  - Tableau
tags:
  - [Tableau, 태블로 신병훈련소, 시각화]

permalink: /Tableau/tableau-camp1/

toc: true
toc_sticky: true

date: 2023-11-25
last_modified_at: 2023-11-25
---
> "스타벅스 메뉴 데이터"와 "매장 정보 데이터"를 이용한 시각화 & 대시보드 제작\
> ☕ 다이어트 중인데.. 칼로리와 카페인이 적은 음료는 뭘까?

<br>

## 🏁 막대 차트
- 값을 길이로 표현
- 수치 데이터 값들 간의 양적 차이를 비교하는데 유용
- 참조선(ex. 평균값, 중간값)을 표현해 특정 값에 도달했는지 비교할 수 있음
- 유의할 점) 비슷한 값들의 비교를 명확하게 하기 위해 데이터를 정렬해야 함

## 🏁 트리 맵
- 계층 구조의 데이터를 표시하는데 적합한 시각화
- 전체 대비 부분의 비율이 얼마나 되는지 비교하는데 많이 사용
- 사각형의 크기와 색상에 따라 데이터의 패턴을 확인할 수 있고, 데이터를 한 번에 볼 수 있음

## 🏁 산점도
- 2개의 연속형 데이터에 대한 상관관계 분석하는데 가장 많이 사용되는 시각화
- 두 개의 축으로 데이터가 얼마나 퍼져있는지 분포를 살펴볼 수 있음
- 상수 라인/ 평균 라인/ 사분위수 및 중앙값/ 추세선과 같은 참조 라인을 추가해서 값의 분포를 비교하기에 유용

<br>

# 1️⃣ 카테고리 별 평균 칼로리 & 평균 칼로리
![barchart](https://eseullee.github.io/assets/images/posts_img/tableau_bootcamp/day1/20230203_tableau_bootcamp_17_1_1.png)

1. 행 선반 (Y축) : 카테고리 / 열 선반 (X축) : 칼로리, 카페인
    - y축에 카테고리를 두고, 칼로리와 카페인을 병렬로 표현
2. 칼로리와 카페인의 **집계 형태를 평균**으로 변경
    - 하나의 카테고리 안에는 여러 개의 메뉴가 있음
    - 카테고리를 기준으로 합계 집계하면, 카테고리 안의 모든 메뉴의 칼로리와 카페인 값이 더해짐 → 비교 불가
3. 평균 카페인에 색상을 적용
    - 평균 카페인이 높을수록 붉은 색을 띠도록 시각화
4. 명확한 비교를 위해 평균 칼로리 기준으로 정렬

<br>

💡 시각화 결과
- 브루드 커피 : 평균 칼로리가 가장 낮지만 평균 카페인이 가장 높음
- 스타벅스피지오, 스타벅스주스(병음료), 티바나 : 상대적으로 평균 칼로리가 낮으면서 평균 카페인이 적은 것으로 보여짐

<br>

# 2️⃣ 메뉴명 별 칼로리 & 카페인
![treemap](https://eseullee.github.io/assets/images/posts_img/tableau_bootcamp/day1/20230203_tableau_bootcamp_17_1_2.png)

1. 마크 유형을 사각형으로 변경
2. 칼로리와 카페인의 **집계 형태를 합계**로 변경
    - 메뉴명은 유일하게 구분되고 중복되지 않는 값\
    (하나의 메뉴명에는 하나의 칼로리, 하나의 카페인 값이 존재)
    - 평균과 합계가 동일함
3. 합계 칼로리에 크기를 적용
    - 칼로리에 따라 크기가 달라지도록 시각화
5. 합계 카페인에 색상을 적용
    - 카페인이 높을수록 붉은 색을 띠도록 시각화
7. 메뉴명에 레이블을 적용
    - 사각형 안에 메뉴명이 보이도록 시각화

<br>

# 3️⃣ 카테고리와 메뉴명을 한 번에 살펴보기
![image](https://github.com/wonness/wonness.github.io/assets/141399098/9db8f998-d594-4c30-b22f-dd9db1665385)


# 4️⃣ 당분 함유량과 칼로리 상관관계
![scatterplot](https://eseullee.github.io/assets/images/posts_img/tableau_bootcamp/day1/20230203_tableau_bootcamp_17_1_5.png)

# 5️⃣ 시군구 별 매장 분포 현황
![map](https://eseullee.github.io/assets/images/posts_img/tableau_bootcamp/day1/20230203_tableau_bootcamp_17_1_6.png)

# 6️⃣ 대시보드 만들기
![dashboard](https://eseullee.github.io/assets/images/posts_img/tableau_bootcamp/day1/20230203_tableau_bootcamp_17_1_7.png)

