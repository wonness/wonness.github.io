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

    ![image](https://github.com/wonness/wonness.github.io/assets/141399098/1949ca35-456a-47a9-88c4-f9626fb37023){: width="70%" height="70%"}


- 매개변수 활용
    - 목표 매출 조정
        - <span style="background-color:#f5f0ff">목표 매출 금액</span>(매개변수)을 유동적으로 조정하고 달성 여부를 색상으로 구분하는 문제
        - 매개변수 생성 시, 허용 가능한 값을 범위로 설정 ➡️ 계산된 필드 생성 `SUM([매출]) > [매개변수명]`
          <details>
          <summary>결과 📊</summary>
          <div markdown="1">

          ![image](https://github.com/wonness/wonness.github.io/assets/141399098/ea6fe299-67e1-4f03-9020-433707b410bd)

          </div>
          </details>
            
    - Top N
        - 매출 <span style="background-color:#f5f0ff">상위 N개의 제품들</span>(매개변수)만 출력하는 문제
        - 제품명에 필터 적용 ➡️ 상위 [매개변수명] 기준 매출 합계 적용
          <details>
          <summary>결과 📊</summary>
          <div markdown="1">

          ![image](https://github.com/wonness/wonness.github.io/assets/141399098/28f7949c-f022-4ba0-befc-d8702ee69fa7)


          </div>
          </details>
            
    - 측정값 변경
        - 매개변수명에 따라 다른 <span style="background-color:#f5f0ff">측정값</span>(매개변수)을 불러오는 문제
        - 매개변수 생성 시, 허용 가능한 값을 목록으로 설정 ➡️ 계산된 필드에서 IF문을 활용하여 [매개변수명]에 따라 불러올 측정값 설정
          <details>
          <summary>결과 📊</summary>
          <div markdown="1">

          ![image](https://github.com/wonness/wonness.github.io/assets/141399098/b53b7bd4-012b-44b4-bb95-7be78df1aa13)



          </div>
          </details>

<br>

### ✏️ 대시보드 동작

필터 또는 하이라이트를 추가하여 사용자와 상호작용할 수 있도록 시각화
![image](https://github.com/wonness/wonness.github.io/assets/141399098/e88ba83a-33b5-4942-9126-d12482c00514){: width="70%" height="70%"}


- [대시보드] > [동작]의 "필터" 기능 = 대시보드에 포함된 시트에서의 "필터로 사용" 기능
: 해당 시트에 있는 값들이 다른 시트에 영향을 주어 필터링됨
- [대시보드] > [동작]의 "하이라이트” 기능
: 해당 시트에 있는 값들이 다른 시트에 영향을 주어 하이라이트됨

<br>

## 🏁 3일차 과제

> **매개 변수**와 **대시보드 작업(동작)**을 사용하여 사용자와 **상호작용** 할 수 있는 시각화 & 대시보드 제작

### 1️⃣ **매개 변수를 사용하여 측정값 변경하기 1 & 마크 색상 표현하기**
