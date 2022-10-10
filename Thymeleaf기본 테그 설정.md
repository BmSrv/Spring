# 공통 레이아웃
  - 메인
    - WEB-INF/view/layout/main.html
      ~~~
        <!DOCTYPE html>
        <html 
          xmlns:th="http://www.thymeleaf.org"
          xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
        >
        <head>
          <link rel="stylesheet" th:href="@{/css/clear.css}">
          <link rel="stylesheet" th:href="@{/css/common.css}">
          <script type="text/javascript" src="@{/js/common.js}"></script>

          <link rel="stylesheet" th:each="css:${addCss}" th:href="@{/css/{item}.css(item=${css})}" >
          <script th:each="js:${addJs}" th:src="@{/js/{item}.js(item=${js})}" ></script>

          <th:block layout:fragment="cssContext"></th:block>
          <th:block layout:fragment="jsContext"></th:block>

        </head>
        <body>
          <header th:replace="outline/header::main"></header>
          <nav th:replace="outline/nav::main"></nav>
          <main layout:fragment="content"></main>
          <footer th:replace="outline/footer::main"></footer>
        </body>
        </html>
      ~~~

  - 헤더
    - WEB-INF/view/outline/header.html
      ~~~
        <!DOCTYPE html>
        <html xmlns:th="http://www.thymeleaf.org">
          <head></head>
          <body>
            <main th:fragment="main">
              <h1>메인 헤더</h1>
            </main>
          </body>

        </html>
      ~~~
      
      
  - 푸터
    - WEB-INF/view/outline/footer.html
      ~~~
        <!DOCTYPE html>
        <html xmlns:th="http://www.thymeleaf.org">
          <head>
          </head>
          <body>
          <main th:fragment="main">
            <h1>메인 푸터</h1>
          </main>


          </body>

        </html>
      ~~~
      
  - 메뉴
    - WEB-INF/view/outline/nav.html
      ~~~
        <!DOCTYPE html>
        <html 
          xmlns:th="http://www.thymeleaf.org"
        >
        <head></head>
        <body>
          <nav th:fragment="main">
            <h1>메뉴</h1>
          </nav>
        </body>

        </html>
      ~~~




