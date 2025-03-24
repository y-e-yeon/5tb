# **📌 Week 1: 윈도우 함수 (Window Functions)**

## **주요 개념**

- **윈도우 함수 (Window Functions)**:
    - `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`
    - `LAG()`, `LEAD()`
    - `SUM() OVER()`, `AVG() OVER()`
    - `PARTITION BY`, `ORDER BY` 등
- 해당 문법의 개념과 사용 시 주의할 점들을 정리하여 깃허브에 정리해 주세요.

---

## **✅ 윈도우 함수 (Window Functions) 학습 및 문제 풀이**


### 📖 14.20.2. Window Function Concepts and Syntax[🔗](https://dev.mysql.com/doc/refman/8.0/en/window-functions-usage.html)

### 📖 14.20.1. Window Function Descriptions[🔗](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html)

### 📖 14.20.4. Named Windows[🔗](https://dev.mysql.com/doc/refman/8.0/en/window-functions-named-windows.html)

### 📖 14.19.1. Aggregate Function Descriptions[🔗](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html)
→ over_clause를 이용하여 집계함수를 윈도우함수처럼 사용하는 부분만 공부해보세요.
        

---
### 📝 LeetCode - Rank Scores[🔗](https://leetcode.com/problems/rank-scores/description/) `DENSE_RANK()`

### 📝 Solvesql - 다음날도 서울숲의 미세먼지 농도는 나쁨 😢[🔗](https://solvesql.com/problems/bad-finedust-measure/) `LEAD()`

### 📝 programmers - 그룹별 조건에 맞는 식당 목록 출력하기[🔗](https://school.programmers.co.kr/learn/courses/30/lessons/131124)
→ 문제를 푸는 다양한 방식이 있지만, **윈도우 함수를 사용하여** 해결하는 방식에 대해 고민해 보시길 바랍니다.
* DATE 형식을 문제의 조건에 맞게 작성해주어야 통과되므로, DATE_FORMAT을 사용해주세요~