# JPA 기초 & Spring Data JPA 기초
[유튜브 채널](https://www.youtube.com/c/최범균) JPA 기초/Spring Data JPA 기초 영상에서 사용하는 예제 코드 

## JPA 기초 내용

* JPA 기초 01 일단 해보기: https://youtu.be/Zwq2McbFOn4
* JPA 기초 02 코드 구조, 영속 컨텍스트: https://youtu.be/7ljqL8ThUts
* JPA 기초 03 엔티티 CRUD 처리: https://youtu.be/kmCKAwOie_I
* JPA 기초 04 엔티티 매핑: https://youtu.be/SbMJVuv8Iyo
* JPA 기초 05 엔티티 식별자 생성 방식: https://youtu.be/Xw9uTs72SVo
* JPA 기초 06 @Embeddable: https://youtu.be/WtS5IszIueA
* JPA 기초 07 @Embeddable 다른 테이블에 매핑하기: https://youtu.be/3_sdQGfL2Lg
* JPA 기초 08 값 콜렉션 Set 매핑: https://youtu.be/lQ4-kVeHVGk 
* JPA 기초 09 값 콜렉션 List 매핑 : https://youtu.be/Wq4B5RpIeAY
* JPA 기초 10 값 콜렉션 Map 매핑: https://youtu.be/CPIgicoqLnM
* JPA 기초 11 값 콜렉션 주의사항: https://youtu.be/yK4Avtxqz-k 
* JPA 기초 12 영속 컨텍스트 & 라이프사이클: https://youtu.be/tUwg78VkWJ0 
* JPA 기초 13 엔티티 연관 매핑 시작에 앞서: https://youtu.be/rZZSYG__8Jc
* JPA 기초 14 엔티티 간 1-1 단방향 연관 매핑: https://youtu.be/BhVzS90Ep78
* JPA 기초 15 엔티티 간 N-1 단방향 연관 매핑: https://youtu.be/i8XAqCGcLqw 
* JPA 기초 16 엔티티 간 1-N 단방향 연관 매핑: https://youtu.be/LAA8ICFS8bs
* JPA 기초 17 영속성 전파 & 연관 고려사항: https://youtu.be/7JAoNNhvsjw
* JPA 기초 18 JPQL 소개: https://youtu.be/UtEhC68GTH0
* JPA 기초 19 Criteria 소개: https://youtu.be/ZAiH382rUF0
* JPA 기초 20 기타 AttributeConverter, @Formula, @DynamicUpdate(@DynamicInsert), @Immutable, @Subselect: https://youtu.be/deJnCTkjLyU

## Spring Data JPA 기초 내용

* Spring Data JPA 01 시작하기: https://youtu.be/1Q3Qtd5HZy4
* Spring Data JPA 02 리포지터리 메서드 작성 규칙: https://youtu.be/qTiHaxVc6GY
* Spring Data JPA 03 정렬 페이징 @Query: https://youtu.be/2-f9RFCT9Ik
* Spring Data JPA 04 Specification을 이용한 검색 조건 지정: https://youtu.be/-znDGy-BQJk
* Spring Data JPA 05 기타: https://youtu.be/SYqknEb0wag

## 로컬 환경 구성

### MySQL 설치

#### 직접 설치
다음 사이트에서 환경에 맞는 설치 파일 받아서 설치

* https://dev.mysql.com/downloads/

#### 도커 이용 설치
컨테이너 생성: 
```
docker create --name mysql8 -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql:8.0.27
```
MYSQL_ROOT_PASSWORD 환경변수는 DB root 사용자 암호를 지정하는데 위 명령어는 root 암호로 root를 사용

컨테이너 시작:
```
docker start mysql8
```

### 데이터베이스와 계정 생성

root 사용자로 MySQL에 접속한 뒤에 jpabegin DB와 jpauser 사용자 생성

```
create database jpabegin CHARACTER SET utf8mb4;

CREATE USER 'jpauser'@'localhost' IDENTIFIED BY 'jpapass';
CREATE USER 'jpauser'@'%' IDENTIFIED BY 'jpapass';

GRANT ALL PRIVILEGES ON jpabegin.* TO 'jpauser'@'localhost';
GRANT ALL PRIVILEGES ON jpabegin.* TO 'jpauser'@'%';
```

# 강의 정리

## 엔티티 매핑

- 기본 애노테이션
  - @Entity: 엔티티 클래스에 설정, 필수
  - @Table: 매핑한 테이블 지정
  - @Id: 식별자 속성에 설정, 필수
  - @Column: 매핑할 칼럼명 지정
    - 지정하지 않으면 필드명/프로퍼티명 사용
  - @Enumerated: enum 타입 매핑할 때 설정
  - @Temporal: java.util.Date, java.util.Calendar 매핑
    - 자바 8 시간/날짜 타입 등장 이후로 거의 안 씀
  - @Basic: 기본 지원 타입 매핑 (거의 안 씀)
- @Table 애노테이션
  - 애노테이션을 생략하면 클래스 이름과 동일한 이름에 매핑
  - 속성
    - name: 테이블 이름 (생략하면 클래스 이름고 동일한 이름)
    - catalog: 카탈로그 이름 (예, MySQL DB 이름)
    - schema: 스키마 이름 (예, 오라클 스키마 이름)
  - 예
    - @Table
    - @Table(name = "hotel_info")
    - @Table(catalog = "point", name = "point_history")
    - @Table(schema = "crm", name = "cust_stat")
- @Enumerated 애노테이션
  - 설정 값
    - EnumType.STRING: enum 타입 값 이름을 저장 (**추천**)
      - 문자열 타입 칼럼에 매핑
    - EnumType.ORDINAL (기본값): enum 타입의 값의 순서를 저장 (바뀔 수 있기에 잘 사용하지 않음)
      - 숫자 타입 칼럼에 매핑
- 엔티티 클래스 제약 조건(스펙 기준)
  - @Entity 적용해야 함
  - @Id 적용해야 함
  - 인자 없는 기본 생성자 필요
  - 기본 생성자는 public이나 protected여야 함
  - 최상위 클래스여야 함
  - final이면 안 됨
- 접근 타입
  - 두 개 접근 타입
    - 필드 접근: 필드 값을 사용해서 매핑
    - 프로퍼티 접근: getter/setter 메서드를 사용해서 매핑
  - 설정 방법
    - @Id 애노테이션을 필드에 붙이면 필드 접근
    - @Id 애노테이션을 getter 메서드에 붙이면 프로퍼티 접근
    - @Access 애노테이션을 사용해서 명시적으로 지정
      - 클래스/개별 필드에 적용 가능
      - @Access(AccessType.PROPERTY)/@Access(AccessType.FILED)
    - 저자는 필드 접근 선호: 불필요한 setter 메서드를 만들 필요 없음

## 식별자 생성 방식

- 직접 할당
  - @Id 설정 대상에 직접 값 설정
    - 사용자가 입력한 값, 규칙에 따라 생성한 값 등 예) 이메일, 주문 번호
    - 저장하기 전에 생성자 할당, 보통 생성 시점에 전달
- 식별 칼럼 방식
  - DB의 식별 칼럼에 매핑(예, MySQL 자동 증가 컬럼)
    - DB가 식별자를 생성하므로 객체 생성시에 식별값을 설정하지 않음
  - 설정 방식
    - @GeneratedValue(strategy = GenerationType.IDENTITY) 설정
    - INSERT 쿼리를 실행해야 식별자를 알 수 있음
      - EntityManager#persist() 호출 시점에 INSERT 쿼리 실행
      - persist() 실행할 때 객체에 식별자 값 할당됨
- 시퀀스 사용 방식
  - 시퀀스를 사용해서 식별자 생성
    - JPA가 식별자 생성 처리 -> 객체 생성시에 식별값을 설정하지 않음
  - 설정 방식
    - @SequenceGenerator로 시퀀스 생성기 설정
    - @GeneratedValue의 generator로 시퀀스 생성기 지정
  - EntityManager#persist() 호출 시점에 시퀀스 사죵
    - persist() 실행할 때 객체에 식별자 값 할당됨
    - INSERT 쿼리는 실행하지 않음
- 테이블 사용 방식
  - 테이블을 시퀀스처럼 사용
    - 테이블에 엔티티를 위한 키를 보관
    - 해당 테이블을 이용해서 다음 식별자 생성
  - 설정 방식
    - @TableGenerator로 테이블 생성기 설정
    - @GeneratedValue의 generator로 테이블 생성기 지정
  - EntityManager#persist() 호출 시점에 테이블 사용
    - persist()할 때 테이블을 이용해서 식별자 구하고 이를 엔티티에 할당
    - INSERT 쿼리는 실행하지 않음

## @Embeddable

- 엔티티가 아닌 타입을 한 개 이상의 필드와 매핑할 때 사용
  - 예: Address, Money 등 매핑
- 엔티티의 한 속성으로 @Embeddable 적용 타입 사용

```java
@Embeddable
public class Address {
    @Column(name = "addr1")
    private String address1;
    
    @Cplumn(name = "addr2")
    private String address2;
}

@Entity
public class Hotel {
    @Id
    @Column(name = "hotel_id")
    private String id;
    
    @Embedded
    private Address address;
}
```

- 타입 필드가 두 개면 `Repeated column` 에러 발생
  - 이땐 @AttributeOverrides 활용
```java
@Entity
public class Employee {
    @Id
    private String id;
    
    @Embedded
    private Address homeAddress;
    
    @AttributeOverrides({
        @AttributeOverride(name = "address1", column = @Column(name = "waddr1")),
        @AttributeOverride(name = "address2", column = @Column(name = "waddr2"))
    })
    @Embedded
    private Address workAddress;
}
```

- @SecondaryTable
  - 다른 테이블에 저장된 데이터를 @Embeddable로 매핑 가능
  - 다른 테이블에 저장된 데이터가 개념적으로 밸류(값)일 때 사용
    - 1-1 관계인 두 테이블을 매핑할 때 종종 출 

- Set 매핑
  - @ElementCollection과 @CollectionTable 사용 
  - Embeddable 타입은 equals랑 hash 메서드가 구현되어있어야함

- List 매핑
  - 콜렉션 테이블을 이용한 값 List 매핑
    - @ElementCollection, @CollectionTable, @OrderColumn 사용
