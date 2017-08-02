---
layout: post
title:  "Spring boot"
date:   2017-07-29 22:00:00 +0900
categories: Spring, SpringBoot
---

프로퍼티 적용 방법 (아스키배너 숨기기)
- 매개변수 : java -jar xxx.jar --spring.main.show-banner=false
- application.properties
  spring.main.show-banner=false
- application.yaml
  spring:
    main:
      show-banner: false
- 환경변수 
  export spring_main_show_banner=false
  
프로퍼티 적용 우선순위(높은 순위로 오버라이딩)
1. 매개변수
2. java:comp/env에서 얻는 JNDI속성
3. JVM 시스템 프로퍼티
4. 운영체제 환경변수
5. random.*로 시작하는 프로퍼티 때문에 무작위로 생성된 값
6. 애플리케이션 외부에 있는 application.properties나 application.yml
7. 애플리케이션 내부에 패키징된 application.properties나 application.yml
8. @PropertySource로 지정된 프로퍼티
9. 기본 프로퍼티

application.properties나 yaml파일 위치 (우선순위)
- 외부 애플리케이션이 작동하는 디렉토리의 /config 하위
- 외부 애플리케이션이 작동하는 디렉토리
- 내부적으로 config 패키지
- 내부적인 클래스패스 위치

동일한 이름의 properties/yaml이 있으면 yaml으로 적용

프로퍼티(탬플릿 캐싱 비활성화)
- spring.thymeleaf.cache=false
- spring.freemarker.cache=false
- spring.groovy.template.cache=false
- spring.velocity.cache=false

프로퍼티(서버포트)
- server.port=8000

프로퍼티(ssl)
- server.port=8443
- server.ssl.key-store=file://path..../mykeys.jks (jar파일안의 것으로 참조하려면 classpath:형식의 url사용)
- server.ssl.key-store-password=xxx
- server.ssl.key-password=xxx

프로퍼티(로그)
- logging.level.root=WARN
- logging.level.org.springframework.security=DEBUG
- logging.path=/var/logs/
- logging.file=xxx.log

프로퍼티(DB)
- spring.datasource.url=jdbc:mysql://localhost/readinglist
- spring.datasource.username=user
- spring.datasource.password=pass
- spring.datasource.driver-class-name=com.mysql.jdbc.Driver (보통 생략함. 오류 날 경우 명시)
- spring.datasource.jndi-name=xxxx (jndi로도 가능)

