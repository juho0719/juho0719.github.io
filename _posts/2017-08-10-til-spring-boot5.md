---
layout: post
title:  "Spring boot5"
date:   2017-08-10 00:45:00 +0900
categories: Spring, SpringBoot
---

@WebIntegrationTest
이걸 선언하면 스프링 부트가 테스트용 애플리케이션 컨텍스트를 생성하고 내장 서블릿도 시작

@WebIntegrationTest안에 프로퍼티도 넣을 수 있음
ex) @WebIntegrationTest(value={"server.port=0"})
randomPort도 지원
ex) @WebIntegrationTest(value={"randomPort=true"})

@Value를 이용하면 프로퍼티를 인스턴스 변수에 저장 가능
ex)@Value("${local.server.port}")
메소드 호출을 변경하여 주입된 포트 번호 사용 가능
ex) rest.getForObject("http://localhost:{port}/bogusPage", String.class, port);

HTML 레이어 테스트는 셀레늄 추천 (Selenium)
셀레늄 의존성 추가 (testCompile("org.springframework.selenium:selenium-java:2.53.0"))

