# 스프링 핵심 원리

## 객체지향 특징

1. 추상화 </br>
  불필요한 세부 사항들은 제거하고 가장 본질적이고 공통적인 부분만 추출하여 표현 </br>
  ex) 추상 클래스, 인터페이스
2. 캡슐화 </br>
   서로 연관있는 속성과 기능들을 하나의 캡슐로 만들어 데이터를 외부로부터 보호하는 것 </br>
   데이터 보호 - 외부로부터 클래스에 정의된 속성과 기능들을 보호 </br>
   데이터 은닉 - 내부의 동작을 감추고 외부에는 필요한 부분만 노출 </br>
   ex) 접근 제어자
3. 상속 </br>
  기존의 클래스를 재활용하여 새로운 클래스를 작성하는 자바의 문법 요소
4. 다형성 </br>
  어떤 객체의 속성이나 기능이 상황에 따라 여러 가지 형태를 가질 수 있는 성질 </br>
  ex) 오버라이딩 , 오버로딩

## 좋은 객체지향 설계의 5가지 원칙 (SOLID)

1. SRP 단일책임원칙
  - 하나의 클래스는 하나의 책임만 가져야한다.
  - 하나의 책임이라는 것은 모호하다. 이에 대한 중요한 기준은 변경이다. 변경이 있을 떄 파급효과가 적으면 단일책임원칙을 잘 따른것
  - ex) UI 변경, 객체의 생성과 사용을 분리

2. OCP 개방폐쇄원칙
  - 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀있어야한다.
  - 다형성
  - 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능 구현
  - 지금까지 배운 역할과 구현의 분리

3. LSP 리스코프치환원칙
  - 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.

4. ISP 인터페이스 분리 원칙
  - 여러개 범용 인터페이스 보다는 하나의 인터페이스 사용
  - 인터페이스가 명확해지고 대체 가능성이 높아진다.

5. DIP 의존관계 역전 원칙
  - 구체화보다는 추상화에 의존하자
  - 구현 클래스에 의존하기 보다는 인터페이스에 의존하자


## 의존성 주입

1. 생성자 주입
2. 필드 주입
3. 수정자(setter) 주입
4. 일반메소드 주입

### 생성자 주입

- 생성자 호출시점에 딱 1번만 호출되는 것을 보장
- 불편, 필수 의존관계에 사용
- 생성자가 딱 한개 있을 때는 @Autowired 생략가능

```java

@Component
public class  OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
      this.memberRepository = memberRepository;
      this.discountPolicy = discountPolicy;
    }
    
}

```

### 필드 주입

- 코드가 간결하지만 외부에서 변경이 불가능해서 테스트하기 힘들다는 단점
- DI 프레임워크가 없으면 아무것도 할 수 없다.
- 사용하지 말자 
  - 어플리케이션의 실제 코드와 관겨없는 테스트코드
  - 스프링 설정을 목표로하는 예를들면, @Configuration 곳에서만 특별한 용도로 사용

```java

@Component
public class  OrderServiceImpl implements OrderService{

    @Autowired
    private MemberRepository memberRepository;
    
    @Autowired
    private DiscountPolicy discountPolicy;
    
}

```

### 수정자 주입

@Autowired 기본 동작은 주입할 대상이 없으면 오류가 발생합니다. 주입할 대상이 없어도 동작하게 하려면 </br>
@Autowired(required = false)로 지정하면 된다.

```java

@Component
public class OrderServiceImpl implements OrderService{
    
    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;
    
    @Autowired
    public void setMemberRepository(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    
    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy){
        this.discountPolicy = discountPolicy;
    }
    
    @Autowired
    public void setOrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy){
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    
}

```

### 일반 메소드 주입

- 한번에 여러 필드를 주입 받을 수 있다.
- 일반적으로 잘 사용하지 않음

**참고 : 의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링 빈이어야 동작한다. </br> 
스프링 빈이 아닌 클래스에서 @Autowired 적용해도 아무 동작 안함**

```java

@Component
public class OrderServiceImpl implements OrderService{
    
    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;
    
    @Autowired
    public void init(MemberRepository memberRepository, DiscountPolicy discountPolicy){
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    
}

```

### 자바빈 프로퍼티 규약 예시

참고 : 자반빈 프로퍼티, 자바에서는 과거부터 필드의 값을 직접 변경하지 않고, setXXX, getXXX라는 메서드를 통해서 값을 읽거나 </br>
수정하는 규칙을 만들었는데, 그것이 자바빈 프로퍼티 규약이다.

```java

class Data {

    private int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

}

```

## 옵션 처리

- Member는 스프링 빈이 아니다.
- setNoBean1() 은 @Autowired(required=false)이므로 호출 자체가 안된다.

```java

static class TestBean {

    // 호출 안됨
    // member가 Bean등록, member==null, exception
    // member가 Bean등록안되어있음, member==null, 빈값 호출
    @Autowired(required = false)
    public void setNoBean1(Member noBean1) {
        System.out.println("noBean1 = " + noBean1);
    }

    // NULL 호출
    @Autowired
    public void setNpBean2(@Nullable Member noBean2) {
        System.out.println("noBean2 = " + noBean2);
    }

    // Optional.empty 호출
    @Autowired
    public void setNoBean3(Optional<Member> noBean3) {
        System.out.println("noBean3 = " + noBean3);
    }
}
```

## 생성자 주입을 사용하자

**불변** </br>
- 대부분 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다. 오히려 대부분의 의존관계를 애플리케이션</br>
종료 전까지는 변하면 안된다 (불변)
- 수정자 주입 사용시, setXXX() 메소드를 public으로 열어두어야 한다.
- 누군가 실수로 변경할 수도 있고, 변경하면 안되는 메소드를 열어두는 것은 좋은 설계 방법이 아니다.
- 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없다. 따라서 불변하게 설계가능
</br>
**누락** </br>
프레임워크 없이 순수한 자바 코드를 단위 테스트 하는 경우
</br>
**final 키워드** </br>
생성자 주입을 사용하면 필드에 final 키워드를 사용할 수 있다. 그래서 생성자에서 혹시라도 값이 설정되지 않는 오류를 컴파일 시점에 막아준다.


## 빈 충돌 시 해결법

1. @Autowired로 필드명 매칭
2. @Qualifier -> @Qualifier끼리 매칭 -> 빈 이름 매칭
3. @Primary 사용

### @Autowired 필드 명 매칭

@Autowired는 타입 매칭을 시도하고, 이 때 여러 빈이 있으면 필드 이름, 파라미터 이름으로 빈 이름을 추가 매칭한다.

<br></br>

**기존코드**

```java

@Autowired
private DiscountPolicy discountPolicy;

```
</br>**필드 명을 빈 이름으로 변경**

```java

@Autowired
private DiscountPolicy rateDiscountPolicy;

```

필드 명이 rateDiscountPolicy이므로 정상 주입된다. </br>
필드 명 매칭은 먼저 타입 매칭을 시도하고 그 결과 여러 빈이 있을 때 추가로 동작하는 기능

### @Qualifier 사용

추가 구분자를 붙여주는 방법. 주입 시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다. </br>
@Qualifier 사용 시, 선언한 어노테이션 끼리 매칭, 없을 경우 빈 이름으로 매칭, 둘다 없을 경우 NoSuchBeanDefinitionException 예외 발생


- RateDiscountPolicy

```java

@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {
    
}

```

- FixDiscountPolicy

```java

@Component
@Qualifier("subDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy {

}

```

- OrderService에서 @Autowired를 통한 의존성 주입

```java

@Component
public class OrderServiceImpl implements OrderService {

  @Autowired
  public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
  }
  
}

```

### @Primary 사용

@Primary는 우선순위를 정하는 방법이다. @Autowired시에 여러 빈이 매칭되면 @Primary가 우선 권을 가진다.

```java

@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {
    
}

```

### @Primary, @Qualifier 활용

코드에서 자주 사용하는 메인 데이터베이스의 커넥션을 획득하는 스프링 빈이 있고, 코드에서 특별한 기능으로 가끔 사용하는 서브 데이터베이스의 </br>
커넥션을 획득하는 스프링 빈이 있다고 가정하자. 메인 데이터베이스를 연결하는 스프링 빈은 @Primary를 적용해서 조회하는 곳에서</br>
@Qualifier 지정 없이 편리하게 조회되고, 서브 데이터베이스 커넥션 빈을 획득할 때는 @Qualifier를 지정해서 명시적으로 획득하는 방식으로 사용하면</br>
코드를 깔끔하게 유지할 수 있다. 물론 이때 메인 데이터베이스의 스프링 빈 등록 시 @Qualifier를 지정해줘도 상관없다.

### 우선순위

@Primary는 기본값처럼 동작하는 것이고, @Qualifier는 매우 섬세하게 동작한다. </br>
이런 경우 어떤 것이 우선권을 가져갈까? </br>
스프링은 자동보다는 수동, 넓은 범위의 선택권 보다는 좁은 범위의 선택권이 우선순위가 높다. </br>
따라서 @Qualifier가 우선순위가 높다.

## 빈 생명주기 콜백 시작

데이터 커넥션 풀이나, 네트워크 소켓처럼 애플리케이션 시작 시점에 필요한 연결을 미리 해두고, 애플리케이션 종료 시점에 연결을 모두 종료하는 </br>
작업을 진행하려면, 객체의 초기화와 종료 작업이 필요하다.
<br></br>

### 스프링 빈의 이벤트 사이클
스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멸전 콜백 -> 스프링 종료
</br>

- 초기화 콜백 : 빈이 생성되고, 빈의 의존관례 주입이 완료된 후 호출
- 소멸전 콜백 : 빈이 소멸되기 직전에 호출

**참고 : 객체의 생성과 초기화를 분리하자** </br>
생성자는 필수 정보(파라미터)를 받고, 메모리를 할당해서 객체를 생성하는 책임을 가진다. 반면에 초기화는 이렇게 생성된 값들을 활용해서 </br>
외부 커넥션을 연결하는 등 무거운 동작을 수행한다. </br>
따라서 생성자 안에서 무거운 초기화 작업을 함께 하는 것 보다는 객체를 생성하는 부분과 초기화 하는 부분을 명확하게 나눈 것이 유지보수 관점에서 좋다.</br>
이 물론 초기화 작업이 내부 값들만 변경하는 정도로 단순한 경우에는 생성자에서 한번에 다 처리하는게 더 나을 수도 있다.


<br></br>

**스프링은 크게 3가지 방법으로 빈 생명주기 콜백을 지원** </br>
- 인터페이스(InitializingBean, DisposableBean)
- 설정 정보에 초기화 메서드, 종료 메서드 지정
- @PostConstruct, @PreDestory 어노테이션 지원
