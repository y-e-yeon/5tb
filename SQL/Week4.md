# **📌 Week 4: CTE, GROUP_CONCAT()**

## **주요 개념**

- **CTE, GROUP_CONCAT()**:
    - `WITH RECURSIVE`
    - `GROUP_CONCAT()`
- 해당 문법의 개념과 사용 시 주의할 점들을 정리하여 깃허브에 정리해 주세요.

---

## **✅ GROUP_CONCAT() 학습 및 문제 풀이**

### 📖 MySQL 공식 문서 - GROUP_CONCAT()[🔗](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_group-concat)

**GROUP_CONCAT()**
- 그룹의 비NULL 값을 연결한 문자열을 반환
- 그룹에 NULL 값만 있을 경우 NULL 반환
- 일반적으로 GROUP BY 절과 함께 사용되며, 하나의 그룹에 속한 여러 값을 한 줄로 합침

**문법**
```sql
GROUP_CONCAT([DISTINCT] expr [, expr ...]
             [ORDER BY col_name [ASC | DESC] [, col_name ...]]
             [SEPARATOR str_val])
```

**주요 옵션 설명**
| 옵션         | 설명 |
|--------------|------|
| `DISTINCT`   | 중복 값을 제거하고 고유한 값만 연결 |
| `ORDER BY`   | 연결되는 값을 지정한 열 기준으로 정렬<br>기본은 오름차순(ASC)이며, `DESC`를 명시하면 내림차순 정렬됨됨 |
| `SEPARATOR`  | 연결된 값들 사이에 삽입할 구분자를 지정<br>기본값은 쉼표(,)이며, 빈 문자열 `''`로 지정하면 구분자 없이 연결됨됨 |

**반환 타입과 제한**
- 반환 타입: TEXT 또는 BLOB
    - 단, group_concat_max_len이 512 이하면 VARCHAR 또는 VARBINARY
- 기본 최대 길이: 1024 (바이트 단위)
    - group_concat_max_len 시스템 변수를 조정하여 변경 가능
- 전체 길이는 max_allowed_packet 값의 제한도 받음

**기타 특이사항**
- 이진 문자열(BINARY)인 경우 16진수(hex) 형태로 출력될 수 있음 (mysql 클라이언트 설정 --binary-as-hex에 따라 다름)
- NULL 값은 무시되며 결과에는 포함되지 않음
- 여러 열을 GROUP_CONCAT(expr1, expr2) 형태로 사용 가능

**유사 함수 비교**
| 함수명            | 설명 |
|-------------------|------|
| `CONCAT()`        | 여러 인자를 순서대로 연결한 문자열을 반환 |
| `CONCAT_WS()`     | 지정한 구분자(separator)를 사용하여 인자들을 연결, `WS`는 "With Separator"를 의미 |

### 📝 programmers - 우유와 요거트가 담긴 장바구니[🔗](https://school.programmers.co.kr/learn/courses/30/lessons/62284) `GROUP_CONCAT()`

    ```jsx
    조건:
    - WITH 구문을 사용해 가상의 테이블을 먼저 만들고,
    - GROUP_CONCAT() 을 활용하여 상품명을 하나의 문자열로 합친 후,
    - 그 문자열을 활용해 Milk와 Yogurt가 모두 존재하는 장바구니만 필터링해야 합니다.
    ```
    
    → 성능 상 더 좋은 쿼리를 짜는 방법들이 있지만, 이번 문제는 **GROUP_CONCAT()**을 활용하여 데이터를 가공하는 연습을 목표로 합니다.
    
### 📝 programmers - 언어별 개발자 분류하기[🔗](https://school.programmers.co.kr/learn/courses/30/lessons/276036) **(OPTIONAL)**는 2주차에 `HAVING`을 공부할 때 풀어본 문제입니다.
    
    해당 문제 상황을 참고하여, **각 개발자가 보유한 기술 목록과 기술 카테고리를 요약하여 출력하는 쿼리**를 작성해주세요.**`GROUP_CONCAT()`**
    
    출력 결과는 다음과 같아야 합니다.
    
    ![4-2.PNG](attachment:079e9ecb-3a6d-4dea-aea8-60981d09d411:4-2.png)
    

## **✅ CTE(`WITH RECURSIVE`) 학습 및 문제 풀이**

### 📖 MySQL 공식 문서 - WITH RECURSIVE[🔗](https://dev.mysql.com/doc/refman/8.0/en/with.html)

**WITH RECURSIVE (재귀 CTE)**
- CTE(Common Table Expression): 쿼리 내에서 일시적으로 정의되는 이름 있는 결과 집합.
- WITH RECURSIVE는 CTE가 자기 자신을 참조할 수 있도록 허용하는 문법.
- 재귀 CTE는 계층 구조 탐색, 시퀀스 생성, 트리 데이터 처리 등에 유용.

**문법 구조**
```sql
WITH RECURSIVE cte_name (column_list) AS (
  SELECT ...          -- 초기(비재귀) SELECT
  UNION [ALL|DISTINCT]
  SELECT ... FROM cte_name ...  -- 재귀 SELECT
)
SELECT * FROM cte_name;
```
- RECURSIVE: 자기 자신을 참조하는 경우 필수
- column_list: 생략 가능. 명시하면 결과 열 이름을 직접 지정
- UNION ALL: 기본. DISTINCT를 사용하면 중복 제거 (예: 순환 방지)

**작동 방식**
1. 초기 SELECT (비재귀 부분): 시작값 제공
2. 재귀 SELECT: 이전 결과를 참조하며 반복
3. 종료 조건: WHERE 절에서 재귀를 끝낼 조건을 반드시 명시해야 함
- 재귀는 더 이상 새로운 행이 생성되지 않을 때 자동으로 멈춤
- 또는 시스템 설정(cte_max_recursion_depth)이나 LIMIT으로 강제 종료 가능

**제약 사항**
- 재귀 SELECT는 반드시 FROM 절에서 단 1회만 CTE 자신을 참조해야 함
- CTE는 왼쪽 조인(LEFT JOIN)의 오른쪽에 오면 안 됨

**재귀 깊이 제한 및 종료 조건**
- 반드시 종료 조건이 있어야 무한 루프 방지
- 시스템 변수로 제한 가능:
    - cte_max_recursion_depth (기본값: 1000)
    - max_execution_time (ms 단위 시간 제한)

**CTE vs 파생 테이블 비교**
| 항목         | CTE                                | 파생 테이블 (Derived Table)             |
|--------------|-------------------------------------|------------------------------------------|
| 사용 범위    | 같은 쿼리 내에서 여러 번 참조 가능 | 한 번만 사용 가능                        |
| 재귀 사용    | 가능 (`WITH RECURSIVE`)            | 불가능                                   |
| 가독성       | 쿼리 상단에 정의되어 가독성 높음    | 서브쿼리 내 포함되어 가독성 낮을 수 있음 |
| 명시적 테이블 | 정의만으로 사용 가능               | `AS 별칭`으로 반드시 지정 필요           |


### 📝 programmers - 입양 시각 구하기(2)[🔗](https://school.programmers.co.kr/learn/courses/30/lessons/59413) `WITH RECURSIVE`

