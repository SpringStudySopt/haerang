
## 목차


[1. Spring MVC](#MVC)

[2. 프로젝트 및 폴더 구조](#스프링-프로젝트-및-폴더-구조)


## MVC
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


## 스프링 프로젝트 및 폴더 구조

- src/main/java
    - 자바(.java) 파일이 모여있는 곳입니다. 
    - 패키지로 잘 분리해서 자바 클래스를 생성해 사용하면 됩니다. 
    - 스프링에서 이미 MVC 패턴의 서블릿 구조를 잡아주기 때문에 따로 서블릿을 만들 필요 없이 스프링 구조에 맞춰 클래스 파일들을 작성해주면 됩니다.

![src/main/java](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcuRWqV%2FbtqCy5chosD%2FhRK9DAUKKyKo3SFi3Wz7P0%2Fimg.png)
     
    
- src/main/resources

![src/main/resources](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FdjsyqQ%2FbtqCxPVhMIf%2FXyRXIpgXAKgW8WifkDu3U0%2Fimg.png)
   
- src/test
    - 위 두 폴더와 같은 역할이지만 테스트를 위한 자바 코드와 리소스를 보관하는 곳
    
![src/test](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcAfubF%2FbtqCvyzKXYt%2FTW7G3mG1ESpSrcb0UPsyEk%2Fimg.png)
    
    
- Maven Dependencies
    - 메이븐에서 자동으로 관리해주는 라이브러리 폴더
    - "pom.xml"에 작성된 라이브러리들을 자동으로 다운 받아 관리해줍니다. 
    - 빌드툴을 사용함으로써 개발자가 직접 관리해주지 않아도 되는 영역
    
![Maven Dependencies](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FJHcVF%2FbtqCrPP3xFi%2F058GLMTI7vvAKbFhExIab1%2Fimg.png)
    
    
### 기타 구조 참고

[스프링(Spring) 프로젝트의 폴더 구조](https://codevang.tistory.com/240)

---

### 출처
[springMvc구조](https://minwan1.github.io/2018/05/28/2018-05-28-spring-mvc/)

[프로젝트및폴더구조](https://codevang.tistory.com/240)
   


