

## MVC(Model View Controller)
- 비즈니스 규칙을 표현하는 도메인 모델(Model)과 프레젠테이션을 표현하는 View를 분리하기 위해 양측 사이에 컨트롤러를 배치하도록 설계한 디자인 패턴
- spring web MVC 모듈의 전체적인 구조는 이 패턴을 중심으로 만들어졌다.

![MVC모델이미지](https://i.imgur.com/pm2EhxT.png)

## Spring DispatcherServlet

- Spring은 MVC 패턴을 구현하기 위해 다른 웹 MVC 프레임워크처럼 Front Controller 패턴을 사용하며 프레임워크의 여러 가지 기능을 제공하는 servlet 중심으로 설계되어 있다. 
- 스프링에서 Front Controller 역할을 하는 것이 바로 DispatcherServlet이다. 
- 스프링의 DispatcherServlet은 단순히 Front Controller 기능만 하는 게 아니라 스프링 IoC 컨테이너와 완전히 통합되어 스프링이 가진 모든 다른 기능을 사용할 수 있게 한다. 
- Spring DispatcherServlet은 Spring MVC의 핵심 요소이다. Spring MVC의 웹 요청 Life Cycle을 주관한다.

### FrontController 패턴이란 모든 Web Application에 대한 요청들을 FrontController로 받고 그 요청을 Controller로 분배해주는 패턴을 의미한다.
### 서블릿이란 클라이언트 요청을 처리하고 그 결과를 다시 클라이언트에게 전송하는 Servlet 클래스의 구현 규칙을 지킨 자바 프로그램이다.

![dispatcherServlet](https://i.imgur.com/7xlCWY9.png)

![디스패처동작순서](https://i.imgur.com/eWjZX8j.png)


## 스프링 부트에서 DispatcherServlet설정
- WebApplicationInitializer구현

```
public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
        ServletRegistration.Dynamic registration = container.addServlet("dispatcher", new DispatcherServlet());
        registration.setLoadOnStartup(1);
        registration.addMapping("/*");
    }

}
```

## spatcherServlet, ContextLoaderListener

- Servlet container(톰켓3.0+) 이상이 실행되어야 한다.
- 그 후 ServletContainerInitializer를 구현한 SpringServletContainerInitializer를 실행시킨다.
- SringServletContainerInitializer는 WebApplicationInitializer를 implements한 class를 찾아내어 onStartup 메소드를 실행시킨다.

### ContextLoaderListener에 의해서 만들어지는 Root WebApplicationContext
- 서비스계층과 DAO를 포함한 웹 환경에 독립전인 빈을 담아둔다.

### DispatcherServlet에 의해서 만들어지는 WebApplicationContext
- DispatcherServlet이 직접 사용하는 컨트롤러 포함한 웹 관련 빈을 등록하는데 사용
- 위와 context와 parent-child applicationContext관계를 맺는다

![context관계](https://i.imgur.com/IUf4orm.png)


## 스프링 MVC Request 생명주기 

![생명주기](https://i.imgur.com/G8y0Pqa.jpg)


---

### 출처
[springMvc구조](https://minwan1.github.io/2018/05/28/2018-05-28-spring-mvc/)


