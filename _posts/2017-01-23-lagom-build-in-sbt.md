---
title:  "Lagom - Lagom build in sbt"
date:   2017-01-23 21:04:00
categories: Lagom
---


라곰 프로젝트를 sbt로 빌드하는 경우 `project/plugin.sbt` 파일에 Lagom Plugin을 명시해야한다  
`addSbtPlugin("com.lightbend.lagom" % "lagom-sbt-plugin" % "1.0.0-M1")`

***

### 빌드 파일 작성

일반적으로 `build.sbt` 하나의 파일로 필요에 따라 여러개로 작성될 수도 있다.  
sbt 빌드 파일은 스칼라 DSL로 작성된다. Activator로 프로젝트를 생성하였다면 라곰을 실행시키기 위한 파일들이 대부분 작성되어 있다.  


* 스칼라 버전 세팅

스칼라 버전은 다음과 같이 명시한다.  
`scalaVersion in ThisBuild := "2.11.8"`  


* 프로젝트 정의

한 서비스에는 적어도 두개의 프로젝트가 있다(API, impl)  
라곰 프로젝트는 일반적인 sbt 프로젝트이다. 다음과 같이 정의한다  

```
lazy val helloworldApi = (project in file("helloworld-api"))
  .settings(
    version := "1.0-SNAPSHOT",
    libraryDependencies += lagomJavadslApi
  )
```
lazy val을 사용하면 초기화 순서 문제를 방지할 수 있다.  
라곰 플러그인은 라곰 라이브러리를 쉽게 추가할 수 있도록 `lagomJavadslApi`를 제공한다.  
만약 보통 sbt에서 사용하는 `groupId`,`artifactId`,`version` 와 같은 것을 지정하고 싶다면  
[Library dependencies in the sbt documentation](http://www.scala-sbt.org/0.13/docs/Library-Dependencies.html) 을 참고한다.  

```
lazy val helloworldImpl = (project in file("helloworld-impl"))
  .enablePlugins(LagomJava)
  .settings(
    version := "1.0-SNAPSHOT"
  )
  .dependsOn(helloworldApi)
```
api프로젝트에는 플러그인을 사용할 필요가 없지만 impl 프로젝트에선 사용할 수 있다.  
`LagomJava` 플러그인을 추가하면, 라곰 프로젝트를 실행하는 데 필요한 설정과 종속성이 추가된다.  
impl 프로젝트는 api프로젝트에 대한 의존성을 선언하여 api인터페이스를 구현한다.(`.dependsOn(hellowrldApi)`)  

```
lazy val hellostreamApi = (project in file("hellostream-api"))
  .settings(
    version := "1.0-SNAPSHOT",
    libraryDependencies += lagomJavadslApi
  )

lazy val hellostreamImpl = (project in file("hellostream-impl"))
  .enablePlugins(LagomJava)
  .settings(
    version := "1.0-SNAPSHOT"
  )
  .dependsOn(hellostreamApi, helloworldApi)
```
첫번째와 비슷하지만 다른 점은 `helloworldApi`도 의존성을 추가해야 서비스를 호출할 수 있다.

