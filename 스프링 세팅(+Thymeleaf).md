# pom.xml

  - Spring Context
  
    ~~~
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
    <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-context</artifactId>
      <version>5.3.23</version>
    </dependency>

    ~~~
    
  - Spring Web MVC
    ~~~
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.23</version>
    </dependency>

    ~~~
  - Java Servlet API
    ~~~
    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>

    ~~~
  - JavaServer Pages(TM) API
    ~~~
    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.3</version>
        <scope>provided</scope>
    </dependency>

    ~~~

# Thymeleaf 
  - Thymeleaf Spring5
    ~~~
    <!-- https://mvnrepository.com/artifact/org.thymeleaf/thymeleaf-spring5 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.15.RELEASE</version>
    </dependency>

    ~~~
    
  - Thymeleaf Layout Dialect
    ~~~
    <!-- https://mvnrepository.com/artifact/nz.net.ultraq.thymeleaf/thymeleaf-layout-dialect -->
    <dependency>
        <groupId>nz.net.ultraq.thymeleaf</groupId>
        <artifactId>thymeleaf-layout-dialect</artifactId>
        <version>3.1.0</version>
    </dependency>

    ~~~
    
   - Thymeleaf Extras Java8time
   
      ~~~
      <!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-java8time -->
      <dependency>
          <groupId>org.thymeleaf.extras</groupId>
          <artifactId>thymeleaf-extras-java8time</artifactId>
          <version>3.0.4.RELEASE</version>
      </dependency>

      ~~~
    
# web.xml
 
  ~~~
    
          <?xml version="1.0" encoding="UTF-8"?>
      <web-app version="4.0"
        xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee                       http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
        <servlet>
          <servlet-name>dispatcher</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <init-param>
            <param-name>contextClass</param-name>
            <param-value>
              org.springframework.web.context.support.AnnotationConfigWebApplicationContext
            </param-value>
          </init-param>
          <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>
              spring.config.ControllerConfig
              spring.config.ModelConfig
              spring.config.MvcConfig
            </param-value>
          </init-param>
          <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
          <servlet-name>dispatcher</servlet-name>
          <url-pattern>/</url-pattern>
        </servlet-mapping>
        <filter>
          <filter-name>encodingFilter</filter-name>
          <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
          </filter-class>
          <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
          </init-param>
        </filter>
        <filter-mapping>
          <filter-name>encodingFilter</filter-name>
          <url-pattern>/*</url-pattern>
        </filter-mapping>

      </web-app>
      
   ~~~
    
# spring.config
  - ControllerConfig.java
    ~~~
      package spring.config;

      import org.springframework.context.annotation.ComponentScan;
      import org.springframework.context.annotation.Configuration;

      @Configuration
      @ComponentScan(basePackages = {"controllers.test"})
      public class ControllerConfig {

      }
    ~~~
  - ModelConfig.java
    ~~~
      @Configuration
      public class ModelConfig {

      }

    ~~~
  
  - MvcConfig.java  
    ~~~
      package spring.config;

      import org.springframework.context.annotation.Configuration;
      import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
      import org.springframework.web.servlet.config.annotation.EnableWebMvc;
      import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
      import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
      import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

      @Configuration
      @EnableWebMvc
      public class MvcConfig implements WebMvcConfigurer{

        @Override
        public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
          configurer.enable();
        }

        @Override
        public void addResourceHandlers(ResourceHandlerRegistry registry) {
          registry.addResourceHandler("/**").addResourceLocations("classpath:/static/");
        }

        @Override
        public void configureViewResolvers(ViewResolverRegistry registry) {
          registry.jsp("/WEB-INF/view/",".jsp");
        }

      }

    ~~~
    
    # Thymeleaf 사용할 경우
    
    ~~~
      @Override
        public void configureViewResolvers(ViewResolverRegistry registry) {
          registry.jsp("/WEB-INF/view/",".jsp");
        }

    ~~~
    
    ∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇∇
    
    ~~~
          @Override
      public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.viewResolver(thymeleafViewResolver());
      }

      @Bean
      public ThymeleafViewResolver thymeleafViewResolver() {
        ThymeleafViewResolver thymeleafViewResolver=new ThymeleafViewResolver();
        thymeleafViewResolver.setCharacterEncoding("utf-8");
        thymeleafViewResolver.setContentType("text/html");
        thymeleafViewResolver.setTemplateEngine(springTemplateEngine());


        return thymeleafViewResolver;
      }

      @Bean
      public SpringTemplateEngine springTemplateEngine() {
        SpringTemplateEngine templateEngine=new SpringTemplateEngine();
        templateEngine.addTemplateResolver(springResourceTemplateResolver());
        templateEngine.setEnableSpringELCompiler(true);

        templateEngine.addDialect(new Java8TimeDialect());
        templateEngine.addDialect(new LayoutDialect());

        return templateEngine;
      }

      @Bean
      public SpringResourceTemplateResolver springResourceTemplateResolver() {
        SpringResourceTemplateResolver resolver=new SpringResourceTemplateResolver();
        resolver.setSuffix(".html");
        resolver.setPrefix("/WEB-INF/view/");
        resolver.setApplicationContext(applicationContext);
        resolver.setCacheable(false);		
        return resolver;
      }
    ~~~

  
    
    
    
    
    
