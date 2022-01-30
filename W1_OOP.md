1. 개체지향이란?
    - 절차적 VS 개체지향
        
        절차적 프로그래밍은 - 하나의 데이터를 가공하는게 목적
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2d1db02-6c94-4df3-a6fc-b1f00dd9d143/Untitled.png)
        
        - 절차지향적 프로그래밍은 ‘돈’이라는 자료가 어떻게 가공이 되고 어떻게 판별이 되어 프로그램이 실행되는지가 중요
        - 객체지향적 프로그래밍은 각각의 고유의 기능 (절차)을 가지게 되고 이를 통해서 개체들간의 상호작용으로 결과를 도출하는것이 중요하다.
        
    - 개체의 핵심
        - 인터페이스와 클래스와의 관계
            - 추상 클래스 → 추상클래스를 상속받음으로서 각각의 유니크한 클래스를 만들 수 있음 (강한 결합)
            
            Abstract classes are useful when you need a class for the purpose of inheritance and polymorphism, but it makes no sense to instantiate the class itself, only its subclasses. They are commonly used when you want to define a template for a group of subclasses that share some common implementation code, but you also want to guarantee that the objects of the superclass cannot be created.
            
            For instance, let's say you need to create Dog, Cat, Hamster and Fish objects. They possess similar properties like color, size, and number of legs as well as behavior so you create an Animal superclass. However, what color is an Animal? How many legs does an Animal object have? In this case, it doesn't make much sense to instantiate an object of type Animal but rather only its subclasses.
            
            Abstract classes also have the added benefit in polymorphism–allowing you to use the (abstract) superclass's type as a method argument or a return type. If for example you had a PetOwner class with a train() method you can define it as taking in an object of type Animal e.g. train(Animal a) as opposed to creating a method for every subtype of Animal.
            
            - 인터페이스 → 기능을 정의한 명세서  (보다 정형화된 프로그램의 개발이 가능하다)
            
            ```kotlin
            interface Pay {
                fun payment(x: Int, y: Int): Int
            }
            
            class CardPay : Pay {
            
                override fun payment(credit: Int, payment: Int): Int {
                    return credit - payment
                }
            }
            
            class CashPay : Pay {
            
                override fun payment(cash: Int, payment: Int): Int {
                    return cash - payment
                }
            }
            
            class Person() {
                lateinit var paymentType: Pay
                var Credit = 0
                var Cash = 0
            
                fun deposit(credit: Int, cash: Int) {
                    Credit = credit
                    Cash = cash
                }
            
                fun payment(amount: Int, payType: String) {
                    if (payType == "Card") {
                        paymentType = CardPay()
                        Credit = paymentType.payment(Credit, amount)
                    } else if (payType == "Cash") {
                        paymentType = CashPay()
                        Cash = paymentType.payment(Cash, amount)
                    }
                }
            }
            
            fun main() {
                val person = Person()
                person.deposit(15000, 10000)
                person.payment(5000, "Cash")
                println(person.Cash)
            }
            ```
            
            - 인터페이스 상속을 통해서
            
        - 개체간에 보내는 메세지란? 기능을 동작하도록 요청을 보내는것
        - Mom 객체에서 baby 객체에게 메시지를 보내는 구조
        
        ```java
        public abstract class Person {
            public abstract void sleep();
        }
        ```
        
        ```java
        public class Baby extends Person {
            @Override
            public void sleep() {
                System.out.println("애기가 잔다.");
            }
        }
        ```
        
        ```java
        public class Mom extends Person {
            @Override
            public void babySleep() {
               Baby baby = new Baby();
               baby.sleep();
            }
        }
        ```
        
        엄마 객체가 애기 객체가 자도록 메세지를 보내고 애기 객체는 이를 실행해 결과를 돌려주는것.
        
    
    - 개체의 책임과 크기
        - 단일기능을 제공하는 것이 중요하다.
        - 개체가 가지는 책임이 클수록 절차적 지향 프로그래밍에 가까워진다
    - 의존
        - 클래스가 다른 클래스를 상속하면 의존관계가 형성되며, 기존 클래스의 코드가 달라지면 상속을 받은 클래스의 코드도 바뀌어야 한다
    - 캡슐화
        - Tell, Don’t ask 법칙
            - 어떤 자료를 다시 주는것이 아니라 기능을 실행해 주는것
            
            - 데이터를 Raw상태로 가져와서 수정하는 행위
            
            ```kotlin
            // member.getExpiryDate() -> 만료 일자 데이터를 가져옴
            if (member.getExpiryDate() ! null &&
                member.getExpiryDate().getDate < System.currentTimeMillis()) {
              // 만료 되었을 때 처리
            }
            
            ```
            
            - 데이터는 모르겠고, 기능을 실행해 주는 행위
            
            ```kotlin
            if (member.isExpired()) {
              // 만료 되었을 때 처리
            }
            ```
            
            - 데이터를 직접적으로 사용하는 코드는 데이터의 변화에 직접적인 영향을 받기 때문에, 요구사항의 변화로 인해 데이터의 구조나 쓰임새가 변경되면 이로 인해 데이터를 사용하는 코드들도 연쇄적으로 수정해 주어야 한다
        
        - 데메테르의 법칙
            - 디미터의 법칙은 다른 객체가 어떠한 자료를 갖고 있는지 속사정을 몰라야 한다는 것을 의미하며, 이러한 이유로 Don’t Talk to Strangers(낯선 이에게 말하지 마라) 또는 Principle of least knowledge(최소 지식 원칙) 으로도 알려져 있다.
            - 즉, 다른객체에게 메시지를 보내서 어떠한 기능을 동작하게 하는것이 중요한 법칙이라는 것.
            
            - 안좋은 예
            
            ```kotlin
            @Getter public class User { private String email; private String name; private Address address; }
            @Getter public class Address { private String region; private String details; }
            
            ```
            
            ```kotlin
            @Service public class NotificationService { 
            public void sendMessageForSeoulUser(final User user) { 
                if("서울".equals(user.getAddress().getRegion())) { 
                   sendNotification(user); 
                   } 
                } 
            }
            ```
            
            - 좋은 예
            
            ```kotlin
            public class Address { 
            private String region; 
            private String details; 
            
            public boolean isSeoulRegion() { 
                return "서울".equals(region); 
                } 
            } 
            
            public class User { 
            private String email; 
            private String name; 
            private Address address; 
            
            public boolean isSeoulUser() { 
                return address.isSeoulRegion(); 
                } 
            }
            
            ```
            
            ```kotlin
            @Service public class NotificationService { 
                public void sendMessageForSeoulUser(final User user) { 
                    if(user.isSeoulUser()) { sendNotification(user); 
                    } 
                } 
            }
            
            출처: https://mangkyu.tistory.com/147 [MangKyu's Diary]
