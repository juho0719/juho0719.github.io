---
title:  "Lagom Installation"
date:   2017-01-23 14:47:00
categories: Lagom
---
Lagom(이하 라곰)은 Maven(이하 메이븐)과 sbt 두가지 빌드 툴을 지원한다.
메이븐이 더 널리 쓰이고 있어 익숙하겠지만, 개인적인 호기심으로 sbt를 써보기로 한다.
-------------


### JDK 설치

JDK설치 법은 대부분 다 알고 있으리라 보고 넘어 가기로 한다.
단지, 라곰은 JDK8이상 버전을 사용해야 하기 때문에 8버전으로 설치한다.

***


### sbt 설치

sbt는 Java(이하 자바)와 Scala(이하 스칼라)의 빌드 툴이다.
이외에도 라곰에서는 Activator를 제공하고 있는 데 라곰 프로젝트 템플릿을 지원하는 툴이다.
Activator를 설치하면 sbt가 내장되어 있기 때문에 Activator를 설치하기로 한다.

옛날 버전 Activator를 설치하면 OutOfMemoryException이 발생 될 수 있어 1.3.10이상 버전을 설치해야 한다.
아래 사이트에서 다운로드 받은 후 설치 디렉토리/bin의 PATH를 잡아주면 된다.
[Activator 다운로드](https://www.lightbend.com/activator/download) 
***
