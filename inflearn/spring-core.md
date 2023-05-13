# 스프링 핵심 원리


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