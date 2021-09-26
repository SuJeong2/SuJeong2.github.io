---
layout: post
title: DDL - Data Definition Language, 데이터 정의어
subtitle: 데이터베이스 DDL(데이터 정의어)에 대해 정리합니다.✍
tags: [Database]
author: Sujeong Lee
comments: True
---

# DDL 이란?

- Data Definition Language
- 데이터 정의어

- 데이터베이스 객체(table, view, index 등)의 구조를 정의

- 테이블을 생성, 칼럼 추가, 타입 변경, 제약조건을 지정하거나 수정할 때 사용

**DB 생성, 변경, 삭제, 사용**

```
create database testdb;
alter database testdb default character set uft8mb4 collate utf8mb4_general_ci;
drop database testdb;
use testdb;
```

**Table 생성하기**

```
CREATE TABLE countries
    ( country_id      CHAR(2) NOT NULL
    , country_name    VARCHAR(40)
    , region_id       INT
    , CONSTRAINT     country_c_id_pk


	PRIMARY KEY (country_id)
    );
```

- 각 칼럼을 선언하고 마지막에 primary key로 쓸 칼럼을 지정

```
CREATE TABLE countries
    ( country_id      CHAR(2) NOT NULL PRIMARY KEY
    , country_name    VARCHAR(40)
    , region_id       INT
    , CONSTRAINT     country_c_id_pk
    );
```

- 칼럼 선언하면서 primary key 선언

**추가적인 속성들(attributes)**

1. PRIMARY KEY : 테이블에서 행을 고유하게 식별하기 위해 사용
   (반적으로 ID 번호, AUTO INCREMENT와 같이 사용되는 경우가 많다)
2. NOT NULL : 각 행은 해당 열의 값을 포함, null 값 허용하지 않음
3. DEFAULT value : 값이 전달되지 않을 때, 추가되는 기본 값을 해당 값으로 설정
4. UNSIGNED : type이 숫자인 경우만 해당되며 숫자가 0 또는 양수로 제한
5. AUTO INCREMENT : 새 레코드가 추가 될 때마다 필드 값을 자동으로 1씩 증가

**Table 생성시 제약 조건**

- 컬럼에 저장될 데이터의 조건을 설정
- 제약조건을 설정하면 조건에 위배되는 데이터는 저장 불가
- 테이블 생성시 컬럼에 직접 지정하거나 constraint로 지정, 또는 ALTER을 이용하여 설정

1. NOT NULL : 칼럼에 NULL 값을 저장할 수 없다. 반드시 쿼리문을 이용하여 값을 설정해야함
2. UNIQUE : 칼럼에 중복된 ㄱ밧을 저장할 수 없지만, NULL은 허용
3. PRIMARY KEY(기본키) : 컬럼에 중복된 값을 저장할 수 없고, NULL도 허용하지 않음
   → NOT NULL + UNIQUE
   주로 유일한 row 값을 지정할 때 사용
4. FOREIGN KEY(참조키, 외래키) : 특정 테이블의 PRIMARY KEY 컬럼에 저장되어 있는 값만 저장, references를 이용해 어떤 컬럼에 어떤 데이터를 참조하는지 반드시 지정해야 함
5. DEFAULT : NULL 값이 들어올 경우 기본 설정되는 값을 지정
