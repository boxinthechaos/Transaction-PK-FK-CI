# Transaction-PK-FK-CI

# Transaction이란
Transaction(트랜잭션)은 쪼갤 수 없는 여 작업들을 논리적으로 묶은 최소단위로 묶은 것이다

# Transaction의 특징
1. 원자성(Atomicity)
  > 트랜잭션의 연산은 DB에 모두 반영 되던지 또는 모두 반영 되지 않아야 한다 만약 하나라도 실패한다면 앞서 성공한 것들을 원상복구 시켜나야 한다

2. 일관성(Consistency)
  > 트랜잭션의 작업 처리 결과는 항상 일관성 있어야 한다

3. 독립성(Isolation)
  > 어떤 트랜잭션이라도 다른 트랜잭션 작업에 끼어들 수 없다

4. 지속성(Durability)
  > 트랜잭션 작업이 완료된다면 결과는 영구적이어야 한다

# 트랜잭션이 원자성을 보장하는 방법
트랜잭션은 로직을 수행하고 난 뒤 모든 로직이 성공적으로 수행 되었다면 모든 결과를 DB에 commit 한다 만약 하나라도 실패 했다면 모든 작업을 rollback한다

# Spring의 Transaction 동작 방식에 대한 기본 개념
Spring에서는 @Transactional 어노테이션을 통해서 쉽고 깔끔하게 트랜잭션 처리를 해줄 수 있다. 그런데 `@Transactional`이 어떻게 동작하여 트랜잭션을 보장하는지 궁금해졌다

## Proxy
스프링은 기본적으로 트랜잭션을 Proxy를 통해서 처리한다
![image](https://github.com/user-attachments/assets/a851b4f8-29c7-43ae-a414-5d9372300aab)
실제로 PostService의 addPost 메서드를 요청하면, 동작 전의 코드로는 위와 같이 보이지만 실제 런타임에서는 아래와 같이 작동한다
![image](https://github.com/user-attachments/assets/a5142da4-9a10-4605-b70b-ea6620587d81)
스프링이 어플리케이션이 동작할 때, Transaction 전후 처리를 하는 Proxy를 동적으로 생성해준다

# 기본 키 (PK, Primary Key)
테이블 내에서 유일하게 존재하는 값의 조합을 설정해서, 중복된 데이터가 테이블에 삽입되는 것을 방지해주고 해당 필드가 NOT NULL 과 UNIQUE 제약 조건의 특징을 모두 가지게 된다

# 외래 키 (FK, Foreign Key)
다른 기본 키를 참조하는 속성 or 속성들의 집합이다
> - 참조 관계의 기본 키와 같은 속성을 가진다
> - 데이터가 새롭게 추가될 때, 외래 키에 해당하는 값이 외래 키가 참조하는 테이블에 존재하는지를 확인한다
> - 하나의 테이블을 다른 테이블에 의존하게 만든 (두 테이블을 연결하는 다리 역할)
>   - 외래 키가 포함된 테이블 = 자식 테이블
>   - 외래 키 값을 제공하는 테이블 = 부모 테이블

데이터의 무결성을 보장
> - 하나의 테이블에서 중복된 데이터가 삽입되는 것을 방지한다
> - 외래 키를 가지는 테이블이 참조하는 기준 테이블의 열은 반드시 PK, UNIQUE 제약조건이 설정되어 있어야 한다

DB 는 무조건 다(N)쪽이 외래 키를 갖는다
> - 외래 키가 있는 쪽이 연관관계의 주인이다
>     - 연관관계의 주인 O : 두 객체 사이에서 '조회, 저장, 수정, 삭제' 가능하다
>     - 연관관계의 주인 X : '조회'만 가능

# PK & FK
## 1) 1:1 연관 관계 (일대일)
![image](https://github.com/user-attachments/assets/29345d35-04bc-4ed5-abc0-1c32ba54228f)
각 전화번호가 단 한 명의 유저와 연결되어 있고, 그 반대도 동일하다면, Users 테이블과 Phonebook 테이블은 1:1 관계

## 2) 1:N 연관 관계 (일대다)
![image](https://github.com/user-attachments/assets/c49f423e-f6b7-4887-a99b-e97f0d66b188)
한 쌍의 부모(1) 中 여러 명의 자식(N)들이 어떤 부모에 속해 있는지 표현해야 한다.
따라서, 부모 테이블의 PK 를 자식 테이블에 FK 로 집어 넣어 관계를 표현한다.
(즉, 부모 테이블(1)에서는 내 자식들이 누구인지 정보를 넣을 필요가 없고,
자식 테이블(N)에서만 각각의 자식들이 자신의 부모 정보(FK)를 넣음으로써 관계를 표현)

## 3) N:M 연관 관계 (다대다)
![image](https://github.com/user-attachments/assets/794d6526-73bb-499f-b67c-12c08598d1b8)
N:M 관계는 관계를 가진 양쪽 엔티티 모두에서 1:N 관계를 가지는 것을 말한다. (서로가 서로를 1:N 관계로 보고 있는 것)
  - 학원:학생 = 1:N
  - 학생:학원 = 1:M
  - 따라서, 학원:학생 = N:M
![image](https://github.com/user-attachments/assets/40d25d62-08a6-4c00-b50c-51eac90b2ba3)
N:M 관계는 서로가 서로를 1:N 관계, 1:M 관계로 갖고 있기 때문에, 서로의 PK 를 자신의 외래키 컬럼으로 갖고 있으면 된다.
일반적으로 N:M 관계는 두 테이블의 대표키를 컬럼으로 갖는 또 다른 테이블(중간 테이블)을 생성해서 관리한다.


# PK키와 FK키를 스프링 부트에 연결하기

## 연관 관 정의 규칙
연관 관계를 매핑할 때 중요한 것은 크게 3가지 있다
- 방향 : 양방향, 단방향
- 연관 관계의 주인 : 양방향일때 연관 관계에서 관리를 주체한다
- 다중성 : 다대일(N:1), 일대다(1:N), 일대일(1:1), 다대다(N:N)

# 1.JPA로 테이블 만들기
 Java에서의 엔티티 설계
 ![image](https://github.com/user-attachments/assets/376f68ad-0418-4429-8aac-9bc323a0e1e8)
 
 ---
 ✔️ Member.java
 ```
@Entity
@Getter @Setter
public class Member {   

	@Id @GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;
    
	private String name;
	private String city;
	private String street;
	private String zipcode;	
    
}
```
- @Id - 현재 엔티의 기본 키임을 나타낸다
- @Entity - JPA에서 관리한다
- @GeneratedValue - KEY값을 DBSM에 시퀀스처럼 자동증가 퀄럼으로 만들어 준다
- @Column(name = "MEMBER_ID") - 컬럼명을 새로 만들어 준다.
---

✔️ Order.java
```
@Entity							
@Table(name = "ORDERS")	
@Getter @Setter
public class Order {
	
	@Id @GeneratedValue
	@Column(name = "ORDER_ID")
	private Long id;
	
	@Column(name = "MEMBER_ID")		// Member.java에 MEMBER_ID와 FK관계
	private Long memberId;
	
	private LocalDateTime orderDate;
	private String status;
	
}
```
---

✔️ OrderItem.java
```
@Entity
@Getter @Setter
public class OrderItem {	
	
	@Id @GeneratedValue
	@Column(name = "ORDER_ITEM_ID")
	private Long id;
	
	@Column(name = "ORDER_ID")		// Order.java에 ORDER_ID와 FK관계
	private Long orderId;	
	
	@Column(name = "ITEM_ID")		// Item.java에 ITEM_ID와 FK관계
	private Long itemId;
	
	private int orderPrice;
	private int count;
	
}
```
---

✔️ Item.java
```@Entity
@Getter @Setter
public class Item {
	
	@Id @GeneratedValue
	@Column(name = "ITEM_ID")
	private Long id;
    
	private String name;
	private int price;
	private int stockQuantity;
}
```
---

✔️ JpaMain.java <- JPA 실행을 위한 main클래스
```
public class JpaMain {

	public static void main(String[] args) {

		EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
		EntityManager em = emf.createEntityManager();
		EntityTransaction tx = em.getTransaction();
		tx.begin();
		
		try {
        	tx.commit();
		} catch (Exception e) {
			tx.rollback();
		}finally {
			em.close();
			emf.close();
		}
	}
```
---

✔️ 클래스에서 화면 우클릭 > Run As > Java Application으로 실행하기
![image](https://github.com/user-attachments/assets/19e97ce6-d078-4027-8e8f-b187b96146fe)

---
✔️ DB - Table 생성
![image](https://github.com/user-attachments/assets/7f0f40d4-75fa-42bc-8e4e-a929318799d7)

---


