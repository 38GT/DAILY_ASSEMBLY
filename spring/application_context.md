<h1 style="font-size: 48px;">Application Context</h1>

모든 빈(Bean) 객체는 어플리케이션 컨텍스트에 등록되어 관리된다.


### ApplicationContext: 스프링의 핵심 컨테이너로 빈 생성, 의존성 주입 및 애플리케이션 전역 리소스를 관리.
### WebApplicationContext: 웹 애플리케이션을 위한 특화된 컨텍스트로, 컨트롤러와 같은 웹 관련 빈을 관리.
### Root ApplicationContext: 웹 애플리케이션 전체에서 공유되는 공통 빈(예: 데이터베이스, 서비스)을 관리. 서블릿 컨텍스트의 부모 컨텍스트 역할.
### ServletContext: 서블릿 컨테이너에서 제공하는 컨텍스트로, 스프링의 WebApplicationContext와 통합되어 서블릿 관련 정보를 관리.
### DispatcherServlet: 스프링 MVC에서 모든 요청을 받아 처리하고, 적절한 컨트롤러로 전달하는 프론트 컨트롤러.
