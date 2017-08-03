---
layout: post
title:  "Spring boot3"
date:   2017-08-03 22:00:00 +0900
categories: Spring, SpringBoot
---

스프링부트에서는 @ContextConfiguration을 사용하지 않고 @SpringApplicationConfiguration으로 사용해야 한다.
(SpringApplication은 컨텍스트뿐만아니라 로깅과 외부 프로퍼티로딩, 스프링 부트의 다른 기능을 활성화)

스프링 MVC 모킹
실제 서블릿 컨테이너에서 컨트롤러를 실행하지 않고도 컨트롤러에 HTTP요청을 할 수 있음.
MockMvcBuilders를 사용
- standalongeSetup() : 수동으로 생성하고, 컨트롤러 한개 이상을 서비스할 Mock MVC를 만듦
- webAppContextSetup() : 컨트롤러 한개 이상을 포함하는 스프링 애플리케이션 컨텍스트를 사용, Mock MVC를 만듦
