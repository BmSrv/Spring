# jpa 설정(Hibernate EntityManager)  
  - 의존성
    ~~~
      <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-entitymanager -->
      <dependency>
          <groupId>org.hibernate</groupId>
          <artifactId>hibernate-entitymanager</artifactId>
          <version>5.6.12.Final</version>
      </dependency>
      
      <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.9.1</version>
          <scope>runtime</scope>
      </dependency>
      <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.30</version>
      </dependency>


    ~~~
  
- persistence.xml
  ~~~
    <?xml version="1.0" encoding="UTF-8"?>
    <persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.2">
      <persistence-unit name="spring_board">
          <properties>
              <!--  필수 속성 -->
              <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver" />
              <property name="javax.persistence.jdbc.user" value="root" />
              <property name="javax.persistence.jdbc.password" value="password" />
              <property name="javax.persistence.jdbc.url" value="serverURL" />
              <property name="hibernate.dialect" value="org.hibernate.dialect.MySQL8Dialect" />


         <!-- 옵션 -->
         <!-- 콘솔에 하이버네이트가 실행하는 SQL문 출력 -->
         <property name="hibernate.show_sql" value="true"/>

         <!-- SQL 출력 시 보기 쉽게 정렬 -->
         <property name="hibernate.format_sql" value="true"/>
         <!-- 쿼리 출력 시 주석(comments)도 함께 출력 -->
         <property name="hibernate.use_sql_comments" value="true"/>
          <!-- JPA 표준에 맞춘 새로운 키 생성 전략 사용 -->
         <property name="hibernate.id.new_generator_mappings" value="true"/>
          <!-- 애플리케이션 실행 시점에 데이터베이스 테이블 자동 생성 -->
          <property name="hibernate.hbm2ddl.auto" value="update"/>
          <!-- 이름 매핑 전략 설정 - 자바의 카멜 표기법을 테이블의 언더스코어 표기법으로 매핑
           ex) lastModifiedDate -> last_modified_date -->
          <property name="hibernate.ejb.naming_strategy" value="org.hibernate.cfg.ImprovedNamingStrategy" />
        </properties>
      </persistence-unit>
    </persistence>
  ~~~
  
  
 - DBConfig.java
    ~~~
      package spring.config;

      import javax.persistence.EntityManager;
      import javax.persistence.EntityManagerFactory;
      import javax.persistence.Persistence;

      import org.springframework.context.annotation.Bean;
      import org.springframework.context.annotation.Configuration;
      import org.springframework.context.annotation.EnableAspectJAutoProxy;

      import commons.proxy.TransactionProxy;

      @Configuration
      @EnableAspectJAutoProxy(proxyTargetClass = true)
      public class DBConfig {
        @Bean
        public EntityManagerFactory entityManagerFactory() {
          EntityManagerFactory emf=Persistence.createEntityManagerFactory("spring_board");

          return emf;
        }

        @Bean
        public EntityManager entityManager() {

          return entityManagerFactory().createEntityManager();
        }


        @Bean
        public TransactionProxy transactionProxy() {
          return new TransactionProxy();
        }
      }

    ~~~
    
  - TransactionProxy.java
    ~~~
      package commons.proxy;

      import javax.persistence.EntityManager;
      import javax.persistence.EntityTransaction;
      import javax.transaction.Transaction;

      import org.aspectj.lang.ProceedingJoinPoint;
      import org.aspectj.lang.annotation.Around;
      import org.aspectj.lang.annotation.Aspect;
      import org.springframework.beans.factory.annotation.Autowired;

      @Aspect
      public class TransactionProxy {

        @Autowired
        EntityManager em;


        @Around("execution(public * models..*Dao.*(..))")
        public Object apply(ProceedingJoinPoint joinPoint) throws Throwable {

          Object result=null;

          EntityTransaction tx =em.getTransaction();
          try {

            tx.begin();

            result=joinPoint.proceed();

            tx.commit();

          } catch (Exception e) {
            e.printStackTrace();
            tx.rollback();
          }

          return result;


        }
      }

    ~~~
    
    
  
