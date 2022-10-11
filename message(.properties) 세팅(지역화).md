# 지역화 세팅
  - MvcConfig
    ~~~
    package spring.config;

    ...

    @Configuration
    @EnableWebMvc
    @Import(DBConfig.class)
    public class MvcConfig implements WebMvcConfigurer {

      ...

        @Bean
      public MessageSource messageSource() {
        ResourceBundleMessageSource ms = new ResourceBundleMessageSource();
        ms.addBasenames("messages.common", "messages.errors","messages.member");
        ms.setDefaultEncoding("UTF-8");
        return ms;
      }

      ...

    }

    ~~~
    
  - ex) messages.common.properties
    ~~~
      member.memId=아이디
      member.memNm=회원명
      member.memPw=비밀번호
      member.memPwRe=비밀번호 확인
      member.email=이메일
      member.mobile=휴대전화번호
      member.regDt=회원가입일
      member.agreeTerms=약관동의
      member.agreeTermsYes=회원가입 약관에 동의합니다
      member.join=회원가입
      member.reset=다시쓰기
      member.logout=로그아웃

      mypage=마이페이지

      member.login=로그인
      member.saveId=아이디 저장


    ~~~

    
    ~~~
    package spring.config;

    ...

    @Configuration
    @EnableWebMvc
    @Import(DBConfig.class)
    public class MvcConfig implements WebMvcConfigurer {
    	
      @Value("${environment}")
      private String environment;


      @Value("${fileUploadPath}")
      private String fileUploadPath;
	
      ...

      @Bean
      public static PropertySourcesPlaceholderConfigurer properties() {
        PropertySourcesPlaceholderConfigurer conf = new PropertySourcesPlaceholderConfigurer();
        conf.setLocations(new ClassPathResource("application.properties"));

        return conf;
      }

      ...

    }

    ~~~
    
  - ex) application.properties
    ~~~
      # 운영 모드  - dev - 개발중, prod - 서비스중
      environment=dev

      fileUploadPath=C:\\uploads
    ~~~
