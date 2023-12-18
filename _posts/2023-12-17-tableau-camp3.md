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

> 3일차 학습 내용\
매개변수 활용 & 대시보드 작업

<br>

### ✏️ 매개변수

- <span style="background-color:#f5f0ff">계산, 필터 및 참조선에서 상수 값을 동적으로 바꿀 수 있는 변수</span>
    
    ex) 카페인 용량에 따라 색상을 표현할 때, 80mg을 기준으로 계산식을 만들었는데 100mg을 기준으로 보고 싶다면 ? 
    → 계산식을 수정할 필요 없이 매개변수 활용 !

  
- 매개변수는 just 변수일 뿐 <span style="text-decoration: underline;">계산, 필터 및 참조선을 적용해야 제 기능을 함</span>
    

- **매개변수 생성**
    
    데이터 유형(실수, 정수, 문자열, 날짜 등)과 허용 가능한 값 설정 방법(전체, 목록, 범위) 선택

    ![image](https://github.com/wonness/wonness.github.io/assets/141399098/1949ca35-456a-47a9-88c4-f9626fb37023){: width="70%" height="70%"}


- **매개변수 활용**
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

필터 또는 하이라이트를 추가하여 **사용자와 상호작용**할 수 있도록 시각화
![image](https://github.com/wonness/wonness.github.io/assets/141399098/e88ba83a-33b5-4942-9126-d12482c00514){: width="70%" height="70%"}

- [대시보드] > [동작]의 "필터" 기능 = 대시보드에 포함된 시트에서의 "필터로 사용" 기능
    
    : 해당 시트에 있는 값들이 다른 시트에 영향을 주어 필터링됨
    
- [대시보드] > [동작]의 "하이라이트” 기능
    
    : 해당 시트에 있는 값들이 다른 시트에 영향을 주어 하이라이트됨
  
<br>

## 🏁 3일차 과제

> **매개 변수**와 **대시보드 작업(동작)**을 사용하여\
> 사용자와 **상호작용** 할 수 있는 시각화 & 대시보드 제작

<br>

### 1️⃣ **매개 변수를 사용하여 측정값 변경하기 1 & 마크 색상 표현하기**

하나의 시각화에서 여러 개의 측정값을 변경해보고, 선택한 카페인 함유량에 따라서 카테고리 색상이 표시되도록 시각화 해보자
![image](https://github.com/wonness/wonness.github.io/assets/141399098/444e796c-9cfe-44fb-a3ba-c30802ad3eff)

1. 매개변수 생성
    - “측정값 선택” 매개변수 생성
        - 데이터 유형 : 문자열
        - 허용 가능한 값 : 목록
        - 값 목록 : 칼로리, 카페인, 당류
   
    - “카페인 함유량 선택” 매개변수 생성
        - 데이터 유형 : 정수
        - 허용 가능한 값 : 범위
        - 값 범위 : 최소값 0, 최대값 200, 단계 크기 20

2. 계산된 필드 생성
    - “선택한 측정값” 계산된 필드 생성
        - “측정값 선택” 매개 변수에 따라 다른 측정값을 불러오는 필드
    
        ```markdown
        IF [측정값 선택] = '칼로리' THEN [칼로리(Kcal)] 
        ELSEIF [측정값 선택] = '카페인' THEN [카페인(MG)]
        ELSEIF [측정값 선택] = '당류' THEN [당류(G)]
        END
        ```
  
    - “카페인 > 선택한 카페인” 계산된 필드 생성
        - “카페인 함유량 선택” 매개 변수에서 선택한 값보다 높은 값과 낮은 값을 구분하는 필드
    
        ```markdown
        IF AVG([카페인(Mg)]) > [카페인 함유량 선택] THEN '선택한 카페인보다 많이 들음'
        ELSE "선택한 카페인보다 적게 들음"
        END
        ```
    
3. 행 선반 (y축) : 카테고리 / 열 선반 (x축) : 평균 선택한 측정값

4. “카페인 > 선택한 카페인”을 색상에 적용
    - “카페인 함유량 선택”에서 선택한 값보다 높으면 노란색, 낮으면 회색을 띠도록 시각화

5. 제목 > 삽입 > 매개변수.측정값 선택
    - “측정값 선택“ 값에 따라서 시트 제목이 변경되도록 시각화

<br>

### 2️⃣ **매개 변수를 사용하여 측정값 변경하기 2**

선택한 측정값에 따라서 두 측정값의 상관 관계를 살펴보는 동적 시각화를 만들어 보자
![image](https://github.com/wonness/wonness.github.io/assets/141399098/4c257082-579d-40b4-9f7e-0b50ed8e920a)

1. 매개변수 생성
    - “X축 선택” 매개변수 생성
        - 데이터 유형 : 문자열
        - 허용 가능한 값 : 목록
        - 값 목록 : 카페인, 칼로리, 나트륨, 당류
     
    - “Y축 선택” 매개변수 생성
        - “X축 선택” 매개변수와 같은 내용
      
2. 계산된 필드 생성
    - “X축” 계산된 필드 생성
        - “X축 선택” 매개 변수에 따라 다른 측정값을 불러오는 필드
    
        ```markdown
        CASE [X축 선택]
        WHEN "카페인" THEN [카페인(Mg)]
        WHEN "칼로리" THEN [칼로리(Kcal)]
        WHEN "나트륨" THEN [나트륨(Mg)]
        WHEN "당류" THEN [당류(G)]
        END
        ```
    
    - “Y축” 계산된 필드 생성
        - “Y축 선택” 매개 변수에 따라 다른 측정값을 불러오는 필드
    
        ```markdown
        CASE [Y축 선택]
        WHEN "카페인" THEN [카페인(Mg)]
        WHEN "칼로리" THEN [칼로리(Kcal)]
        WHEN "나트륨" THEN [나트륨(Mg)]
        WHEN "당류" THEN [당류(G)]
        END
        ```
    
3. 행 선반 (y축) : Y축 / 열 선반 (x축) : X축 → 둘 다 연속형 변수
4. 메뉴명과 카테고리를 마크 선반의 세부 정보에 놓기
    - 시각화의 **집계 기준이 메뉴명 수준(하위수준)으로 변경**
    - 카테고리는 3번 대시보드 동작에서 사용
5. 분석 탭 > 평균 라인을 테이블에 추가
    - 라인은 각각의 측정값 별 구간을 의미

<br>

### 3️⃣ 대시보드 동작 적용하기

1번, 2번 과제를 이용해서 대시보드 만들고, **대시보드 동작**을 이용해 **해당 카테고리 별 제품을 하이라이트** 해보자
![image](https://github.com/wonness/wonness.github.io/assets/141399098/ff2bb6b8-a26b-48b6-8186-7a6f7dcaf307)

1. 대시보드 > 동작 > 동작 추가 > 하이라이트 선택
2. 원본 시트 : "카테고리 별 카페인" / 대상 시트 : "칼로리와 나트륨의 상관관계" / 동작 실행 조건 : 마우스 오버
    - “카테고리 별 카페인”에서 카테고리에 마우스를 올리면, “칼로리와 나트륨의 상관관계”에서 해당 제품이 하이라이트 되도록 시각화
3. 대상 하이라이트 > 선택한 필드 > **“카테고리”** 선택
    - 대상 시트에 하이라이트가 되는 필드가 없다면 올바르게 작동하지 않으므로, <span style="text-decoration: underline;">2번 과제에서 “카테고리” 세부 정보를 추가해 둔 것</span> !

<br>
