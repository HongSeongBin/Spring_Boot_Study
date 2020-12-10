# Spring_Boot_Study


### 1210 Study
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

