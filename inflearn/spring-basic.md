# 스프링 입문

![IMG_C73CAA29DA51-1.jpeg](..%2F..%2F..%2F..%2F..%2FDownloads%2FIMG_C73CAA29DA51-1.jpeg)

- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버 (ViewResolver)가 화면을 찾아서 처리한다.
  - 스프링부트 템플릿 엔진 기본 viewName 매핑
  - resource:templates/{viewName}.html

![IMG_147B5681CB35-1.jpeg](..%2F..%2F..%2F..%2F..%2FDownloads%2FIMG_147B5681CB35-1.jpeg)

- @ResponseBody를 사용
  - Http의 Body의 문자 내용을 직접 반환 viewResolver 대신에 HttpMessageConverter가 동작
  - 기본 문자 처리 : StringHttpMessageConverter
  - 기본 객체 처리 : MappingJackson2HttpMessageConverter

참고 : 클라이언트의 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해서 HttpMessageConverter가 선택된다.

## 스프링 빈 등록하는 2가지 방법
1. 컴포넌트 스캔과 자동 의존관계 설정
2. 자바 코드로 직접 스프링 빈 등록하기

### 스프링 빈 등록

- 회원 서비스

```java

@Service
public class MemberService{
    
    private final MemberRepository memberRepository;
    
    @Autowired
    public MemberService(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    
}

```


#### 컴포넌트 스캔 원리
- @Component 어노테이션이 있으면 스프링 빈으로 자동 등록된다.
- @Controller 컨트롤러가 스프링 빈으로 자동등록된 이유도 컴포넌트 스캔 때문

```java

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any (or empty String otherwise)
	 */
	@AliasFor(annotation = Component.class)
	String value() default "";

}


```

- @Component를 포함하는 다음 어노테이션도 스프링 빈으로 자동 등록된다.
  - @Controller, @Service, @Repository


**참고: 스프링은 스프링 컨테이너에 스프링 빈을 등록할때, 기본으로 싱글톤으로 등록한다(유일하게 하나만 등록해서 공유) 따라서 같은 스프링 빈이면 모두 같은 인스턴스이다 </br>
설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.**

#### DI 방식 (3가지)

1. 필드 주입

```java

@Service
public class MemberService{
    
    @Autowired
    private MemberRepository memberRepository;
    
}

```

2. setter 주입

```java

@Service
public class MemberService{
    
    private MemberRepository memberRepository;
    
    @Autowired
    public void setMemberRepository(MeemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    
}

```

3. 생성자 주입

```java

@Service
public class MemberService{
    
    private final MemberRepository memberRepository;
    
    public MemberService(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    
}

```

의존관계(Runtime) 실행 중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장 (순환참조 방지)

### 자바코드로 직접 스프링 빈 등록하기

@Service, @Repository, @Autowired 어노테이션 사용 안함

```java


@Configuration
public class SpringConfig{
    
    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }
    
    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
    
}

```