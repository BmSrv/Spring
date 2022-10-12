# multipart 설정
  - web.xml

~~~
<servlet>

...

  <multipart-config>
      <max-file-size>단일 파일 최대크기</max-file-size>
      <max-request-size>모든 파일 최대 크기</max-request-size>
      <file-size-threshold>버퍼 크기</file-size-threshold>
  </multipart-config>
  
...

</servlet>
~~~
- ex)

~~~
<servlet>

...

  <multipart-config>
      <max-file-size>20971520</max-file-size>
      <max-request-size>62914560</max-request-size>
      <file-size-threshold>20971520</file-size-threshold>
  </multipart-config>
  
...

</servlet>
~~~
