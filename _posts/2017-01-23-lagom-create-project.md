---
title:  "Lagom - Create Project"
date:   2017-01-23 18:18:00
categories: Lagom
---

### Creating new Lagom project

라곰 프로젝트를 만드는 가장 쉬운 방법은 Activator를 이용하는 것이다.

`activator new 프로젝트명 lagom-java`

Activator를 이용하면 helloworld와 hellostream 두개의 서비스가 포함되어 생성된다.

***


### Lagom Project Structure(Activator)

```
프로젝트명
 ㄴ hellostream-api
 ㄴ hellostream-impl
 ㄴ helloworld-api
 ㄴ helloworld-impl
 ㄴ project
    ㄴ build.properties
    ㄴ plugins.sbt
 ㄴ build.sbt
```

api 프로젝트는 서비스 인터페이스, impl 프로젝트는 서비스 구현체가 있다.
아래는helloworld-api 프로젝트의 HelloService.java이다. 

```java
public interface HelloService extends Service {
    ServiceCall<NotUsed, String> hello(String id);
    
    @Override
    default Descriptor descriptor() {
    return named("helloservice").withCalls(
        restCall(Method.GET,  "/api/hello/:id", this::hello)
      ).withAutoAcl(true);
  }
}
```

Service 인터페이스를 상속 받고, descriptor()를 구현한다.
descriptor()는 서비스명과 REST endpoint를 정의한다.
선언된 enpoint에 구현체에서 사용할 추상 메소드를 선언한다. (예: `ServiceCall<NotUsed, String> hello(String id);`와 같은..)

구현체는 helloworld-impl프로젝트에 구현되어 있다. (HelloServiceImple.java)
```java
public class HelloServiceImpl implements HelloService {

  private final PersistentEntityRegistry persistentEntityRegistry;

  @Inject
  public HelloServiceImpl(PersistentEntityRegistry persistentEntityRegistry) {
    this.persistentEntityRegistry = persistentEntityRegistry;
    persistentEntityRegistry.register(HelloWorld.class);
  }

  @Override
  public ServiceCall<NotUsed, String> hello(String id) {
    return request -> {
      // Look up the hello world entity for the given ID.
      PersistentEntityRef<HelloCommand> ref = persistentEntityRegistry.refFor(HelloWorld.class, id);
      // Ask the entity the Hello command.
      return ref.ask(new Hello(id, Optional.empty()));
    };
  }
}
```
PersistentEntity 레지스트리를 사용하면 Event Sourcing and CQRS를 사용하여 데이터베이스에 데이터를 넣을 수 있다.

***


### Running Lagom services

activator 콘솔에서 runAll을 입력하면 모든 서비스를 시작할 수 있다.

```
$ cd my-first-system
$ activator
... (booting up)
> runAll
[info] Starting embedded Cassandra server
..........
[info] Cassandra server running at 127.0.0.1:4000
[info] Service locator is running at http://localhost:8000
[info] Service gateway is running at http://localhost:9000
[info] Service helloworld-impl listening for HTTP on 0:0:0:0:0:0:0:0:24266
[info] Service hellostream-impl listening for HTTP on 0:0:0:0:0:0:0:0:26230
(Services started, use Ctrl+D to stop and go back to the console...)
```

curl로 해당 서비스가 동작하는지 테스트할 수 있다.(curl이 없는 경우 브라우저에서 테스트해도 된다.)

`$ curl http://localhost:9000/api/hello/World`

***