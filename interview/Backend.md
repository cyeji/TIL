# 백엔드 면접 질문 대비

## Q. Spring MVC의 구조와 처리 과정을 설명해 보세요.

<details>
<summary>[핵심 답변]</summary>
<div markdown="1">

    1. DispatcherServlet이 어플리케이션으로 들어오는 모든 Request를 받아서 HandlerAdapter에게 요청 처리를 위임합니다. </br>
    2. HandlerAdapter는 HandlerMapping에 Mapping된 Controller에게 전달합니다. </br>
    3. Controller는 요청을 처리한 뒤 응답값을 다시 DispatcherServlet으로 전달합니다. 응답값은 ModelAndView가 될 수도 있고, </br>
        @ResponseBody어노테이션이 있다면 Json형태가 될 수도 있습니다. </br>
    4. ViewResolver에게 어떤 View를 반환할 것인지 요청한 다음, 해당 View 객체를 화면으로 전달하게 됩니다. </br>

</div>
</details>

## Q. Spring Security란?

<details>
<summary>[핵심 답변]</summary>
<div markdown="1">
    
    Spring Security란 자바 어플리케이션에서 인증과 권한 부여, 일반적인 공격에 대한 보호 기능을 제공하는 프레임워크입니다.
    Spring Security를 사용하면 어플리케이션의 보안 관련 기능을 자체적으로 구현할 필요없이 쉽고 안전하게 구현할 수 있고 
    다양한 요구사항을 충족하기 위해서 쉽게 확장할 수도 있습니다.

</div>
<summary>[꼬꼬무]</summary>
<div markdown="1">
    
    - 스프링 시큐리티의 전체적인 인증
    
</div>
</details>

## Q. Spring 프레임워크에서 사용되는 디자인 패턴

<details>
<summary>[핵심 답변]</summary>
<div markdown="1">
    
    싱글톤패턴 : 싱글톤 범위의 빈 </br>
    팩토리패턴 : 빈 팩토리 클래스 </br>
    프로토타입 패턴 : 프로토타입 범위의 빈 </br>
    어댑터 패턴 : SpringWeb 및 SpringMVC </br>
    프록시 패턴 : Spring Asepct지향 프로그래밍 지원 </br>
    탬플릿 메소드 패턴 : JDBCTemplate, HibernateTemplate </br>
    프론트 컨트롤러 : SpringMVC DispatcherServlet </br>
    
</div>
</details>

## Q. Spring 트랜잭션 격리 수준

<details>
<summary>[핵심 답변]</summary>
<div markdown="1">
    
1. `DEFAULT`: 기본적으로 데이터베이스에서 지정한 격리 수준을 사용합니다. 대부분의 데이터베이스는 `READ_COMMITTED` 격리 수준을 기본값으로 사용합니다.
2. `READ_UNCOMMITTED`: 커밋되지 않은 데이터를 다른 트랜잭션에서도 읽을 수 있습니다.
3. `READ_COMMITTED`: 커밋된 데이터만 읽을 수 있습니다. 다른 트랜잭션이 해당 데이터를 변경하면 해당 트랜잭션이 커밋되기 전까지 해당 데이터를 읽을 수 없습니다.
4. `REPEATABLE_READ`: 동일한 쿼리를 실행하면 항상 같은 결과가 나오도록 보장합니다. 다른 트랜잭션이 해당 데이터를 변경해도 해당 트랜잭션이 커밋되기 전까지 해당 데이터를 읽을 수 없습니다.
5. `SERIALIZABLE`: 가장 엄격한 격리 수준으로, 모든 트랜잭션을 직렬화하여 실행합니다. 즉, 동시에 실행되는 여러 트랜잭션 간에 서로 영향을 미치지 않습니다.
    
</div>
</details>

