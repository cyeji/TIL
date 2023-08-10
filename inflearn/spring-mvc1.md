## 웹 어플리케이션 이해

### 웹 서버 (Web Server)
- HTTP 기반으로 동작
- 정적 리소스 제공, 기타 부가기능
- 정적(파일) HTML,CSS, JS, 이미지, 영상
- Niginx, APACHE

### 웹 어플리케이션 서버(WAS - WebApplication Server)
- HTTP 기반으로 동작
- 웹 서버기능 + (정적 리소스 제공 기능)
- 프로그램 코드를 실행해서 애플리케이션 로직 수행
  - 동적 HTML, HTTP API(JSON)
  - 서블릿 , JSP, 스프링 MVC
  - 톰캣, Jetty, Undertow


## 서블릿(Servlet)

자바에서 제공해주는 HTTP통신 기술

###  특징

```java

@WebServlet(name="helloServlet", urlPatterns="/hello")
public class HelloServlet extends HttpServlet{

  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response){
    // 애플리케이션 로직
  }
 
}
```

- urlPatterns(/hello)의 URL이 호출되면 서블릿 코드가 실행
- HTTP 요청 정보를 편리하게 사용할 수 있는 HttpServletRequest
- HTTP 응답 정보를 편리하게 제공할 수 있는 HttpServletResponse
- 개발자는 HTTP 스펙을 편리하게 사용

### 서블릿 컨테이너

- 톰캣처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너라고 함
- 서블릿 컨테이너는 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기 관리
- 서블릿 객체는 싱글톤으로 관리
  - 고객의 요청이 올 때마다 계속 객체를 생성하는 것은 비효율
  - 최초 로딩 시점에 서블릿 객체를 미리 만들어두고 재활용
  - 모든 고객 요청을 동일한 서블릿 객체 인스턴스에 접근
  - 공유 변수 사용 주의
  - 서블릿 컨테이너 종료시 함께 종료
- JSP도 서블릿으로 변환되어서 사용
- 동시 요청을 위한 멀티 쓰레드 처리 지원

