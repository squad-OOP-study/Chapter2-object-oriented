# 개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴

## Chapter2. 객체 지향

---

## 1. 절차지향과 객체지향
- 절차지향이라는 용어 때문에 객체지향은 절차적이지 않다고 생각할 수 있지만, 객체지향도 마찬가지로 절차적으로 프로그램을 수행한다. 
- 단지 절차지향은 프로그램의 순서를 먼저 결정하고 데이터와 함수를 설계하는 반면, 객체지향은 데이터와 프로시저를 하나로 묶는 객체라는 단위를 먼저 설계한 후 프로그램의 순서를 결정한다는 차이가 있다.
- [잘 정리된 글](https://m.blog.naver.com/atalanta16/220249264429)

### 절차지향
- 프로시저(Procedure) 단위로 프로그램이 구성되며, 데이터를 중심으로 한 패러다임
- 여러 프로시저가 동일한 데이터를 공유

- **장점**
    
    1. 구현이 비교적 쉬움
        - 데이터를 중심으로 하기때문에 함수를 데이터에 맞게 구현함
    2. 규모가 크지 않은 프로젝트에서 유용함

- **단점**
    
    1. 요구사항이 변경되면 수정되어야 할 프로시저가 증가함
        - 프로시저가 수정되면 동일한 데이터를 사용하는 모든 프로시저가 수정되어야 함
    2. 같은 데이터를 프로시저들이 서로 다른 의미로 사용될 수 있음
        - 서로 다른 기능을 하는 프로시저들이 같은 변수를 사용할 위험성이 있음
    3. 프로그램의 규모가 커질수록 코드의 수정과 기능의 추가에 대한 비용이 큼
    
### 객체지향
- 데이터 및 데이터와 관련된 프로시저를 객체(Object) 단위로 구성하고 관리하는 패러다임
- 자신의 객체 데이터에만 접근할 수 있음

- **장점**
    
    1. 요구사항이 변경되더라도 해당 객체만 수정하면 되므로 유지보수가 쉬움
    2. 유지보수가 쉽기 때문에 대형 프로젝트에서 유용함
    3. 재사용성이 높음 (상속, 다형성 등)

- **단점**

    1. 객체 지향적으로 설계하고 개발하는데 비용이 큼
        - 객체끼리의 관계와 재사용성을 높이기 위한 상속, 다형성 등의 고민
    2. 실행 속도가 절차지향에 비해 느림
        - 객체를 할당하고 접근하는 과정 등에 대한 CPU의 연산
    3. 메모리 양의 증가
        - 변수가 아닌 객체 단위로 구성되기 때문

---

## 2. 객체(Object)
- 객체는 클래스의 인스턴스이며, 메모리에 할당되기 전까지 객체는 존재하지 않는다.
- **객체는 데이터와 그 데이터를 조작하는 프로시저(오퍼레이션, 메서드, 함수)로 구성**된다.
    - 오퍼레이션(Operation) : 보통 객체가 제공하는 기능을 오퍼레이션(Operation)이라고 부른다.
- 객체가 어떤 데이터를 내부에 가지고 있고, 어떻게 구현되어있는지에 대한것은 사용하는 입장에서는 알 필요가 없다. 

> (TMI) C언어에도 객체(Object)라는 개념이 있다고 한다. [참고1](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=han95173&logNo=220765393422) [참고2](http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf)<br><br>
>3.15<br>
>1 object<br>
>region of data storage in the execution environment, the contents of which can represent values.

### 인터페이스와 클래스
- 오퍼레이션의 사용법은 일반적으로 세 개로 구성되며 이를 `시그니처(Signature)`라고 부른다.
    1. 기능 식별 이름
    2. 파라미터 및 파라미터 타입
    3. 기능 실행 결과 값
- 객체가 제공하는 모든 오퍼레이션 집합을 객체의 `인터페이스(interface)`라고 부른다.
    - 객체를 사용하기 위한 명세서나 규칙, 프로토타입
- 오퍼레이션을 구현하는데 필요한 구현이 포함된 것을 `클래스(class)`라고 부른다.
- 위에서 언급한 `인터페이스`와 `클래스`는 자바에서 사용하는 것을 말한것이 아니라 객체 지향에서 사용하는 개념상의 `인터페이스`와 `클래스`를 말한다.

### 메시지
- 객체는 메시지를 통해 서로 통신한다. 즉, A객체에서 B객체의 오퍼레이션 실행을 요청하는 것을 `메시지를 보낸다`라고 한다.
- `메소드를 호출한다`라고 부르기도 한다.
- 이런 개념을 이용한 `메시징 패턴(Messaging Pattern)`도 존재한다.
    - 이 메시징 패턴은 다른 디자인 패턴을 구현하는데 사용된다.
- [잘 정리된 글](https://panonit.com/blog/overview-message-passing-object-oriented-programming)

---

## 3. 객체의 책임과 크기
- 객체는 자신만의 책임을 가지고 있다. 책임이라는 것은 하나의 객체는 하나의 일만 한다는 것인데, 한 객체에 기능이 많아지게 되면 결국 데이터를 공유해서 사용하게 되고 이는 결국 절차 지향적인 구조를 갖게 된다.
- 한 객체가 여러가지 일을 하게 되면 기능을 변경하는 것이 어렵게 된다.
- 이러한 원칙을 `객체 지향 설계 원칙 5가지 (SOLID)`중 하나인 `단일 책임 원칙(SRP)`라고 한다.


---

**[출처 및 참고사이트]**
- 개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴
- https://ko.wikipedia.org/wiki/%EC%A0%88%EC%B0%A8%EC%A0%81_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D
- http://www.incodom.kr/%EC%A0%88%EC%B0%A8_%EC%A7%80%ED%96%A5
- http://www.incodom.kr/%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5
- https://m.blog.naver.com/atalanta16/220249264429
- https://ko.wikipedia.org/wiki/%EA%B0%9D%EC%B2%B4_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)
- https://panonit.com/blog/overview-message-passing-object-oriented-programming
