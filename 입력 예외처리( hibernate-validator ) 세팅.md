# hibernate-validator
  - 의존성
    ~~~
        <!-- https://mvnrepository.com/artifact/javax.validation/validation-api -->
    <dependency>
        <groupId>javax.validation</groupId>
        <artifactId>validation-api</artifactId>
        <version>2.0.1.Final</version>
    </dependency>

      <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-validator -->
      <dependency>
          <groupId>org.hibernate</groupId>
          <artifactId>hibernate-validator</artifactId>
          <version>5.4.3.Final</version>
      </dependency>
    ~~~
    
  - test.html
  ~~~
  ...
  
      <form method="post" th:object="${loginRequest}">
      <div th:each="err:${#fields.globalErrors()}" th:text="${err}"></div>
      <dl>
        <dt th:text=#{login.id}></dt>
        <dd>
          <input type="text" name="memId" th:field="*{memId}" >
          <span th:each="err:${#fields.errors('memId')}" th:text="${err}"></span>
         </dd>
      </dl>
      <dl>
        <dt th:text=#{login.pw}></dt>
        <dd>
          <input type="password" name="memPw" th:field="*{memPw}">
          <span th:each="err:${#fields.errors('memPw')}" th:text="${err}"></span>
        </dd>
      </dl>
      <button th:text=#{login}></button>
      </form>
      
  ...
  ~~~
  
  - LoginRequest.java
  
  ~~~
    package models.request;

    import org.hibernate.validator.constraints.NotBlank;

    public class LoginRequest {
      @NotBlank
      private String memId;
      @NotBlank
      private String memPw;

      ...

    }

  ~~~
  
  - TestController.java
  ~~~
	@GetMapping("/test")
	public String test(Model model) {
		model.addAttribute("loginRequest", new LoginRequest());
		model.addAttribute("addCss", new String[] {"ddd","sss","fff"});
		model.addAttribute("addJs", new String[] {"aaaa","bbb","ccc"});
		return "test";
	}
  
	@PostMapping("/test")
	public String testPs(@Valid LoginRequest request,Errors errors,Model model) {
		
		System.out.println(request);
		
		if (errors.hasErrors()) {
			errors.reject("testError");
			return "test";
		}
		
		return "test2";
	}
	
  ~~~
  
  - test.properties
  ~~~
  ...
  
    test=메세지 소스 테스트중
    test.tt=테스트2

    login=로그인
    login.id=아이디
    login.pw=패스워드
    testError=오류를 수정하세요
  
  ...
  ~~~
  
  
# Error 처리 (org.springframework.validation.Errors)

- #fields.globalErrors
  - errors.reject("에러코드")
  - errors.reject("에러코드","없을시 출력될 문자")

- #fields.errors
  - errors.rejectValue("필드이름","에러코드")
  - errors.rejectValue("필드이름","에러코드","없을시 출력될 문자")
