---
layout: post
title: DML - Data Manipulation Language, 데이터 정의어
subtitle: 데이터베이스 DML(데이터 조작어)에 대해 정리합니다.✍
tags: [Database]
author: Sujeong Lee
comments: True
---

# DML 이란?

- Data Manipulation Language

- 데이터 조작어

- 종류

  > insert - 추가  
  >  select - 조회  
  > update - 수정  
  > delete - 삭제

<br>
<br>

# SELECT - 데이터 조회

```
SELECT *
FROM STUDENTS;

SELECT ALL student_ID
FROM STUDENTS;

SELECT DISTINCT student_ID
FROM STUDENTS;

SELECT student_ID, student_NAME
FROM STUDENTS;
```

#### SELECT Clause 조건

- \* : 테이블에서 모든 열을 선택
- ALL : 선택된 모든 행을 반환
- DISTINCT : 선택된 모든 행 중에서 중복 행은 제거
- 컬럼명 : 해당 컬럼명을 선택

```
SELECT  student_NAME as 이름
	student_EMAIL "이메일"
        student_GRADE "점수"
        student_GRADE/10 "평균"
FROM STUDENTS;
```

- 사칙연산 가능 (+ \* / - )

- ALIAS 별칭 : 조회했을 때, 해당 컬럼명의 이름을 설정

  예를들어 조회하면

  | student_NAME | student_EMAIL | student_GRADE | student_GRADE/10 |
  | ------------ | ------------- | ------------- | ---------------- |
  | ...          | ...           | ...           | ...              |

  이렇게 나오는게 아니라

  | **이름** | **이메일** | **점수** | **평균** |
  | -------- | ---------- | -------- | -------- |
  | **...**  | **...**    | **...**  | **...**  |

  이렇게 나오게 된다.

---

**IFNULL(값1, 값2) - NULL값 처리하기**

```
SELECT  student_NAME as 이름
	student_EMAIL "이메일"
        student_GRADE "점수"
        (IFNULL(student_GRADE,1)/10) "평균"
FROM STUDENTS;
```

IFNULL은 값1이 NULL일 경우 값2를 리턴해서 계산해준다. 아닐 경우 값1로 계산

---

**CASE문**

```
SELECT  student_NAME, student_EMAIL,
		case  when student_GRADE>80 then '우수'
        	  when student_GRADE>50 then '평균'
              else '부족'
        end "평가"
FROM STUDENTS;
```

컬럼 값에 따라 값을 설정해준다

컬럼명 : 평가

80점 이상 : 우수 / 50점 이상 : 평균 / 50점 미만 : 부족

---

**조건 검색 - WHERE**

```
//학생 이름이 TEST이면서 점수가 60점 이상인 학생의 이름,이메일,점수 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_GRAGE>=60 AND student_NAME = 'TEST';

//점수가 20점이거나 50점인 학생의 이름,이메일,점수 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_GRAGE = 50 OR student_GRAGE = 20;

//점수가 50점이 아니고 20점이 아닌 학생의 이름,이메일,점수 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_GRAGE != 50 AND student_GRAGE != 20;

//점수가 20점이거나 50점이 아닌 학생의 이름,이메일,점수 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE NOT (student_GRAGE = 50 OR student_GRAGE = 20);
```

- 조건에 맞는 행만 조회
- AND, OR, NOT 사용 가능

---

**IN, BETWEEN**

```
//학생 PART 번호가 50,60,70인 학생의 이름, 이메일, 성적 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_PART IN (50, 60, 70);

//학생 PART 번호가 50,60,70이 아닌 학생의 이름, 이메일, 성적 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_PART NOT IN (50, 60, 70);
//성적이 50이상 80이하인 학생의 이름, 이메일, 성적 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_GRAGE BETWEEN 50 AND 80;
```

- 결국 WHERE student_GRAGE >= 50 AND student_GRAGE <=80 이랑 같다

---

**NULL 비교 - IS NULL / IS NOT NULL**

```
//이메일을 알 수 없는 학생의 이름, 이메일, 성적 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_EMAIL IS NULL;

//이메일을 알 수 있는(NULL값이 아닌) 학생의 이름, 이메일, 성적 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_EMAIL IS NOT NULL;
```

※ 주의 ※

절대 = NULL 로 비교할 수 없다

```
WHERE student_EMAIL = NULL; //안 된다
```

---

**LIKE (Wild card : %, \_ )**

```
//이름에 K가 들어가는 학생의 이름, 이메일, 성적 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_NAME LIKE '%K%';

//이름의 2번째에 K가 들어간 학생의 이름, 이메일, 성적 조회
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
WHERE student_NAME LIKE '_K%;
```

%는 글자 수 상관없는 와일드 카드

\_는 1개의 글자수를 나타냄.(자리수를 나타낸다고 생각)

---

**정렬 ORDER BY**

```
//이름 오른차순 정렬
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
ORDER BY student_NAME ASC ;

//이름 내림차순 정렬
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
ORDER BY student_NAME DESC ;
```

- ASC : 오른차순 정렬, 생략 가능
- DESC : 내림차순 정렬

```
//학생 번호로 오름차순하고 나서 이름으로 내림차순
SELECT  student_NAME, student_EMAIL, student_GRAGE
FROM STUDENTS
ORDER BY student_ID , student_NAME DESC ;
```

- 정렬 조건을 여러가지 사용할 수 있다. 순서대로 정렬이 적용된다

# Insert - 데이터 추가

```
INSERT INT 테이블 명 (컬럼명 ....)
VALUES (값들 ....)

INSERT INTO member (userid, username, userpwd)
VALUES ('kim', '김씨', '1234');

INSERT INTO member (userid, userpwd)
VALUES ('kim', '1234');

INSERT INTO member (userid, username, userpwd)
VALUES
	('kim', '김씨', '1234'),
	('park', '박씨', '1324');

INSERT INTO member (userid, username, userpwd, joindate)
VALUES
	('kim', '김씨', '1234', now()),
	('park', '박씨', '1324', now());
```

- 데이터베이스 객체에 데이터를 입력(추가)할 때

- now()하면 현재 날짜가 저장됨

# Update - 데이터 수정

```
UPDATE 테이블명 SET 컬럼명='바꿀 값'
WHERE 조건

//userid가 kim인 행의 userpwd와 email 값을 변경
UPDATE member SET userpwd="123123", email="ssafy@naver.com"
where userid = 'kim';

//모든 데이터의 userpwd, email 값을 변경
UPDATE member SET userpwd="123123", email="ssafy@naver.com"
```

- WHERE 절의 조건에 만족하는 레코드의 값을 변경
- 따라서, WHERE 조건문을 생략하면 모든 데이터가 바뀌게 된다 (생각만해도 끔찍..😫)

# DELETE - 데이터 삭제

```
DELETE FROM 테이블 명 WHERE 조건

//userid가 kim인 레코드 삭제
DELETE from member WHERE userid='kim';
```

- 조건에 만족하는 레코드의 값을 변경
- 따라서, WHERE 조건문을 생략하면 모든 데이터가 바뀌게 된다 (이것도 끔찍..😫)
