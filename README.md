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

- Map 매핑
  - @ElementCollection, @CollectionTable, @MapKeyColumn 사용

- 콜렉션 주의사항
  - JPA를 사용하면 페이징이라던가 여러 성능 문제를 마주치게 됨
  - CQRS(Command Query Responsibility Segregation) 기법 적용
    - 변경 기능을 위한 모델과 조회 기능을 위한 모델을 분리
      - 변경 기능: JPA 활용
      - 조회 기능: MyBatis/JdbcTemplate/JPA 중 알맞은 기술 사용
    - 모든 기능을 JPA로 구현할 필요는 없음
      - 특히 목록, 상세와 같은 조회 기능
  - JPA로 다 하겠다는 생각은 하지말고, 명령 모델과 조회 모델을 잘 분리해서 적절한 스택을 사용해보자

- 영속 컨텍스트 & 라이프사이클
  - 영속(Persistent) 엔티티/객체
    - DB 데이터에 매핑되는 메모리상의 객체
  - 영속 컨텍스트
    - 일종의 메모리 저장소
    - EntityManager가 관리할 엔티티 객체 보관
    - (엔티티 타입, 식별자) -> 엔티티 객체
    - 트랜잭션 커밋 시점에 변경 반영
    - 분리됨 상태는 변경을 추적하지 않음
  - EntityManager.close(): 영속 컨텍스트 제거
  - 배치 처리 X: 대량 변경은 굳이 JPA로 할 필요 없음. 그냥 직접 쿼리 실행이 좋음
  - 영속 컨섹스트와 캐시
    - 동일 식별자 -> 동일 객체
      - 영속 컨텍스트에 보관된 객체 조회
      - Repeatable Read 효과

## JPA 기초 14 엔티티 간 1-1 단방향 연관 매핑

- 연관 매핑은 진짜 필요할 때만 사용할 것
  - 연관된 객체 탐색이 쉽다는 이유로 사용하지 말 것
  - 조회 기능은 별도 모델을 만들어 구현 (CQRS)
- Embeddable 매핑이 가능하다면 Embeddable 매핑 사용할 것
- OneToOne은 EAGER fetch로 갖고 오기에 left join 해서 갖고옴

```java
@Entity
public class MembershipCard {
    @Id
    private String number;
    
    @OneToOne
    @JoinColumn(name = "user_email")
    private User owner;
}
```

- PK를 연결하려면 아래와 같이 작성

```java
@Entity
@Table(name = "best_pick")
public class BestPick {
    @Id @Column("user_email")
    private String email;
    
    @OneTOOne
    @PrimaryKeyJoinColumn(name = "user_email")
    private User user;
    
    private String title;
}
```

## JPA 기초 15 엔티티 간 N-1 단방향 연관 매핑

- 연관 매핑은 진짜 필요할 때만 사용할 것
  - 연관된 객체 탐색이 쉽다는 이유로 사용하지 말 것
  - 조회 기능은 별도 모델을 만들어 구현 (CQRS)
- @ManyToOne은 기본적으로 EAGER fetch이고, left join으로 갖고옴 (null일 수도 있으니)

```java
@Entity
@Table(name = "sight_review")
public class Review {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "sight_id")
    private Sight sight;
    
    private int grade;
    
    private String comment;
}
```

## JPA 기초 16 엔티티 간 1-N 단방향 연관 매핑

- 연관 매핑은 진짜 필요할 때만 사용할 것
  - 연관된 객체 탐색이 쉽다는 이유로 사용하지 말 것
  - 조회 기능은 별도 모델을 만들어 구현 (CQRS)
- Embeddable 매핑이 가능하다면 Embeddable 매핑 사용할 것
- 1-N 단방향 연관 매핑
  - 콜렉션을 사용한 매핑
    - Set
    - List
    - Map
- 저자 기준으론 10년 하면서 1-N 거의 안 썼다 함?

```java
@Entity
@Table(name = "team")
public class Team {
    @Id
    private String id;
    
    private String name;
    
    @OneToMnay
    @JoinColumn(name = "team_id")
    private Set<Player> players = new HashSet<>(); 
}
```

## JPA 기초 17 영속성 전파 & 연관 고려사항

### 영속성 전파

- 연관된 엔티티에 영속 상태를 전파
  - 예, 저장할 때 연관된 엔티티도 함께 저장
- 근데 특별한 이유 없으면, 웬만하면 사용하지 말 것
- CascadeType 사용
  - ALL
  - PERSIST: 저장할 때 연관된 엔티티 같이 저장
  - MERGE
  - REMOVE
  - REFRESH
  - DETACH

### 연관 고려 사항

- 연관 대신에 ID 값으로 참조 고려
  - 객체 탐색이 쉽다고 연관 쓰기 없기
- 조회는 전용 쿼리나 구현 사용 고려 (CQRS)
- 엔티티가 아닌 밸류인지 확인
  - 1-1, 1-N 관계에서 특히
- 1-N 보다는 N-1 (물론 진짜 어쩔 수 없이 써야한다면)
- 양방향 X

```java
@Entity
@Table(name = "team")
public class Team {

    @Id
    private String id;
    private String name;
    
    @OneToMany(
          cascade = CascadeType.PERSIST
    )
    @JoinColumn(name = "team_id")
    private Set<Player> players = new HashSet<>();
}
```

## JPA 기초 18 JPQL 소개

## JPQL

- JPA Query Language
  - SQL 쿼리와 유사
  - 테이블 대신 엔티티 이름, 속성 사용
  - select 별칭 from 엔티티명 별칭...
    - select r from Review r
    - select r from Review as r
  - 쿼리 생성
    - TypedQuery<T> EntityManager#createQuery(String ql, Class<T> resultClass)
  - where + and, or, 괄호 등
    - select r from Review r where r.hotelId = :hotelId
    - select r from Review r where r.hotelId = ?
    - select r from Review r where r.hotelId = :hotelId and r.mark > :minMark
    - select p from Player p where p.position = :pos or p.team.id = :teamId
  - 파라미터
    - 이름 사용한 경우: query.setParameter("hotelId", "H-001")
    - 인덱스 기반: query.setParameter(0, "H-001") -> ?에 대응
  - order by
    - select r from Review r order by r.id
    - select r from Review r order by r.id asc
    - select r from Review r order by r.id desc
    - select p from Player p order by p.position, p.name
    - select p from Player p order by p.team.id, p.name
  - 비교 연산자
    - `=, <>, > >= < <=, between, in, not in, like, not like, is null, is not null`
  - 페이징 처리
    - query.setFirstResult(number): 0부터 시작, 시작행
    - query.setMaxResults(number): 최대 결과 개수

### 주의
 - 다음 경우는 JPQL 말고 일반 쿼리 사용 고려
   - 여러 테이블 조인
     - 레거시 테이블 조인
   - DBMS에 특화된 쿼리 필요
     - ex: 오라클 힌트
   - 서브 쿼리 필요
   - 통계, 대량 데이터 조회/처리
 
```java
class Test {
    public static void main(String[] args) {
        TypedQuery<Review> query = em.createQuery(
                "select r from Review r where r.hotelId = :hotelId order by r.id desc",
                Review.class);
        query.setParameter("hotelId", "H-001");
        List<Review> review = query.getResultList();
    }
}
```

## JPA 기초 20 기타 AttributeConverter, @Formula, @DynamicUpdate(@DynamicInsert), @Immutable, @Subselect

### AttributeConverter 

- 매핑을 지원하지 안흔 자바 타입과 DB 타입 간 변환 처리
  - 예: boolean 타입과 char(1) 타입 간 변환

```java
public class BooleanYesNoConverter implements AttributeConverter<Boolean, String> {
    @Override
    public String converToDatabaseColumn(Boolean attribute) {
        return Ojbects.equals(Boolean.TRUE, attribute) ? "Y" : "N";
    }
    
    @Override
    public Boolean convertToEntityAttribute(String dbData) {
        return "Y".equals(dbData) ? true: false;
    }
}

@Entity
@Table(name = "notice")
public class Notice {
    @Column(name = "open_yn")
    @Convert(converter = BooleanYesNoConverter.class)
    private boolean opened;
}
```

### @Formula

- SQL을 이용한 속성 매핑
  - 조회에서만 매핑 처리 (INSERT, UPDATE 매핑 대상 아님)
  - 하이버네이트 제공 기능 (org.hibernate.annotations.Formula)
  - 주로 DB 함수 호출, 서브 쿼리 결과를 매핑

```java
@Entity
public class Notice {
    
    @Column(name = "cat")
    private String categoryCode;
    
    @Formula("(select c.name from category c where c.cat_id = cat)")
    private String categoryName;
}
```

### 수정 쿼리의 칼럼

- 수정 쿼리는 기본적으로 모든 칼럼 포함
- notice.open() 메서드를 실행하면 true로 바뀌게 됨
  - 이때, `update notice set cat=?, content=?, open_yn=?, title=? where notice_id=?` 형태로 다 나가게됨
- @DynamicUpdate/@DynamicInsert
  - @DynamicUpdate: 변경된 칼럼만 UPDATE 쿼리에 포함
  - @DynamicInsert: null이 아닌 칼럼만 INSERT 쿼리에 포함
    - 주의: 기본값을 사용할 수 있음 (null을 지정해야 할 경우 사용 X)

```java
@Entity
@Table(name = "notice")
@DynamicUpdate
public class Notice {
    private boolean opened;
    
    public void open() {
        this.opened = true;
    }
}

/** 바뀌는 부분만 나감
 * update notice
 * set open_yn=?
 * where notice_id=?
 */
```

### @Immutable

- 변경 추적 대상에서 제외 처리
  - 변경 추적 위한 메모리 사용 감소
  - 주로 조회 목적으로만 사용되는 엔티티 매핑에 사용
  - 참고:
    - @Immutable이 적용된 엔티티도 저장은 됨
    - 코드 수준에서 persist() 하지 않도록 주의

```java
@Entity
@Table(name = "notice")
@Immutable
public class NoticeReadOnly {
    @Id
    @Column(name = "notice_id")
    private Long id;
    private String title;
}
```

### @Subselect

- select 결과를 엔티티로 매핑
  - 수정 대상이 아니므로 @Immutable과 함께 사용

```java
@Subselect(
    """
    select a.article_id, a.title, w.name as writer, a.written_at
    from article a left join writer w on a.writer_id = w.id
    """
)
@Entity
@Immutable
public class ArticleListView {
    @Id @Column(name = "article_id")
    private Long id;
    private String title;
    private String writer;
    @Column(name = "written_at")
    private LocalDateTime writtenAt;
}
```

## Spring Data JPA 01 시작하기

- JPA를 쌩으로 사용하지 않음
- Spring Boot + Spring Data JPA -> (거의) 설정 없이 사용 가능
- 자동 설정
  - persistence.xml
  - EntityManagerFactory
- 스프링 연동
  - 스프링 트랜잭션 연동
  - EntityManager 연동
- spring-boot-starter-data-jpa 의존
  - 필요한 설정 자동 처리
- 스프링 부트 설정
- 엔티티 단위로 Repository 인터페이스를 상속 받은 인터페이스 생성
  - 또는 그 하위 인터페이스
- 지정한 규칙에 맞게 메서드 추가
- 필요한 곳에 해당 인터페이스 타입을 주입해서 사용
- Repository 인터페이스
  - 스프링 데이터 JPA가 제공하는 특별한 타입으로
  - 이 인터페이스를 상속한 인터페이스를 이용해 빈(bean) 객체를 생성
```java
// T: 엔티티 타입
// ID: 엔티티의 식별자 타입
public interface Repository<T, ID> {}

public interface UserRepository extends Repository<User, String> {}
```

## Spring Data JPA 02 리포지터리 메서드 작성 규칙

### 식별자로 엔티티 조회

- findById
  - T findById(ID id)
    - 없으면 null
  - Optional<T> findById(ID id)
    - 없으면 empty Optional

### 엔티티 삭제

- delete
  - void delete(T entity)
  - void deleteById(ID id)
    - 내부적으로 findById()로 엔티티를 조회한 뒤 delete()로 삭제
- 삭제할 대상이 존재하지 않으면 익셉션 발생

```java
Optional<User> userOpt = userRepository.findById("email2");
userOpt.ifPresent(user -> {
    userRepository.delete(user);
});
```

### 엔티티 저장 메서드

- save
  - void save(T entity)
  - T save(T entity)
- `User savedUser = userRepository.save(new User("a@a.com", ...))` 실행하면,
  - 내부적으로 select ~~~ from user where user.email=? 실행하고 아래 실행
  - insert into user (create_date, name, email) values (?, ?, ?)

### save() 동작 방식

- 새 엔티티면 EntityManager#persist() 실행
- 새 엔티티가 아니면 EntityManager#merge() 실행
- 새 엔티티인지 판단하는 기준
  - Persistable을 구현한 엔티티
    - isNew()로 판단
  - @Version 속성이 있는 경우
    - 버전 값이 null이면서 새 엔티티로 판단
  - 식별자가 참조 타입이면
    - 식별자가 null이면 새 엔티티로 판단
  - 식별자가 숫자 타입이면
    - 0이면 새 엔티티로 판단

```java
// userRepository.save(new User("a.a.com", ...) // a.acom가 참조 타입
public class Entity { // 내가 임의로 지은거
  public <S extends T> S save (S entity) {
    Assert.notNull(entity, "Entity must not be null.");
    if (this.entityInformation.isNew(entity)) {
      this.em.persist(entity);
      return entity;
    } else {
        return this.em.merge(entity);
    }
  }
}
```

### 특정 조건으로 찾기

- findBy프로퍼티(값): 프로퍼티가 특정 값인 대상
  - List<User> findByName(String name)
  - List<Hotel> findByGradeAndName(Grade g, String name)
- 조건 비교
  - List<User> findByNameLike(String keyword)
  - List<User> findByCreatedAtAfter(LocalDateTime time)
  - List<Hotel> findByYearBetween(int from, int to)
  - LessThan, IsNull, Containing, In 등 활용
- findAll(): 모두 조회

### 주의

- findBy 메서드를 남용하지 말 것, 파라미터가 3~4개 이상이면 네이밍 규칙으로 외려 헷갈림
- 검색 조건이 단순하지 않으면 @Query, SQL, Specification/QueryDSL 사용

## Spring Data JPA 03 정렬 페이징 @QUery

### 정렬 1

- find 메서드에 OrderBy 붙임
  - OrderBy 뒤에 프로퍼티명 + Asc/Desc
  - 여러 프로퍼티 지정 가능

### 정렬 2

- Sort 타입 사용
- `List<User> findByNameLike(String keyword, Sort sort)`

```java
Sort sort1 = Sort.by(Sort.Order.asc("name"));
List<User> users1 = userRepository.findByNameLike("이름%", sort1);
// order by u.name asc
        
Sort sort2 = Sort.by(Sort.Order.asc("name"), Sort.Order.desc("email"));
List<User> users2 = userRepository.findByNameLike("이름%", sort2);
// order by u.name asc, email desc
```

### 페이징

- Pageable/PageRequest 사용
- `List<User> findByNameLike(String keyword, Pageable pageable`

```java
// page는 0부터 시작
// 한 페이지에 10개 기준으로 두 번째 페이지 조회
Pageable pageable = PageRequest.ofSize(10).withPage(1);
List<User> user3 = userRepository.findByNameLike("이름%", pageable);

Sort sort3 = Sort.by(Sort.Order.asc("name"), Sort.Order.desc("email"));
Pagable pageable = PageRequest.ofSize(10).withPage(1).withSort(sort3);
List<User> users3 = userRepository.findByNameLike("이름%", pageable);
```

### 페이징 조회 결과 Page로 구하기

- Page 타입: 페이징 처리에 필요한 값을 함께 제공
  - 전체 페이지 개수, 전체 개수 등
  - findTop/findFirst, findTopN/findFirstN 같은 메서드도 사용 가능
- Pageable을 사용하는 메서드의 리턴 타입을 Page로 지정하면 됨
- `Page<User> findByEmailLike(String keyword, Pageable pageable)`

```java
Pageable pageable = PageRequest.ofSize(10).withPage(0).withSort(sort);
Page<User> page = userRepository.findByEmailLike("email%", pageable);
long totalElements = page.getTotalElements(); // 조건에 해당하는 전체 개수
int totalPages = page.getTotalPages(); // 전체 페이지 개수
List<User> content = page.getContent(); // 현재 페이지 결과 목록
int size = page.getSize(); // 페이지 크기
int pageNumber = page.getNumber(); // 현재 페이지
int numberOfElements = page.getNumberOfElements(); // content의 개수
```

### @Query

- 메서드 명명 규칙이 아닌 JPQL을 직접 사용
  - 메서드 이름이 간결해짐

```java
@Query("select u from User u where u.createDate > :since order by u.createDate desc")
List<User> findRecentUsers(@Param("since") LocalDateTime since);

@Query("select u from User u where u.createDate > :since")
List<User> findRecentUsers(@Param("since") LocalDateTime since, Sort sort);

@Query("select u from User u where u.createDate > :since")
Page<User> findRecentUsers(@Param("since") LocalDateTime since, Pageable pageable);
```
