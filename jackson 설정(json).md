# jackson 설정(json)

  - 의존성
    ~~~
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.13.3</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.datatype</groupId>
      <artifactId>jackson-datatype-jsr310</artifactId>
        <version>2.13.3</version>
    </dependency>
    ~~~
    
  - MvcConfig.java
    ~~~
      import org.springframework.http.converter.HttpMessageConverter;
      import org.springframework.http.converter.json.Jackson2ObjectMapperBuilder;
      import org.springframework.http.converter.json.MappingJackson2HttpMessageConverter;

      import com.fasterxml.jackson.databind.ObjectMapper;
      import com.faterxml.jackson.databind.SerializationFeature;
      ... 생략

      @Configuration
      @EnableWebMvc
      public class MvcConfig implements WebMvcConfigurer {
        ... 생략

        @Override
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
          DateTimeFormatter 	formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
          ObjectMapper objectMapper = Jackson2ObjectMapperBuilder
              .json()
              .serializerByType(LocalDateTime.class, new LocalDateTimeSerializer(formatter))
              .build();
          converters.add(0, new MappingJackson2HttpMessageConverter(objectMapper));
        }
      }
    ~~~

  - controller
    ~~~
      package controllers.test;

      import java.util.ArrayList;
      import java.util.List;

      import org.springframework.web.bind.annotation.*;

      @RestController
      public class RestControllerTest {
        @GetMapping("/rest")
        public List<String> test() {

          List<String> list=new ArrayList<>();
          list.add("abcd");
          list.add("efg");

          return list;
        }
      }

    ~~~

