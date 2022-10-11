# HandlerInterceptor (org.springframework.web.servlet.HandlerInterceptor) 세팅
  - TestHandlerInterceptor
  ~~~
package common.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class TestHandlerInterceptor implements HandlerInterceptor {
	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
		
		//응답전
		
	}
	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		
		//응답후
		
	}
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		
		// 컨트롤러를 실행한 후 (응답을 할지(true) 안할지(false) )
		
		return true;
	}

}


  ~~~
  
  - MvcConfig
  ~~~
  ...
    @Bean
    public TestHandlerInterceptor testHandlerInterceptor() {
      return new TestHandlerInterceptor();
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
      registry.addInterceptor(testHandlerInterceptor()).addPathPatterns("/**");
    }

  ...
  ~~~
  
