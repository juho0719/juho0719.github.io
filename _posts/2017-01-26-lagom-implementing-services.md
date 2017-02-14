---
layout: post
title:  "Lagom - Implementing services"
date:   2017-01-26 13:48:00 +0900
categories: Lagom
---

### Implementing services

서비스들은 서비스 디스크립터 인터페이스의 구현을 제공하고 해당 디스크립터가 지정한 각 메소드를 구현하는 걸로 작성된다.  
아래 코드는 `HelloService` 디스크립터를 구현한 예제이다.  

```scala
import com.lightbend.lagom.javadsl.api.*;
import akka.NotUsed;
import static java.util.concurrent.CompletableFuture.completedFuture;

public class HelloServiceImpl implements HelloService {

    public ServiceCall<String, String> sayHello() {
        return name -> completedFuture("Hello " + name);
    }
}
```
`sayHello()`메소드는 람다를 사용하여 구현됐다. 여기서 중요한 점은 