---
title: RESTful
date: 2022-09-05 22:55:00 +09:00
categories: [Blog, Java, RESTful]
tags: [restful java]
---

## RESTful?

- REST란? REpresentational State Transfer)의 약자
- REST의 기본 원칙을 지킨 서비스

## REST의 6가지 원칙

- Uniform Interface
- Stateless
- Caching
- Client-Server
- Layered system
- Code on demand (optional)

### Uniform Interface

- Resource(URI)에 대한 요청을 일관적으로 수행하는 아키텍처 스타일
- 요청을 하는 Client에 무관
- 모든 플랫폼에서 요청 가능하며 Loosely Coupling(느슨한 결합)형태를 갖춤

### Stateless

- 어떠한 request도 client 상태(state)에 관여하지 않음
- 즉, 어떤 서버에 요청을 보내든 같은 결과를 얻을 수 있음

### Caching

- 이전에 가져온 리소스들을 재사용
- GET 메소드에서만 동작

### Client-Server

- client는 server에 대해 알 필요 없고, server도 client에 대해 알 필요 없음 (서로 독립적)

### Layered system

- 여러 계층으로 구성
- 클라이언트와 최종 서버사이에 많은 중간 서버들이 존재할 수 있음
- 중간 서버는 LB를 활성화하고, 공유 캐시를 사용하여 가용성을 향상시킬 수 있음

### Code on demand (optional)

- 필요의 경우 client는 server의 코드를 수행할 수 있어야 함

## RESTful하게 디자인한다는 것은?

- 리소스(Resource)와 행위(Behavior)를 직관적으로 분리
- 행위는 `HTTP Method`로 표현하며, `GET`, `POST`, `PUT`, `PATCH`, `DELETE`를 목적에 맞게 사용
- 메시지(Message)는 `Header`와 `Body`를 분리해서 사용
- API의 버전을 관리하고, 하위호환성을 보장해야 함
- 서버와 클라이언트가 같은 방식으로 데이터를 주고 받아야 함(json-json, form-form)

## RESTful의 장점

- 멀티플랫폼 지원 가능
- 원하는 타입으로 데이터를 주고 받음
- 기존에 사용하던 HTTP를 그대로 사용 가능

## RESTful의 단점

- 메소드 갯수가 적음
- 분산환경에는 적합하지 않음
- HTTP 프로토콜만 지원 가능
