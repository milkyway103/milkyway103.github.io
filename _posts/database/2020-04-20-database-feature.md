---
layout: post
title: 데이터베이스의 특징과 등장 배경
categories : database
tags : database filesystem DBMS
comments : true
---

## 데이터베이스를 사용하는 이유
데이터베이스가 존재하기 이전에는 파일 시스템을 이용하여 데이터를 관리하였다. (현재도 부분적으로 사용되고 있다.) 데이터를 각각의 파일 단위로 저장하며 이러한 일들을 처리하기 위한 독립적인 어플리케이션과 상호 연동이 되어야 한다.
파일 시스템을 이용하여 데이터를 관리할 때 데이터 종속성 문제와 중복성, 데이터 무결성 문제가 생긴다.

### 파일 시스템
각 응용 프로그램(어플리케이션)이 독립적으로 자료를 파일 형태로 배치하고 관리하는 전통적인 데이터 처리 시스템
대부분의 경우 데이터 파일과 어플리케이션이 1:1로 대응됨
-> 응용프로그램과 data 간 상호 의존성이 높다.

### 파일 시스템의 문제점
- 데이터 종속성 문제 (Data Dependency)
  : 데이터 구조가 바뀔 때 프로그램을 수정하고 다시 컴파일해야 한다.
- 데이터 중복성 문제 (Data Redundancy)
  : 한 시스템 내의 자료가 중복 저장됨
    -> 데이터의 추가, 변경, 삭제가 있을 경우 데이터 간 불일치가 일어날 수 있음 (데이터 무결성 문제)

### DBMS
DataBase Management System
데이터베이스를 관리하며 응용 프로그램들이 데이터베이스를 공유하며 사용할 수 있는 환경을 제공하는 소프트웨어

### DBMS의 장점
- 데이터를 체계적으로 통합하고 구성함으로써 **중복을 최소화**
- 데이터의 동시 공유가 가능
- 수정, 삭제, 검색에 용이

### 데이터베이스의 특징
1. 데이터의 독립성
    - 물리적 독립성 : 데이터베이스 사이즈를 늘리거나 성능 향상을 위해 데이터 파일을 늘리거나 새롭게 추가하더라도 개념 스키마 혹은 관련된 응용 프로그램을 수정할 필요가 없다.
    - 논리적 독립성 : 논리 스키마가 변하더라도 상위 단계에 존재하는 외부 스키마가 영향을 받지 않음.
2. 데이터의 무결성 : 여러 경로를 통해 잘못된 데이터가 발생하는 경우의 수를 방지. 데이터의 유효성 검사를 통해 데이터의 무결성을 구현한다.
3. 데이터의 보안성 : 인가된 사용자들만 데이터베이스 혹은 데이터베이스 내 자원에 접근할 수 있도록 계정 관리 또는 접근 권한을 설정함으로써 모든 데이터에 보안을 구현할 수 있다.
4. 데이터의 일관성 : 연관된 정보를 논리적인 구조로 관리함으로써 어떤 하나의 데이터만 변경했을 경우 발생할 수 있는 데이터의 불일치성을 배제할 수 있다. 또한 작업 즁 일부 데이터만 변경되어 나머지 데이터와 일치하지 않는 경우의 수를 배제할 수 있다.
5. 데이터의 중복 최소화 : 데이터베이스는 데이터를 통합하여 관리함으로써 파일 시스템의 단점 중 하나인 자료의 중복과 데이터의 중복성 문제를 해결할 수 있다.

## 데이터베이스의 성능
데이터베이스의 성능 이슈는 디스크 입출력을 어떻게 줄이느냐에서 시작된다. 디스크 입출력이란 디스크 드라이브의 플래터(원판)을 돌려서 읽어야 할 데이터가 저장된 위치로 디스크 헤더로 이동시킨 다음 데이터를 읽는 것을 의미한다. 이때 데이터를 읽는데 걸리는 시간은 디스크 헤더를 움직여서 읽고 쓸 위치로 옮기는 단계에서 결정된다.
즉 디스크의 성능은 디스크 헤더의 위치 이동 없이 얼마나 많은 데이터를 한 번에 기록하느냐에 따라 결정된다.

그렇기 때문에 순차 입출력이 랜덤 입출력보다 빠를 수밖에 없다. 하지만 현실에서는 대부분의 입출력 작업이 랜덤 입출력이다. 랜덤 입출력을 순차 입출력으로 바꾸어 실행할 수는 없을까? 이러한 생각에서부터 시작되는 **데이터베이스 쿼리 튜닝은 랜덤 입출력 자체를 줄여주는 것이 목적** 이라고 할 수 있다.

## 테이블 & 레코드

### 출처
- [Interview_Question_for_Beginner](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database#%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)
- [파일시스템/DBMS 장단점](https://moolgogiheart.tistory.com/47)
- [파일시스템과 DBMS](https://hoonmaro.tistory.com/16)