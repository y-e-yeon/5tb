## **📌 Week 5: 정규표현식&비트연산자**

## **주요 개념**

- **정규 표현식 (REGEXP)**
    - `REGEXP_LIKE()`
    - `REGEXP_REPLACE()`
    - `REGEXP_SUBSTR()`
    - `pattern syntax`
- **비트 연산자**: `&`, `|`, `^`, `<<`, `>>`
- 해당 문법의 개념과 사용 시 주의할 점들을 정리하여 깃허브에 정리해 주세요.

---

## **✅ 정규표현식 학습 및 문제 풀이**

### 📖 MySQL 공식 문서 - 정규표현식[🔗](https://dev.mysql.com/doc/refman/8.0/en/regexp.html) 

| 함수/연산자                                                                  | 설명                                                |
| ----------------------------------------------------------------------- | ------------------------------------------------- |
| `REGEXP`, `RLIKE`                                                       | 문자열이 정규표현식과 **일치하는지 여부** 반환 (`REGEXP_LIKE()`와 동일) |
| `NOT REGEXP`                                                            | 정규표현식과 **일치하지 않는지 여부** 확인                         |
| `REGEXP_LIKE(expr, pat[, match_type])`                                  | 정규표현식과 일치 여부 반환 (`1` 또는 `0`)                      |
| `REGEXP_INSTR(expr, pat[, pos, occurrence, return_option, match_type])` | 일치하는 **위치의 인덱스 반환**                               |
| `REGEXP_SUBSTR(expr, pat[, pos, occurrence, match_type])`               | 정규표현식과 **일치하는 하위 문자열 반환**                         |
| `REGEXP_REPLACE(expr, pat, repl[, pos, occurrence, match_type])`        | 일치하는 부분을 **지정 문자열로 치환**                           |

#### match_type 옵션
| 옵션  | 의미                            |
| --- | ----------------------------- |
| `c` | 대소문자 구분 (case-sensitive)      |
| `i` | 대소문자 구분 안함 (case-insensitive) |
| `m` | 멀티라인 모드 (줄바꿈 문자 인식)           |
| `n` | `.`이 줄바꿈도 포함                  |
| `u` | Unix-style 줄바꿈만 사용 (`\n` 인식)  |

#### 정규표현식 문법
| 패턴      | 의미       | 예시         | 결과              |
| ------- | -------- | ---------- | --------------- |
| `^`     | 문자열의 시작  | `^abc`     | `abc`로 시작       |
| `$`     | 문자열의 끝   | `xyz$`     | `xyz`로 끝        |
| `.`     | 임의의 한 문자 | `a.c`      | `abc`, `acc`    |
| `*`     | 0개 이상 반복 | `a*`       | `aa`, `a`, `""` |
| `+`     | 1개 이상 반복 | `a+`       | `a`, `aaa`      |
| `?`     | 0 또는 1개  | `a?`       | `a`, `""`       |
| `a\|b`  | a 또는 b   | `foo\|bar` | `foo`, `bar`    |
| `(abc)` | 그룹 지정    | `(ab)+`    | `abab`, `ab`    |
| `{n}`   | 정확히 n개   | `a{3}`     | `aaa`           |
| `{n,}`  | 최소 n개    | `a{2,}`    | `aa`, `aaaa`    |
| `{m,n}` | m\~n개    | `a{2,4}`   | `aa`, `aaa`     |

#### 문자 클래스
| 패턴            | 의미               |
| ------------- | ---------------- |
| `[abc]`       | a, b, c 중 하나     |
| `[^abc]`      | a, b, c 제외       |
| `[a-z]`       | 소문자 알파벳          |
| `[[:digit:]]` | 숫자 (`0-9`)       |
| `[[:alpha:]]` | 알파벳              |
| `[[:alnum:]]` | 알파벳 + 숫자         |
| `[[:space:]]` | 공백, 탭, 줄바꿈 등     |
| `\\b`         | 단어 경계 (ICU에서 지원) |

#### 함수별 예시
```sql
--REGEXP_LIKE
SELECT REGEXP_LIKE('abc', 'ABC', 'i');  -- 1 (대소문자 무시)

--REGEXP_INSTR
SELECT REGEXP_INSTR('dog cat dog', 'dog');     -- 1
SELECT REGEXP_INSTR('dog cat dog', 'dog', 2);  -- 9

--REGEXP_SUBSTR
SELECT REGEXP_SUBSTR('abc def ghi', '[a-z]+');        -- 'abc'
SELECT REGEXP_SUBSTR('abc def ghi', '[a-z]+', 1, 3);  -- 'ghi'

--REGEXP_REPLACE
SELECT REGEXP_REPLACE('a b c', 'b', 'X');  -- 'a X c'
```

#### 주의사항 및 팁
- 이모지(🍣)와 같은 4바이트 문자 처리 시 주의 필요
- (, [, ], \ 등은 두 번 이스케이프 필요 (\\(, \\[ 등)
- ICU는 \b로 단어 경계 표시, Spencer 방식의 [[:<:]]와 [[:>:]]는 지원 안함
- 성능을 위해 [a-z] 같은 범위 표현이 [[:alpha:]]보다 빠름
- REGEXP_LIKE()가 가장 기본적인 매칭 함수이며, 나머지는 그 확장 버전

### 📝 programmers - 서울에 위치한 식당 목록 출력하기[🔗](https://school.programmers.co.kr/learn/courses/30/lessons/131118)
```sql
```
![image](../SQL/image/Week5/.png)

## **✅ 비트연산자 학습 및 문제 풀이**

### 📖 MySQL 공식 문서 - 비트연산자[🔗](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html)

| 연산자/함수                               | 설명                |       |
| ------------------------------------ | ----------------- | ----- |
| `&`                                  | 비트 AND            |       |
| \`                                   | \`                | 비트 OR |
| `^`                                  | 비트 XOR            |       |
| `~`                                  | 비트 반전             |       |
| `<<`                                 | 왼쪽 시프트            |       |
| `>>`                                 | 오른쪽 시프트           |       |
| `BIT_COUNT()`                        | 설정된 비트의 수 반환      |       |
| `BIT_AND()`, `BIT_OR()`, `BIT_XOR()` | 집계 함수 (GROUP BY용) |       |

#### 연산 대상의 평가 방식
- 비트 연산은 두 가지 방식 중 하나로 평가됨:
    1. 숫자 평가 (Numeric Evaluation)
        - 기본값이며, 대부분의 상황에서 사용됨.
        - 64비트 부호 없는 정수로 변환하여 연산함.
        - 예: SELECT 29 | 15; → 31

    2. 이진 문자열 평가 (Binary String Evaluation)
        - _binary, BINARY, CAST(... AS BINARY) 등을 사용하거나 이진 타입(예: VARBINARY, BLOB)으로 선언된 변수 사용 시 적용됨.
        - 길이가 같은 이진 문자열끼리만 연산 가능.
        - 결과도 이진 문자열로 반환됨.
        - 예: SELECT _binary X'40' | X'01'; → 'A'

#### 주요 연산자
| 연산자/함수         | 의미       | 예시              | 결과                     |      |
| -------------- | -------- | --------------- | ---------------------- | ---- |
| `&`            | AND      | `29 & 15`       | `13`                   |      |
| \`             | \`       | OR              | `29 \| 15`             | `31` |
| `^`            | XOR      | `11 ^ 3`        | `8`                    |      |
| `~`            | 반전       | `~0`            | `18446744073709551615` |      |
| `<<`           | 왼쪽 쉬프트   | `1 << 2`        | `4`                    |      |
| `>>`           | 오른쪽 쉬프트  | `4 >> 2`        | `1`                    |      |
| `BIT_COUNT(n)` | 설정된 비트 수 | `BIT_COUNT(15)` | `4`                    |      |

#### 사용 시 주의할 점
1. 연산 대상의 평가 방식 주의
    - 리터럴만 사용 시: 숫자로 간주 → 정수 결과 반환
    - _binary 혹은 BINARY 사용 시: 이진 문자열로 간주 → 문자열 결과 반환
2. 이진 문자열 연산 시 길이 일치 필요
3. 결과 타입 주의

    | 연산식                 | 결과 타입                          |
    | ------------------- | ------------------------------ |
    | 숫자 vs 숫자            | 정수 (BIGINT)                    |
    | 이진 vs 이진            | 이진 문자열                         |
    | 숫자 vs 이진            | 숫자                             |
    | 집계 함수에서 NULL만 있을 경우 | 중립값 반환 (예: `FF..FF`, `00..00`) |

### 📝 programmers - 부모의 형질을 모두 가지는 대장균 찾기[🔗](https://school.programmers.co.kr/learn/courses/30/lessons/301647)