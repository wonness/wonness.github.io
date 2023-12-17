---
title: "[태블로 신병훈련소] Day3 - 매개변수, 대시보드 작업"
excerpt: ""

categories:
  - Tableau
tags:
  - [Tableau, 태블로 신병훈련소, 시각화]

permalink: /Tableau/tableau-camp3/

toc: true
toc_sticky: true

date: 2023-12-17
last_modified_at: 2023-12-17
---

## 🏁 3일차

> 3일차 학습 안내\
매개변수 활용 & 대시보드 작업

<br>

### ✏️ 매개변수

- 계산, 필터 및 참조선에서 상수 값을 동적으로 바꿀 수 있는 변수
    
    ex) 카페인 용량에 따라 색상을 표현할 때, 80mg을 기준으로 계산식을 만들었는데 100mg을 기준으로 보고 싶다면 ? 
    → 계산식을 수정할 필요 없이 매개변수 활용 !
    

- 매개변수 생성
    
    데이터 유형(실수, 정수, 문자열, 날짜 등)과 허용 가능한 값 설정 방법(전체, 목록, 범위) 선택

    ![image](https://github.com/wonness/wonness.github.io/assets/141399098/1949ca35-456a-47a9-88c4-f9626fb37023)


- 매개변수 활용
    - 목표 매출 조정
        - 목표 매출 금액을 유동적으로 조정하고 달성 여부를 색상으로 구분하는 문제
        - 매개변수 : 목표 매출 금액
        - 매개변수 생성 시, 허용 가능한 값을 범위로 설정 ➡️ 계산된 필드 생성 `SUM([매출]) > [매개변수명]`
          <details>
          <summary>결과 📊</summary>
          <div markdown="1">

          ![image](https://github.com/wonness/wonness.github.io/assets/141399098/ea6fe299-67e1-4f03-9020-433707b410bd)

          </div>
          </details>
            
    - Top N
        - 매출 상위 N개의 제품들만 출력하는 문제
        - 매개변수 : 출력할 제품 수
        - 제품명에 필터 적용 ➡️ 상위 [매개변수명] 기준 매출 합계 적용
          <details>
          <summary>결과 📊</summary>
          <div markdown="1">

          ![image](https://github.com/wonness/wonness.github.io/assets/141399098/28f7949c-f022-4ba0-befc-d8702ee69fa7)


          </div>
          </details>
            
    - 측정값 변경
        - 매개변수명에 따라 다른 측정값을 불러오는 문제
        - 매개변수 : 측정값
        - 매개변수 생성 시, 허용 가능한 값을 목록으로 설정 ➡️ 계산된 필드에서 IF문을 활용하여 [매개변수명]에 따라 불러올 측정값 설정
          <details>
          <summary>결과 📊</summary>
          <div markdown="1">

          ![image](https://github.com/wonness/wonness.github.io/assets/141399098/b53b7bd4-012b-44b4-bb95-7be78df1aa13)



          </div>
          </details>
