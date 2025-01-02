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

