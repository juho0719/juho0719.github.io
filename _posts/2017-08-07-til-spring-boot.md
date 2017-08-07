---
layout: post
title:  "Spring boot4"
date:   2017-08-07 22:00:00 +0900
categories: Spring, SpringBoot
---

스프링 시큐리티 테스트 모듈 추가
testCompile('org.springframework.wecurity:spring-security-test')
springSecurity()는 Mock MVC용 스프링 시큐리티를 활성화

스프링 시큐리티 인증 요청
@WithMockUser : UserDetails생성 후 보안 컨텍스트 로드
@WithUserDetails : 지정한 사용자 이름으로 UserDetails객체를 조회하여 보안 컨텍스트 로드

```java
@Test
@WithUserDetails("craig")   //craig를 사용자로 사용
public void homePage_authenticatedUser() throws Exception {
    Reader expectedReader=new Reader();
    expectedReader.setUsername("craig");
    expectedReader.setPassword("password");
    expectedReader.setFullname("Craig Walls");
    expectedReader.setUsername("craig");
    
    mockMvc.perform(get("/"))     //GET 요청 수행
        .andExpect(status().isOk())
        .andExpect(view().name("readingList")
        .andExpect(model().attribute("reader", samepropertiyValuesAs(expectedReader)))
        .andExpect(model().attribute("books", hasSize(0)))
        .andExpect(model().attribute("amazonID", hasSize()))
}

```
