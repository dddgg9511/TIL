# Filter / Intercepter 차이

# Filter

<aside>
💡 J2EE 표준 스펙 기능으로 디스패치 서블릿(Dispatcher servlet)에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가작업을 처리할 수 있는 기능을 제공

</aside>

- 디스패처 서블렛(Dispatcher servlet)은 스프링의 가장 앞단에 존재하는 front Controller 이므로 filter는 스프링 범위 밖에서 처리
- 스프링 컨테이너가 아닌 tomcat같은 웹 컨테이너에 의해 관리
![filter](https://user-images.githubusercontent.com/39615281/176852579-d3857ee9-48e5-4b24-8fa3-e13879b6d125.png)


## 필터의 메소드

### init()

> init 메소드는 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드이다. 웹 컨테이너가 1회 init 메소드를 호출하여 필터 객체를 초기화 하면 이후의 요청들은 doFilter를 통해 처리된다.
> 

### doFilter()

> doFilter 메소드는 url-pattern에 맞는 모든 HTTP 요청이 디스프처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드이다. doFilter의 파라미터로는 FilterChain이 있는데, FilterChain의 doFilter를 통해 다음 대상으로 요청을 전달하게 된다. chain.doFilter() 전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다.
> 

### destroy

> destroy 메소드는 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드이다. 이는 웹 컨테이너에 의해 1번 호출되며 이후에는 이제 doFilter에 의해 처리되지 않는다.
> 

# Intercepter

<aside>
💡 J2EE 포준 스펙인 필터(Filter)와 달리 Spring이 제공하는 기술로써, 디스패처 서블릿(Dispatcher Servlet)이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제

</aside>

- 웹 컨테이너에서 동작하는 필터와 달리 스프링 컨텍스트에서 동작 한다.

디스패처 서블릿은 핸들러 매핑을 통해 적절한 컨트롤러를 찾도록 요청하는데, 그 결고로 실행 체인을 돌려준다.

그래서 이 실행 체인은 1개 이상의 인터셉터가 등록되어 있다면 순차적으로 인터셉터들을 거쳐 컨트롤러가 실행하도록 하고 인터셉터가 없다면 바로 컨트롤러를 실행한다.
![interceptor](https://user-images.githubusercontent.com/39615281/176852664-e83f5423-d0cf-4779-8863-7226491b5f0f.png)

## 인터셉터의 메소드

### preHandle

> preHandle 메소드는 컨트롤러가 호출되기 전에 실행된다. 그렇기 때문에 컨트롤러 이전에 처리해야하는 전처리 작업이나 요청 정보를 가공하거나 추가하는 경우에 사용할 수 있다.
> 
- 3번째 파라미터인 handler 파라미터는 핸들러 매핑이 찾아준 컨트롤러 빈에 매핑되는 handlerMethod라는 새로운 타입의 객체로써, @RequestMapping이 붙은 메소드의 정ㅇ보를 추상화한 객체이다.
- preHandle의 반환 타입은 boolean인데 반환값이 true이면 다음 단계로 진행이 되지만 false 라면 작업을 중단하여 이후의 작업(다음 인터셉터 또는 컨트롤러)은 진행되지 않는다.

### postHandle

> postHandle 메소드는 컨트롤러를 호출된 후에 실행된다. 그렇기 때문에 컨트롤러 이후에 처리해야 하는 후처리 작업이 있을 때 사용할 수 있다.
> 
- postHandle 메소드에는 컨트롤러가 반혼하는 ModelAndView 타입의 정보가 제공되는데, 최근에는 json 형태로 데이터를 제공하는 RestAPI 기반의 컨트롤러(@RestController)를 만들면서 자주 사용되지는 않는다.

### afterCompletion

> 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 된료된 후에 실행된다. 요청 처리 중에 사용한 리소스를 반활할 때 사용하기에 적합하다.
> 

# 필터 vs 인터셉터 차이 및 용도

## 필터 vs 인터셉터 차이
![filter_interceptor](https://user-images.githubusercontent.com/39615281/176852754-b0d81ac9-8662-4a07-bef0-715925bafd48.png)

### Request/Response 객체 조작 가능 여부

- **Filter**
    
    > 필터는 Request와 Response를 조작할 수 있지만 인터셉터는 조작할 수 없다. 여기서 조작한다는 것은 내부 상태를 변경한다는 것이 아니라 다른 객체로 바꿔친다는 의미이다.
    > 
    - 필터가 다음 필터를 호출하기 이ㅜ해서는 필터 체이닝(다음 필터 호출)을 해주어야 한다. 그리고 이때 Requset/Response 객체를 넘겨주므로 우리가 원하는 Request/Response 객체를 넣어 줄 수 있다.
    
    ```java
    public MyFilter implements Filter{
    	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain){
    		// 개발자가 다른 request와 respnose를 넣어 줄 수 있음.
    		chain.dofilter(reqeust, response);
    	}
    }
    ```
    
- **Interceptor**
    
    > 디스패처 서블릿이 여러 인터셉터 목록을 가지고 있고, for문으로 순차적으로 실행시킨다, 그리고 true를 반환하면 다음 인터셉터가 실행되거나 컨트롤러로 요청이 전달되며, false가 반환되면 요청이 중단된다. 그러므로 우리가 다른 Request/Response 객체를 넘겨줄 수 없다
    > 
    
    ```java
    public class MyInterceptor implements HandlerInterceptor{
    	default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler){
    		//Request/Response를 교체할 수 없고 boolean 값만 반환할 수 있다.
    		return true;
    	}
    }
    ```
    

## 필터와 인터셉터의 용도 및 예시

### 필터

- 공통된 보안 및 인증/인가 관련 작업
- 모든 요청에 대한 로깅 또는 감사
- 이미지/데이터 압축 및 문자열 인코딩
- spring과 분리되어야 하는 기능

> 필터에서는 기본적으로 스프링과 무관하게 전역적으로 처리해야 하는 작얿들을 처리할 수 있다.
> 

### 인터셉터

- 세부적인 보안 및 인증/인가 공통 작업
- API 호출에 대한 로깅 또는 감사
- Controller로 넘겨주는 정보(데이터)의 가공

> 클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업
>
