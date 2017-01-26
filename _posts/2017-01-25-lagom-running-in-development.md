---
layout: post
title:  "Lagom - Running in development"
date:   2017-01-25 00:05:00 +0900
categories: Lagom
---

이전 포스트를 보고 프로젝트를 생성하였다면 console로 접속하여 해당 라곰 프로젝트로 들어가 `activator`를 실행한다.  

```
$ cd <path to your Lagom project>
$ activator
[info] ...
>
```
그리고 모든 서비스를 시작하려면 `runAll`을 실행한다.  

```
> runAll
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
예외적으로 몇 가지 서비스만 수동으로 시작하려는 경우에는 sbt모드에서 `라곰프로젝트명/run` 태스크를 실행하면 된다.  
위의 작업은 해당 서비스만 시작하기 때문에 Service Locator와 Cassandra server를 먼저 수동으로 시작한 후 사용해야 한다.  

***

#### 서비스 포트 할당

서비스가 시작되면 포트가 할당된다.  
포트가 할당되는 알고리즘은 다음과 같다.  
1. 프로젝트명을 hash한다.  
2. hash된 값을 포트범위에 적용시킨다.(기본범위 `[49152,65535]`)  
3. 만약 다른 프로젝트에서 이 포트를 사용하는 곳이 없다면 선택된 포트가 할당된다.  
   (해당 포트를 사용한 프로젝트가 있다면 프로젝트명을 알파벳순으로 나열하여 첫번째 프로젝트에 할당하고, 다른 프로젝트는 그 이웃한 포트를 할당한다.)  

포트를 수동으로 지정하고 싶다면 `build.sbt`에 아래와 같이 설정한다.  
`lazy val usersImpl = (project in file("usersImpl")).enablePlugins(LagomJava).settings(lagomServicePort := 11000)`  

포트 범위를 수정하고 싶다면 `build.sbt`에 아래와 같이 설정한다.  
`lagomServicesPortRange in ThisBuild := PortRange(40000, 45000)`  

범위를 작게 잡으면 새로운 프로젝트가 추가될 때마다 기존 프로젝트의 포트가 변경될 가능성이 높으므로 너무 작지 않게 잡는 것이 좋다.  

***

#### Service Locator

* 포트 수동 할당  
Service Locator의 기본 포트는 `8000`이다. 이 부분을 변경하고 싶다면 `build.sbt`에 아래와 같이 설정한다.  
`lagomServiceLocatorPort in ThisBuild := 10000`  

* 외부 서비스 연동  
외부의 서비스와 연동하고 싶다면 아래와 같이 Service Locator에 등록하고 사용할 수 있다.  
`lagomUnmanagedServices in ThisBuild := Map("weather" -> "http://localhost:3333")`  
위의 설정은 `http://lolcalhost:3333`에서 실행중인 `weather`라는 서비스를 등록한다는 의미이다.   
위의 서비스와의 통신하려면 Service Locator를 `@Inject`하여 `weather`서비스의 URI를 찾거나 해당 서비스를 통해 작업을 수행하면 된다. 
만약 해당 서비스가 라곰 서비스라면 아래 링크를 참조하여 외부 라곰 프로젝트 통합하는 방법을 알아본다.  
[integrating with an external Lagom Projects](http://www.lagomframework.com/documentation/1.2.x/java/MultipleBuilds.html)  

* 수동 시작과 종료 태스크  
`runAll`태스크를 실행하면 자동으로 시작되지만, `runAll`태스크를 실행하지 않는 경우에는 수동으로 시작해주어야 한다. 
Service Locator를 수동으로 시작하거나 종료하려면 sbt모드에서 아래의 태스크를 실행한다.  
시작 태스크 : `lagomServiceLocatorStart`  
종료 태스크 : `lagomServiceLocatorStop`  

* 비활성화(Disable) 
`build.sbt`설정 파일에 아래와 같이 설정하면 비활성화 시킬 수 있다.  
`lagomServiceLocatorEnabled in ThisBuild := false`  
Service Locator를 비활성화 시키면 서비스를 통신할 수 없으므로 서비스에서 ServiceLocator를 구현해주어야 한다.  

***

#### Cassandra server

라곰은 카산드라를 기본 데이터베이스로 사용하고 있다. 
기본적으로 카산드라서버가 내장되어 있기 때문에 따로 설치할 필요는 없다.

* 포트 수동 할당  
라곰 내부의 카산드라서버는 기본적으로 `4000`포트를 할당하고 있다. 
원래 카산드라서버는 `9042`포트를 사용하기 때문에 `9042`를 그대로 쓰고 싶어 하는 사람도 있을 것이다. 
포트를 수동으로 설정하려면 `build.sbt`에 아래와 같이 설정하면 된다.  
`lagomCassandraPort in ThisBuild := 9042`  

* 데이터베이스 파일 유지  
기본적으로 카산드라서버가 재시작될 때 모든 데이터베이스 파일들은 삭제되고 다시 생성된다. 
이를 막고 싶다면 `build.sbt`에 아래와 같이 설정하면 된다.  
`lagomCassandraCleanOnStart in ThisBuild := false`  

* Keyspace  
Keyspace는 복제 데이터를 의미하는 네임스페이스이다.  
각 서비스는 유니크한 keyspace를 사용하여 다른 서비스의 테이블과 충돌나지 않게 해야 한다. 
다행히 라곰에서는 기본으로 프로젝트 이름을 keyspace로 자동 설정한다.(허용되지 않는 문자 제거나 수정 후) 
자동 생성된 keyspace가 적절하지 않다면 수동으로 조절할 수 있는 데, `build.sbt`에 아래와 같이 작성하면 된다.  

```
lazy val usersImpl = (project in file("usersImpl")).enablePlugins(LagomJava)
  .settings(
    name := "users-impl",
    lagomCassandraKeyspace := "users"
  )
```
원래대로라면 프로젝트명이 `users-impl`이라 keyspace는 `users_impl`로 설정되어야 하는데 위의 내용처럼 하면 `users`로 설정된다.  

Keyspace는 Production모드에서도 마찬가지로 제공되어야 한다.  
Development, Production 모두에서 사용할 수 있는 keyspace를 제공하려면 구성파일에 설정해놓는 것이 좋다.  
위의 설정파일외에 `application.conf`파일을 생성하여 `impl프로젝트/src/main/resources/`에 넣으면 된다.  
내용은 아래와 같다.  

```
cassandra-journal.keyspace=users
cassandra-snapshot-store.keyspace=users
lagom.persistence.read-side.cassandra.keyspace=users
```
`application.conf`를 통해 제공되는 keyspace가 항상 빌드에서 설정되는 keyspace보다 우선된다.  

* JVM Option  
카산드라는 별도의 프로세스로 동작되며 적절한 메모리가 기본적으로 설정되어 있다. 하지만 `build.sbt`를 통해 이 설정값들도 변경할 수 있다.  

```
lagomCassandraJvmOptions in ThisBuild := 
  Seq("-Xms256m", "-Xmx1024m", "-Dcassandra.jmx.local.port=4099",
    "-DCassandraLauncher.configResource=dev-embedded-cassandra.yaml") // these are actually the default jvm options
```
위처럼 JvmOption을 수정하여 `-DCassandraLauncher.configResource` 옵션으로 yaml파일을 구성할 수도 있다. 
(경로 `src/main/resources`)  

* Logging  
로깅은 `STDOUT`으로 출력되고, 카산드라의 로그 수준은 에러로 설정되도록 구성된다. 
다음은 `logback.xml` 파일이다.  

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%date{ISO8601} %-5level %logger - %msg%n</pattern>
    </encoder>
  </appender>

  <logger name="org.apache.cassandra" level="ERROR" />

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
  
</configuration>
```
사용된 `logback.xml`을 편집 할 수있는 기능이 없다.  
로깅 구성을 조정해야 하는 경우에는 카산드라서버를 별도로 설치하고 로컬에서 실행중인 카산드라에 연결하는 방식으로 해야 한다.
(하단의 `로컬 카산드라 인스턴스 접속` 참조)

* 시작 시간 설정  
`runAll`태스크로 카산드라 서버가 자동으로 시작될 때, 최대 20초동안 가동되어 실행되지만 `build.sbt`설정을 통해 이 시간을 조정할 수 있다.  

```
import scala.concurrent.duration._ // Mind that the import is needed.
lagomCassandraMaxBootWaitingTime in ThisBuild := 0.seconds
```
최대 부팅 대기시간을 0으로 변경하면 운영 배포 시나리오를 에뮬레이션하는 데 유용할 수 있다.  
카산드라 인스턴스가 서비스가 시작되는 순간에 사용 가능하지 않을 수도 있기 때문이다.(라고 사이트에는 적혀있지만 이해가 잘 되지 않음)  

* 수동 시작과 종료 태스크  
Service Locator와 마찬가지로 수동으로 시작, 종료할 수 있다.  
`lagomCassandraStart`, `lagomCassandraStop`  

* 비활성화(Disable)  
마찬가지로 `build.sbt`에 다음과 같은 내용을 추가하면 된다.  
`lagomCassandraEnabled in ThisBuild := false`  

* 로컬 카산드라 인스턴스 접속  
로컬에서 돌고있는 카산드라 인스턴스에 접속하여 사용하는 것도 가능하다. `build.sbt`에 다음과 같은 내용을 추가하면 된다.  

```
lagomCassandraEnabled in ThisBuild := false
lagomCassandraPort in ThisBuild := 9042
```
물론 로컬 카산드라 포트가 `9042`일 경우에 위와 같이 설정한다.  

***

### Kafka Server

라곰은 기본적으로 정보를 서로 공유해야 하기 때문에 Kafka(이하 카프카)의 Message Broker(이하 메세지 브로커)를 사용한다. 
마이크로 서비스 아키텍처에서 메세지 브로커를 사용하는 것은 서비스를 너무 강하게 결합하는 것을 피하기 위해 가장 중요하다. 
그렇기 때문에 라곰 개발환경에 카프카 서버를 내장해 제공하고 있다.   

* 포트 수동 할당  
카프카서버는 기본적으로 `9092`포트를 사용한다. 그리고 카프카는 ZooKeeper(이하 주키퍼)를 사용하는 데 주키퍼는 `2181`포트를 사용한다.  
이와 같이 설정된 포트를 변경하고 싶을 때는 `build.sbt`에 다음과 같이 설정하면 된다.  

```
lagomKafkaPort in ThisBuild := 10000
lagomKafkaZookeperPort in ThisBuild := 9999
```

* 설정 파일  
카프카 서버는 설정파일을 따로 구성할 수 있다.  
기본적으로 카프카 0.10버전과 함께 `server.properties`을 사용하고 있다.  
기본설정에서 수정하고 싶은 설정이 있는 경우 `build.sbt`에 아래와 같이 설정한다.  
`lagomKafkaPropertiesFile in ThisBuild := Some(file("path") / "to" / "your" / "own" / "server.properties")`  
경로는 프로젝트 루트에 대한 상대경로로 작성해야 한다.  

* JVM Option  
카프카 서버 또한 별도의 프로세스로 구동된다. 
카산드라와 마찬가지로 JVM Option을 조정할 수 있다. 
`build.sbt`에 아래와 같이 설정하면 된다.

```
lagomKafkaJvmOptions in ThisBuild := Seq("-Xms256m", "-Xmx1024m") // these are actually the default jvm options
```

* 로그  
로그는 파일에만 작성되며, `<your-project-root>/target/lagom-dynamic-projects/lagom-internal-meta-project-kafka/target/log4j_output` 에서 찾을 수 있다.  

* 커밋 로그  
카프카는 기본적으로 오래가는 커밋로그이다. 다음 경로에 모든 데이터가 저장된다.  
`<your-project-root>/target/lagom-dynamic-projects/lagom-internal-meta-project-kafka/target/logs`

* 수동 시작과 종료 태스크  
카프카도 수동으로 시작과 종료할 수 있다.  
`lagomCKafkaStart`, `lagomKafkaStop`  

* 비활성화(Disable)  
`build.sbt`에 다음과 같이 설정하면 비활성화 할 수 있다.  
`lagomKafkaEnabled in ThisBuild := false`  
보통 외부 카프카 서버에 접속하기 위해 비활성화 설정을 많이 한다.  

* 외부 카프카 서버 접속  
외부 카프카 서버에 접속하기 위해서는 `build.sbt`에 다음과 같은 설정을 추가한다.  

```
lagomKafkaEnabled in ThisBuild := false
lagomKafkaAddress in ThisBuild := "localhost:10000"
```
위처럼 `localhost`로 설정한다는 것은 로컬에 있는 카프카 서버에 접속한다는 뜻이다.  
위와 같이 로컬에 접속하는 경우에는 포트 번호만 적어줘도 된다.  

```
lagomKafkaEnabled in ThisBuild := false
lagomKafkaPort in ThisBuild := 10000
```
