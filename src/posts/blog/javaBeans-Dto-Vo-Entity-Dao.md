---
title: "JavaBeans, DTO, VO, Entity, DAO 에 대해 알아보자"
category: "Java"
date: "2020-06-17"
desc: "우아한 CRUD 에서 다룬 내용을 정리했습니다"
thumbnail: "./images/default.jpg"
alt: "우아한 CRUD 에서 다룬 JavaBeans, DTO, VO, Entity, DAO 객체의 사용법과 차이를 정리했습니다"
---

## 이 포스팅을 작성한 이유
우아한 CRUD에서 정상혁님이 발표해주신 모든 발표 내용을 이 블로그에서 공유했는데 그 중 내가 너무나 궁금했던 내용들을 간추려서 포스팅하고자 한다.
혼자서 이해한 부분을 정리했기에 조금은 설명이 불친절할 수 있는데 더 자세한 내용은 [이 곳](https://www.youtube.com/watch?v=cflK7FTGPlg)에서 세미나를 시청해보는 걸 추천한다. 

---

### 아키텍처 패턴(?)의 변화
1. 모든  것을 한 곳에 담는 형태
2. 뷰 레이어를 분리하여 관리한 형태 (1번만도 못한 상태 발생)
3. 속성 값을 담을 클래스를 만드는 형태 (만능 클래스)
4. 속성 값을 담을 클래스를 만드는 형태(자바빈즈, VO, DTO, Entity) -- 요즘 사용 형태

---

### 속성 값을 담는 클래스들
1. 자바빈즈 
- 자바빈즈 스펙이 존재
- 구현 규칙이 있다
- getter/setter 사용
- 공개 기본 생성자 존재
- 직렬화 가능해야 함
<br>

2. VO
- (DDD관점에선) 값이 같으면 동일하다고 간주되는, 식별성이 없는 작은 객체
- (폭넓게 생각하면) DTO와 같은 의미(데이터를 나르는 측면에서 봤을 때) 
- 많은 서적에서 DTO와 VO를 거의 같다고 서술했는데 이 이유는 VO를 처음 선보인 책이 후에 2판 서적에서는 TO라고 표현했기 때문이다.
- DDD관점에서 보면 엔티티와 연관 있고, 폭넓게 생각하면 DTO와 같다. 
<br>

3. DTO
- (원래 개념)원격 호출을 효율화하기 위해 나온 패턴
- (요즘 개념)네트워크 전송 시의 Data holder 역할로 요즘은 폭넓게 쓰이는 느낌
  - (요즘 쓰이는 형태)네트워크 전송과 관계없는 1회성 전송 객체
- 원래 개념과 다르게 쓰이고 있는데 문제 될 건 없을까?
  - '레이어 간의 경계를 넘어서 데이터를 전달'하는 역할은 과거와 동일하다고 생각할 수 있음
  - 다만 다양한 객체의 역할을 다 DTO로 칭하는 건 혼란도 있음
<br>

4. Entity
- 연속성과 식별성의 맥락에서 정의되는 객체(DDD관점)
- JPA의 @Entity로 익숙한 개념: DB 테이블과 대응되는 객체
- Entity와 VO를 비교하는 게 의미가 있다
  - Entity : 속성값이 모두 같아도 id 값이 다르면 다른 엔티티
  - VO     : 속성값이 모두 같으면 같은 VO

---

### 만능 클래스 문제를 어떻게 해결했을까?
1. 엔티티 감추기
  - ENTITY가 뷰, API 응답에 바로 노출될 때의 비용이 큼
  - 외부 노출용 DTO를 따로 만들기
    - 이름 짓기가 어려움(비슷한 속성을 가진 객체가 많이 만들어지기 때문에)
2. 엔티티 간의 선긋기
  - 만능 클래스가 많은 테이블을 참조하는 형태가 좋지 않은 문제를 발생시킴
  - 다른 AGGREGATE의 Root를 직접 참조하지 않고 ID로만 참조하기

---

### 쿼리로 모든 걸 해결하지 않아야 하는 이유
- 여러 Aggregate에 걸친 조회 = JOIN 사용 = 성능 저하



1. JOIN을 하지 않고 해결할 수 있는 케이스
  - Service 레이어에서 조합
  

2. JOIN이 필수적인 경우 (항상 부딪히는 문제)
  - Service 단에서 다른 AGGREGATE 를 다시 조합 (SELECT 문에 같은 AGGREGATE 컬럼만 존재)
  - 맞춤형 전용 DTO 생성 (SELECT 문에 다른 AGGREGATE 컬럼도 존재)

---

### Repository vs DAO
1. DAO 
  - 퍼시스턴스 레이어를 캡슐화
2. Repository
  - 도메인 레이어에 객체 지향적인 컬렉션 관리 인터페이스를 제공

- REPOSITORY를 깔끔하게 유지는 하는데 이 2가지 개념을 구분해서 생각하는 게 도움이 됐던 것 같다. REPOSITORY는 AGGREGATE와 1:1 개념, DAO는 앞과 같은 생각을 안하는, 그런 것을 깨고 접근할 때 생각하는 것 같다.

---

### Immutable vs Mutable 객체 
1. Imuutable
- 다른 레이어에 메서드 파라미터로 Immutable 객체를 보내도 값이 안 바뀌었다는 확신을 할 수 있다.
- DTO류가 여러 레이어를 오간다면 Immutable하면 더 좋다.
  - 보낼 때, 생성할 때 바뀌지 않는 점이 개발할 때 안심할 수 있어서 좋다.
2. Mutable
- 상태를 바꾸는 메서드가 포함될 수도 있다. 
  - 만약 상태를 바꾸는 메서드가 있다면 시스템 전체 상태를 바꾸는 (예 : 이슈 타이틀을 바꿀 때 DB의 타이틀까지 변경) 그런 행위일 때 호출하는 건 상당히 정당한 것 같다.
  - 메서드명도 그 행위를 잘 드러내어야한다

---

### 끝나지 않는 고민
- 비슷한 속성을 가진 클래스가 너무 많이 생기는 건 아닐까?
  - DTO 이름 짓기 어려움
