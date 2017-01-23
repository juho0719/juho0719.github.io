---
title:  "Lagom - Lagom build in sbt"
date:   2017-01-23 21:04:00
categories: Lagom
---


라곰 프로젝트를 sbt로 빌드하는 경우 `project/plugin.sbt` 파일에 Lagom Plugin은 명시해야한다:
`addSbtPlugin("com.lightbend.lagom" % "lagom-sbt-plugin" % "1.0.0-M1")`


### 빌드 파일 작성

일반적으로 `build.sbt` 하나의 파일로 필요에 따라 여러개로 작성될 수도 있다.
sbt 빌드 파일은 스칼라 DSL로 작성된다. Activator로 프로젝트를 생성하였다면 라곰을 실행시키기 위한 파일들이 대부분 작성되어 있다.


#### 스칼라 버전 세팅

스칼라 버전은 다음과 같이 명시한다:
`scalaVersion in ThisBuild := "2.11.8"`


#### 프로젝트 정의

한 서비스에는 적어도 두개의 프로젝트가 있다(API, impl)
라곰 프로젝트는 일반적인 sbt 프로젝트이다. 다음과 같이 정의한다
```
lazy val helloworldApi = (project in file("helloworld-api"))
  .settings(
    version := "1.0-SNAPSHOT",
    libraryDependencies += lagomJavadslApi
  )
```

