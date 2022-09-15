---
layout: post
title: SpringBoot에서 JPA 사용하기
# subtitle: A awesome static site generator.
author: woong99
categories: spring
#

tags: springboot
sidebar: []
---

## JPA란?

- Java Persistence API (자바 ORM 기술에 대한 API 표준 명세)로 ORM을 사용하기 위한 인터페이스들을 모아둔 것이다.
- 자바 애플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스이다.
- ORM에 대한 자바 API 규격이며 대표적으로 Hibernate가 JPA를 구현한 구현체이다.

## ORM이란?

- Object-Relational Mapping (객체와 관계형데이터베이스 매핑, 객체와 DB의 테이블이 매핑을 이루는 것)
- 한마디로, 객체가 테이블이 되도록 매핑시켜주는 프레임워크이다.
- 프로그램의 복잡도를 줄이고 자바 객체와 쿼리를 분리할 수 있으며, 트랜잭션 처리나 기타 데이터베이스 관련 작업을 좀 더 편리하게 처리할 수 있다.
- SQL Query가 아닌 직관적인 코드(메소드)로 데이터를 조작할 수 있다.

## Hibernate란?

- JPA를 사용하기 위해서 JPA를 구현한 ORM 프레임워크 중 하나이다.
- 자바를 위한 오픈소스 ORM 프레임워크를 제공한다.
- Hibernate는 JPA 명세의 구현체이다. `javax.persistence.EntityManager`와 같은 JPA의 인터페이스를 직접 구현한 라이브러리이다.

---

## 의존성 추가

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    runtimeOnly 'com.h2database:h2'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

JPA 사용을 위한 `Spring Data JPA`와 테스트 용으로 사용할 인 메모리 데이터베이스를 위한 `H2 Database`와 `Lombok`을 추가한다.

## application.yaml 추가

```yaml
# H2
spring:
  h2:
    console:
      enabled: true
      path: /h2

  # Datasource
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:test
    username: sa
    password:
  jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: tre
```

애플리케이션을 실행하고, `localhost:포트번호/h2` 주소로 접속하면, H2 데이터베이스를 사용할 수 있다.

## Member 엔티티

```java
@Getter
@ToString
@Entity
@Table
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;


    @Setter
    @Column(nullable = false)
    private String name;

    @Setter
    @Column(nullable = false)
    private int age;

    protected Member() {
    }

    public Member(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public static Member of(String name, int age) {
        return new Member(name, age);
    }
}

```

- `@Entity`는 Member 클래스가 엔티티임을 명시함
- `@Table`은 Member 엔티티와 맵핑할 테이블명을 지정한다. 클래스명과 일치한다면 테이블명을 굳이 명시하지 않아도 된다.
- `@Id`는 객체의 Primary Key를 의미한다. JPA는 이 id를 통해 객체를 구분한다.
- `@GeneratedValue(strategy = GenerationType.AUTO)`는 JPA에서 엔티티의 Primary Key를 생성하여 주는 기능이다. AUTO가 기본값으로 DB 방언에 맞춰 자동으로 설정하여 준다.
- 엔티티에서는 무분별한 Setter의 사용은 좋지 않다고 들었다. id의 값은 고유해야 하는데 함부로 Set 메소드를 사용하면 안된다 생각하여 별도로 Setter 메소드를 사용하였다.
- `@Column`은 객체 필드와 DB 테이블 컬럼을 맵핑한다.

### @Column의 속성

- name: 맵핑할 테이블의 컬럼 이름을 지정 (기본 값 : "")
- insertable: 엔티티 저장 시 선언된 필드도 같이 저장 (기본 값: true)
- updatable: 엔티티 수정 시 이 필드를 함께 수정 (기본 값 : true)
- table: 지정한 필드를 다른 테이블에 맵핑 (기본 값 : "")
- nullable: NULL을 허용할지, 허용하지 않을지 결정 (기본 값 : true )
- unique: 제약조건을 걸 때 사용 (기본 값 : false)
- columnDefinition: DB 컬럼 정보를 직접적으로 지정할 때 사용 (기본 값 : "")
- length: varchar의 길이를 조정 (기본 값 : 255)
- precision, scale: BigInteger, BigDemical 타입에서 사용, 각각 소수점 포함 자리수, 소수의 자리수를 의미 (기본 값 : 0)

## JpaRepository를 상속

```java
@Repository
public interface MemberRepository extends JpaRepository<Member, Long> {
}
```

JpaRepository 인터페이스를 상속받음으로써, 기본적인 CRUD 메소드를 사용할 수 있다.

## Service 작성

```Java
@Service
public class MemberService {

    @Autowired
    private MemberRepository memberRepository;

    public Member save(Member member) {
        memberRepository.save(member);
        return member;
    }

    public List<Member> saveAll(List<Member> members) {
        memberRepository.saveAll(members);
        return members;
    }

    public void deleteById(Long id) {
        memberRepository.deleteById(id);
    }

    public void deleteAll() {
        memberRepository.deleteAll();
    }

    public Member findById(Long id) {
        Optional<Member> member = memberRepository.findById(id);
        return member.orElse(null);
    }

    public List<Member> findAll() {
        return memberRepository.findAll();
    }
}
```

## Controller 작성

```java

@RestController
@RequestMapping("/api")
public class MemberController {

    private final MemberService memberService;

    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    @PostMapping("/save")
    public ResponseEntity<Member> saveMember(@RequestBody Member member) {
        return new ResponseEntity<>(memberService.save(member), HttpStatus.OK);
    }

    @PostMapping("/save-all")
    public ResponseEntity<List<Member>> saveAllMembers(@RequestBody List<Member> members) {
        return new ResponseEntity<>(memberService.saveAll(members), HttpStatus.OK);
    }

    @GetMapping()
    public ResponseEntity<Member> findMember(@RequestParam Long id) {
        if (memberService.findById(id) == null) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }
        return new ResponseEntity<>(memberService.findById(id), HttpStatus.OK);
    }

    @GetMapping("/find-all")
    public ResponseEntity<List<Member>> findMembers() {
        return new ResponseEntity<>(memberService.findAll(), HttpStatus.OK);
    }


    @DeleteMapping()
    public ResponseEntity<Void> deleteMember(@RequestParam Long id) {
        memberService.deleteById(id);
        return new ResponseEntity<>(HttpStatus.OK);
    }

    @DeleteMapping("/all")
    public ResponseEntity<Void> deleteAllMember() {
        memberService.deleteAll();
        return new ResponseEntity<>(HttpStatus.OK);
    }

}
```
