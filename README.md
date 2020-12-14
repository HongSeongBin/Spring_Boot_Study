# Spring_Boot_Study

## 최종정리

#### 프로젝트 생성

: 스프링 부트 스타터(start.spring.io) 를 이용하면 손쉽게 환경을 셋팅할 수 있어.
 
 Gradle로 생성할지 Maven으로 생성할지는 본인의 자유이나 요즈음 Gradle을 자주 사용한다고 함.
 
 dependency에 추가하고자 하는 라이브러리들을 찾아서 추가해주면 돼.
 
 <br/>
 
 
 
 #### lombok
 
 : lombok을 이용하면 기존에 생성해야했던 코드들에 대해 어노테이셔만 사용해줌으로써 사용에 편리성을 제공받을 수 있어
 
   @Getter, @Setter : 흔히 클래스의 멤버변수는 private으로 선언한 후 getter와 setter함수를 정의하여 주는데 이때 lombok을 이용하여 클래스 상단에 어노테이션을 넣어주면 따로 getter, setter 구현할 필요가 없어.
   
 <br/>
 
 
 
 #### IntelliJ Graddle 대신에 자바 직접 실행
 
 : preferences -> build,execution,deployment -> build tools -> gradle -> build and run using: -> IntelliJ IDEA -> Run tests using: Gradle -> IntelliJ IDEA
 
  최근 IntelliJ 버전은 Gradle로 실행하는 것이 기본설정인데 위 처럼 설정을 해주면 자바로 바로 실행해서 실행속도가 더 빠르다고 함.
  
  <br/>
  
  
 
 #### H2 Database
 
 : 처음 사용해봤는데 간단하게 혼자서 테스트하는 용도나 토이프로젝트에서 사용하기에 매우 좋은 것 같음. 적은 용량에 웹 콘솔을 이용할 수 있는 장점이 있네.
 
 <br/>
 
 
 
 #### hibernate 설정
 
 : 일반적으로 jpa는 크게 인터페이스라 생각하면 되고 세세한 구현은 hibernate라는 녀석이 한데. jpa를 이용하면 쿼리를 직접 날리지 않아도 되자나. 내부에서 알아서 만들어서 날려주니까!!
  
  하지만 프로젝트를 진행하다보면 이를 눈으로 확인하고 싶을 때가 많지.
  
  -> application.yml -> jpa : properties : hibernate: show_sql : true (혹은 format_sql : true)로 설정!! format_sql은 쿼리 뿐 아니라 sql 쿼리에 들어가는 인자까지 보여준데.
  
  <br/>
  
  
  
  #### 전체적인 구조
  
  : 음.. 지금까지 공부한 범위 내에서는 크게 controller, service, domain, repository 로 이루어진 구조가 큰 구조였어. controller는 느끼기에 라우팅을 위한 용도 같았고 repository는 database에 직접적으로 접근하는 로직, domain은 entity, service가 실제 서비스 기능구현 담당인 느낌이었고 해당 부분들은 가각 @Controller, @Service, @Entity, @Repository 어노테이션을 이용해서 spring boot가 관리할 수 있게 해줄 수 있었어
  
  <br/>
  
  
  
  #### 설계시 주의 사항
  
  : 도메인 모델을 설계하다보면 다대다관계가 발생할 수 있는데 이는 테이블 설계 단계에서 하나의 테이블을 더 생성해주어 일대다, 다대일 관계로 풀어서 설계해주어야해


   양방향 관계일 경우 주인을 정해주어야 하는데 일반적으로 외래키가 있는 녀석을 연관관계의 주인으로 정하는 것이 좋데!! 연관관계의 주인은 단순이 외래키를 누가 관리하냐의 문제이지 비즈니스상 우위에 있다고 주인으로 정하면 안돼.
   
   <br/>
   
   
   
  
 #### Entity 설계시
 
 : pk로 잡을 녀석의 선언 위에 @Id를 해주면 돼. 참고로 @GeneratedValue를 이용할경우 알아서 pk 값을 생성해서 넣어주고 @Column(name = "ooo") 형태로 칼럼네임도 내가 지정해 줄 수 있어.
 
<br/>



#### 값 타입

: 값 타입은 변경 불가능하게 설계해야 해. @Embeddable 을 클래스 상단에 붙임과 동시에 주의해야할 것은 생성자에서 값을 모두 초기화해서 변경 불가능한 클래스를 만드는거야. 이때 기본 생성자의 경우 protected으로 내용없는 녀석을 만들어둬야해. public 도 상관없으나 protected가 조금 더 안전한 코드래.

<br/>



#### 지연로딩

: 모든 연관관계는 지연로딩(LAZY)으로 설정하는게 좋데!! 즉시로딩

<br/>

#### 설계->코드

설계했던 내용들을 바탕으로 코드구현 진행.

* Member : Member class (회원관련 domain)
* Order : Order class (주문관련 domain) , OrderStatus enum (주문상태를 나타내기 위한 enum)
* Delivery : Delivery class (배송관련 domain), DeliveryStatus enum (배송상태를 나타내기 위한 enum)
* OrderItem : OrderItem class (주문상품관련 domain)
* Item : Item abstrct class (상품관련 domain) , Book Album Movie class 들이 상속하는 형태
* Category : Category class (카테고리관련 domain)

<br/>



#### Entity간의 관계 표현

: JPA를 이용하면 쉽게 Entity간의 관계를 구현할 수 있어. 

  how?  ->  annotation 형태로 제공
  
  * @OneToMany : 1 대 다 관계
  * @OneToOne : 1 대 1 관계
  * @ManyToOne : 다 대 1 관계
  * @ManyToMany : 다 대 다 관계 ( 실무에서 해당 annotation은 사용하는 걸 추천하지 않는데... why? -> 다 대 다 관계일 경우 Spring Boot에서 내부적으로 중간 다리 역할을 해주는 녀석을 만들어주는데 이때 그 내부의 값들은 프로그래머가 더이상 수정을 할 수가 없어. 예를들어 중간다리 역할을 하는 녀석에게 시간같은 데이터를 추가하고 싶더라도 그게 안되게 되는거지.. 그래서 ManyToMany 즉 다대다 관계는 설계할 때 중간다리 역할을 해주는 녀석까지 직접 설계하고 ManyToMany annotaiton을 안쓰는게 좋데)
  
 <br/>
 
 
 
 #### Entity 속 primary key 관련 anootation
 
 * @Id : primary key를 위한 변수에 해당 annotation을 넣어주면 돼
 * @GeneratedValue : 스프링부트에서 자동으로 키 값을 생성해서 넣어주게하는 annotation
 
<br/>



#### lombok

: 스프링부트에서 개발과정에서 편리함을 제공해주기위해 존재하는 라이브러리.
  @Getter , @Setter 등의 annotation을 이용해 getter , setter 함수를 직접 구현하지 않아도 되게 할 뿐더러 @RequiredArgsConstructor 를 통해 final로 생성된 변수에 대한 생성자 자동 생성 등 많은 편리함을 제공해줘
  
<br/>



#### @Transactional

: @Transactional(readOnly = true) 에서 readOnly = true를 넣어주는 이유는?? -> 일반적으로 default 옵션은 readOnly = false야. 그러나 조회처럼 데이터를 변경하는 일이 없을경우 readOnly=true 옵션을 이용하면 내부적으로 복잡한 과정을 몇몇 생략할 수 있어 성능상의 향상을 기대할 수 있어.

그리고 테스트 할 때 해당 annotation을 넣어주면 반복 가능한 테스트를 지원해줘. 즉 각각의 테스트를 실행할 때 마다 트랜젝션을 시작하고 테스트가 끝나면 트랙잭션을 강제로 롤백해줘!

<br/>



#### Test시 주의사항

: 일반적인 메소드들을 위한 단위테스트가 아닌 전체적으로 스프링부트를 통한 통합테스트를 진행하고 싶을 경우 해당 Test class에 @SpringBootTest annotation 을 넣어서 진행해주면 돼. 그리고 given, when, then 즉 주어진 것과, 우엇을 할때, 그래서 확인하고 싶은 것 이런식으로 나누어서 코드를 작성하면 후에 관리하기 편해진데!!
 
<br/>



#### 생성자 함수를 만들 때 주의사항

: 만약 내부적으로 프로그래머가 원하는 생성자를 만들어서 사용하고 또 해당 생성자만을 이용하게 하고싶다면 해당 생성자 함수를 정의 한뒤 기본 생성자를 protected 로 해놓으면 돼!

 그럴 경우 다른 패키지에서 new A()이런식으로 사용하게되는 것을 막을 수 있는거지!
 
<br/>



#### 예외사항 처리

: 예외사항을 처리할 때 내가 직접 정의한 exception을 이용하면 좀 더 그때그때 상황에 맞게 사용할 수 있네!! 이렇게 하자

<br/>



#### 동적 쿼리

: 동적쿼리를 JPQL로 짜러면 힘들어 -> JPA Critenria라는 것이 있는데 이를 이용하면 조금 더 쉽게 가능하나 코드를 봤을 때 쿼리가 직관적으로 보이진 않아

 -> Querydsl이란 것을 사용하면 된다는데 이거는 나중에 시간나면 따로 공부하자. 내용이 많은거 같애.

<br/>

#### sl4j

: log관련해서 사용할 수 있는 여러 라이브러리들 중 추천

<br/>


#### devtools

: build.gradle에 devtools을 추가하면 개발하는데 편리한 많은 기능들을 누릴 수 있따 (후에 공부해서 내용 추가할 것)

<br/>



#### notempty

: 이 부분은 @NotEmpty 어노테이션이 내가 사용하는 환경에서는 안잡히는데 버전 문제인지 한번 알아보자
  
 용도는 그 값이 비어있으면 안될 때를 처리해주는 로직을 스프링에게 맞기는 것.
 
<br/>



#### 개발 주의

* Entity에는 핵심 비지니스로직만 있고 화면을 위한 로직은 없는게 조금 더 바람직한 스타일이야.
화면쪽은 화면에 맞는 api나 form객체나 dto사용해야해.
* api를 개발할 때에는 Entity를 바로 보내주면 안돼!! 
* 어떤 객체의 값을 넣을 때 set을 자주 사용하여 값을 넣는 거 보다는 static 메소드 형태로 create하는 녀석을 따로 만들어주고 이녀석을 통해 한번에 해주는게 코드가 더 깔끔하고 조아.


