
### 목차

> 1 JPA
>> 1.1 JPA란
>> 1.2 JPA사용 이유 
>> 1.3 객체와 관계형데이터베이스 차이

> 2 영속성 컨텍스트
>> 2.1 영속성 컨텍스트란
>> 2.2 엔티티 생명주기
>> 2.3영속성 컨텍스트 장점

## JPA란?
JPA가 무엇인지 정의하기 전에 한 가지 개념에 대해 알아야한다.

> ORM(Object Relational Mapping) 이란?
 RDB테이블을 객체지향적으로 사용하기 위한 기술
 객체와 RDB의 테이블을 매핑하여 자바 프로그램 코드상에서 RDB를 객체지향적으로 쓸 수 있게 고려한 기술
 객체는 객체대로 관계형 데이터베이스는 관계형 데이터베이스대로 설계
 ORM 프레임워크가 중간에서 매핑
 
**자바의 영속성 관리와 ORM을 위한 표준 기술이 JPA(Java Persistent API)이다.**

## JPA 사용 이유?
객체와 관계형 데이터 베이스는 패러다임이 불일치하다.
- 객체지향 프로그래밍: 추상화, 캡슐화, 정보은닉, 상속, 다형성 등 시스템의 복잡성을 제어할 수 있는 다양한 장치들을 제공한다.
- 관계형 데이터베이스: 데이터 중심으로 구조화, 집합적인 사고가 필요. 추상화, 상속, 다형성 같은 개념이 없다.


이처럼 패러다임이 불일치한 상황에서 자바 컬렉션에 저장하듯이 DB에 저장하는 방법인 JPA를 사용하게 된다.

**한마디로 객체 중심의 애플리케이션 개발이 가능하다.**



> 추가로,
스프링에서 흔히 사용하는 것으로 알고있는 JPA는, JPA를 이용하는 Spring-data-jpa프레임워크이지 JPA는 아니다.
순수 JPA는 Java진영에서 ORM으로 사용하는 인터페이스의 모음이다.
![image](https://github.com/24tngus/CS_STUDY/assets/75667075/685ade18-876a-42ba-a10b-fae2e1268409)



## 객체와 관계형 데이터베이스의 차이
### 상속
![image](https://github.com/24tngus/CS_STUDY/assets/75667075/cefffcc9-831b-4e55-9ba8-6297c23fa136)


케이스마다 조회하기 위해서는 JOIN과정이 필요하다. 하지만, 자바 컬렉션에 저장한다면?
list.get(albumId)로 해결할 수 있다.

### 연관관계
![image](https://github.com/24tngus/CS_STUDY/assets/75667075/c06a4851-e322-40de-a3fe-7db7f11c024c)

테이블에는 연관관계를 표현 하기위해서는 아래와같이 외래키를 사용해서 표현해야한다.
하지만 아래와 같이 표현하게 되면 Member객체에서 한번에 Team을 알 수 없다. 
(java같은 경우는 Member 안에 Team team클래스로 선언하면 Member을 불러와도 Team을 알 수 있음)
즉, 객체 지향의 특징을 잃어버린다.

```java
class Member {

    String id;      // MEMBER_ID 컬럼 사용
    Long teamId;    // TEAM_ID FK 컬럼 사용
    String userName;
}
```
```java
class Team {

    Long id;        // TEAM_ID PK 사용
    String name;
}
```


```java
Member member = jpa.find(Member.class, memberId);
Team team = member.getTeam();
```
객체를 조회할 때 외래키를 참조로 변환하는 일을 JPA가 처리해준다.
즉, 객체지향에 맞게 매핑을 할 수 있다.

# 영속성 컨텍스트

## 영속성 컨텍스트란?

"엔티티를 영구 저장하는 환경" 이라는 뜻으로 애플리케이션과 데이터베이스 사이에서 객체를 보관하는 가상의 데이터베이스 같은 역할을 한다. 엔티티 매니저를 통해 저장하거나 조회하면 엔티티 매니저는 영속성 컨텍스트에 엔티티를 보관하고 관리한다.
위의 그림과 같이 EntityManager Factory가 고객에 요청마다 EntityManager을 생성한다. 이 EntityManager은 DB Connection을 사용하여 DB를 사용한다

```java
em.persist(entity) //entity을 영속성 컨텍스트에 저장
```

## 엔티티의 생명주기
![image](https://github.com/24tngus/CS_STUDY/assets/75667075/393948d8-ede7-4997-aabc-8adfa6f1f5cb)

### 비영속(new/ transient): 영속성 컨텍스트와 관계가 없는 새로운 상태
```java
Member member = new Member();
member.setId(1L)
member.setName("JPA")
// JPA와 관련이 없는 그저 객체만 생성된 상태
```

### 영속(managed): 영속성 컨텍스트에 관리되는 상태
DB에 저장되는 것이 아니고 영속성 컨텍스트에서 관리되는 것이다. 영속된다고 쿼리가 나가는 것이 아니며, commit을 해야 쿼리가 실행되게 된다.

```java
//객체 생성(비영속)
Member member = new Member();
member.setId(1L)
member.setName("JPA")

EntityManager em = emf.createEntityManager();
em.getTransaction().begin();
//영속
em.persist(member)
```

### 준영속(detached): 영속성 컨텍스트에 저장되어 있다가 분리된 상태
em.detach(entity) : 특정 엔티티만 준영속상태로 바꿈
em.clear(): Entity Manager의 영속성 컨텍스트를 초기화함
em.close() : 영속성 컨텍스트 종료
```java
//비영속
Member member = new Member();
member.setId(1L);
member.setName("JPA");
tx.commit();

//영속
em.persist(member);
//준영속, 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member);
tx.commit();
```

### 삭제(removed): 삭제된 상태
실제 DB 삭제를 요청한 상태

```java
//비영속
Member member = new Member();
member.setId(1L);
member.setName("JPA");
tx.commit();

//영속
em.persist(member);
//준영속, 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member);
//삭제, 객체를 삭제한 상태
em.remove(member);
tx.commit();
```

## 영속성 컨텍스트의 장점
### 1차 캐시

![image](https://github.com/24tngus/CS_STUDY/assets/75667075/005229c7-531d-44ac-9c48-6845f7c4392e)

- 엔티티에 저장한 후 조회하게 될 경우에는 바로 DB에서 찾지 않고 영속성 컨텍스트에 있는 1차 캐시에서 조회하게 된다.
- 1차 캐시에 없는 경우에는 DB에서 조회 후 1차 캐시에 저장하고 반환하게 된다.
- 하지만, EntityManager 자체는 트렌젝션 단위로 생명주기가 이루어지기 때문에 그렇게 뚜렷한 성능 개선은 보이지 않는다.

### 동일성 보장
- Java의 Collection 비교가 가능한 것 처럼 JPA의 동일성을 보장해준다.
- 1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜젝션 격리 수준을 데이터베이스가 아닌 애플리케이션 차원에서 제공한다.
- REPEATABLE READ: DB에서 데이터를 조회할 때, 하나의 데이터에 대해서 여러번 조회하면 값이 변경되지 않은 한 같은 데이터가 나오게 된다.
```java
Member a = em.find(Member.class, "member1"); 
Member b = em.find(Member.class, "member1"); 

System.out.println(a==b) // true
```
### 트렌젝션을 지원하는 쓰기 지연
```java
try{
    Member member1 = new Member(150L, "A");
    Member member2 = new Member(160L, "B");

    em.persist(member1);
    em.persist(member2);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다
    System.out.println("====================");
    tx.commit();
//커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다

    }catch (Exception e){
        tx.rollback();
    }finally {
        em.close();

    }
```

- 1차 캐시에 넣음과 동시에 쓰기 지연 SQL 저장소에 insert쿼리를 생성해서 넣는다.
- 마찬가지로 memberB도 쿼리를 생성해서 쓰기지연에 넣는다.
- transaction.commit하는 시점에서 쓰기지연 SQL 저장소에 쿼리가 flush되면서 db쿼리가 날려진다.
**즉, 바로 쿼리가 보내지는 것이 아니라 트랜젝션 커밋 시점에 모아둔 쿼리가 나가는 것이다.**

> 하이버네이트 옵션
<property name="hibernate.jdbc.batch_size" value="10"/>
해당 옵션값만큼 모았다가 DB에 쿼리를 날릴 수 있다.
버퍼링같은 기능

## 변경 감지
![image](https://github.com/24tngus/CS_STUDY/assets/75667075/b33837fc-f059-4143-a181-2c1129ffcaa6)

- 엔티티 수정, 변경 감지, 더티 체킹
- 저장하지 않았는데 업데이트 쿼리가 날라옴
```java
//영속 엔티티 조회 
Member member = em.find(Member.class, 150L); 
//영속 엔티티 수정 
member.setName("SUJIN"); 
//커밋할 때 수정된 
tx.commit();
```

flush가 실행되면 1차캐시랑 스냅샷(최초 상태)를 비교하게 된다.
만약 Entity와 스냅샷이 다르다면 업데이트쿼리를 보내게 되며, DB에 쿼리를 보내 업데이트한다.

## 질문
### Collection이 무엇인가?
- Collection 객체는 여러 원소들을 담을 수 있는 자료구조를 뜻한다
- 배열이 가장 기본적인 자료구조이며, DTO 도한 자료를 담는 하나의 방식이라고 볼 수 있다

> 자바에서 자료구조 유현
>> 순서가 있는 목록인 List형
>> 순서가 중요하지 않은 목록인 set형
>> 먼저 들어온 것이 먼저 나가는 Queue형
>> KEY-VALUE의 형태로 저장되는 Map형

결론적으로 질문주신 Collection은 자바에서의 배열이든 다양한 원소들을 담아 구성할 수 있도록 하며, 관계형 데이터베이스는 이러한 Collection으로 표현하지 못하기에 FK를 걸어서 join을 통해 표현합니다.
이렇게 자바와 관계형DB는 다른 방식으로 표현하기 때문에 객체지향을 온전히 활용하지 못합니다.
그래서 ORM인 JPA를 사용합니다!
