---
layout: post
title:  "Lagom - Running in development"
date:   2017-01-25 00:05:00
categories: Lagom
---

이전 포스트를 보고 프로젝트를 생성하였다면 console로 접속하여 해당 라곰 프로젝트로 들어가 `activator`를 실행한다.  

```
$ cd <path to your Lagom project>
$ activator
[info] ...
\>
```
그리고 모든 서비스를 시작하려면 `runAll`을 실행한다.  

```
\> runAll
[info] ...
[info] Service helloworld-impl listening for HTTP on 0:0:0:0:0:0:0:0:23966
[info] Service hellostream-impl listening for HTTP on 0:0:0:0:0:0:0:0:27462
(Services started, press enter to stop and go back to the console...)
```
또한, Hot-reload 모드로 실행되기 때문에 서비스의 변경이 있어도 재시작할 필요가 없다.  
sbt에서 `runAll`을 실행하면 다음과 같은 작업을 수행한다.  
(activator가 sbt환경에서 수행되기때문에 activator로도 sbt console모드를 사용할 수 있다.)  

* Service Locator 시작
* Cassandra server(이하 카산드라 서버) 시작
* Kafka server(이하 카프카 서버) 시작
* 모든 서비스 시작 (그리고 Service Locator에 등록된다.)

웹 브라우저나 `curl`과 같은 명령 도구에서 `http://localhost:8000/services`를 수행하면 서비스가 실행 중인지 확인할 수 있다.  
Service Locator는 다음과 같은 JSON을 반환한다.  

```
[{"name":"hellostream","url":"http://0.0.0.0:26230"},
 {"name":"cas_native","url":"tcp://127.0.0.1:4000/cas_native"},
 {"name":"helloservice","url":"http://0.0.0.0:24266"}]
```
`cas_native`는 카산드라 서버이다. 카산드라는 Lagom의 기본 데이버베이스이다.  

***

#### 서비스 수동 실행

앞서 말했듯이 빌드에 정의된 모든 라곰 서비스는 `runAll` 태스크로 실행할 수 있다.(서비스들은 병렬로 시작)  
예외적으로 몇 가지 서비스만 수동으로 시작하려는 경우에는 sbt모드에서 `라곰프로젝트명/run` 을 실행하면 된다.  
위의 작업은 해당 서비스만 시작하기 때문에 Service Locator와 Cassandra server를 먼저 수동으로 시작한 후 사용해야 한다.  

#### 서비스 포트 할당

