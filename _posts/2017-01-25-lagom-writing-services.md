---
layout: post
title:  "Lagom - Writing services"
date:   2017-01-25 17:51:00 +0900
categories: Lagom
---

### Service Descriptors

라곰 서비스는 서비스 디스크립터인 인터페이스에서 만들어 진다.  
이 인터페이스는 서비스 호출 및 구현방법을 정의하고 인터페이스가 기본 전송 프로토콜로 매핑되는 방식을 설명하는 메타 데이터도 정의한다. 
일반적으로 서비스 디스크립터를 구현하고 사용하는 것은 REST, 웹소켓등 어떤 전송방식이더라도 변하지 않아야 한다.  

```scala
import com.lightbend.lagom.javadsl.api.*;

import static com.lightbend.lagom.javadsl.api.Service.*;

public interface HelloService extends Service {
    ServiceCall<String, String> sayHello();

    default Descriptor descriptor() {
        return named("hello").withCalls(
                call(this::sayHello)
        );
    }
}
```
위의 디스크립터는 `sayHello`를 호출하는 서비스를 정의하고 있다.  
`sayHello()`는 `ServiceCall`을 반환하는 메소드이고 해당 서비스가 사용되면 호출되거나 서비스 그자체에서 구현할 수 있는 호출을 말한다.  
다음 인터페이스를 보자.  

```scala
interface ServiceCall<Request, Response> {
  CompletionStage<Response> invoke(Request request);
}
```
여기서 중요한 점은 위의 인터페이스에는 `sayHello()`가 없음에도 호출됐고, `invoke()`도 호출할 수 있는 권한을 가져왔다는 것이다.  
`ServiceCall`은 두개의 파라미터를 가진다. `Request`는 내부로 들어오는 request message이고 `Response`는 외부로 나가는 response message이다. 
이전의 `HelloService`에서 정의한 `sayHello()`를 보면 두개의 타입 모두 `String`형태로 되어 있는 것을 볼 수 있다. 
따라서 이 서비스는 단순 텍스트를 처리하는 서비스인 것을 확인할 수 있다.  
`sayHello()` 메소드는 프로그래밍 방식으로 어떻게 호출하고 구현하는 것인지 설명하지만 어떤 전송방식에 매핑되는지는 설명하지 않는다. 
이것은 dscriptor() 호출을 기본으로 제공하고, 인터페이스는 서비스에 의해 작성된다.  
이 서비스는 단순히 `sayHello()`호출하고, `hello`를 리턴받는 다는 것을 알 수있다. 
매우 간단하기 때문에 `call` 메소드에 `sayHello()`를 전달하는 것 이상을 수행할 필요가 없다.  

***

### Call identifiers

서비스 호출에는 식별자가 필요하다.  
식별자는 클라이언트와 서비스 구현에 라우팅 정보를 제공하기 위해 사용되므로 유선을 통한 호출을 적절히 매핑할 수 있다. 
식별자는 정적이름, 경로 또는 동적 구성 요소를 가질 수 있다. 동적 경로 파라미터는 경로에서 추출하여 서비스 호출 메소드에 전달된다.
가장 간단한 타입의 식별자는 이름이고, 이름은 디폴트로 구현한 인터페이스의 메소드 이름과 동일하게 설정된다.  
커스텀 이름은 `namedCall`메소드에 전달하는 것으로 지정할 수 있다.  

```scala
default Descriptor descriptor() {
    return named("hello").withCalls(
            namedCall("hello", this::sayHello)
    );
}
```
위의 유형은 `sayHello`대신 `hello`로 변경한 예제코드이다. 만약 REST형태로 구현되면 `/hello`로 호출할 수 있다.  

* Path based identifiers  

식별자의 두번째 타입은 경로 기반 식별자이다. 이것은 호출을 라우팅하는 URI경로와 쿼리 문자열이고, 동적 경로 파라미터롤 선택적으로 추출할 수 있다. 
이 타입은 `pathCall` 메소드를 사용하여 설정할 수 있다. 
동적 경로 파라미터는 경로에 동적파트를 선언하여 경로에서 추출된다. 
콜론(:)을 프리픽스로 사용한다.(예:`/order/:id`) 라곰은 경로에 있는 파라미터에서 추출할 것이고, 서비스 호출 메소드로 전달할 것이다. 
라곰은 메소드에서 허용하는 타입을 변환하기 위해 `PathParamSerializer`를 사용한다. 
라곰은 `String`, `Long`, `Integer`, `Boolean`과 같은 많은 `PathParamSerializer`가 기본 제공된다. 
다음은 `long`타입의 파라미터에서 추출하여 서비스 호출에 전달하는 예제이다.  

```scala
ServiceCall<NotUsed, Order> getOrder(long id);

default Descriptor descriptor() {
    return named("orders").withCalls(
            pathCall("/order/:id", this::getOrder)
    );
}
```
여러개의 파라미터도 가능하다. 다음은 그 예제이다.  

```scala
ServiceCall<NotUsed, Item> getItem(long orderId, String itemId);

default Descriptor descriptor() {
    return named("orders").withCalls(
            pathCall("/order/:orderId/item/:itemId", this::getItem)
    );
}
```

쿼리 문자열 파라미터에서도 경로의 끝에 `?`를 붙인 후에 리스트 구분자로 `&`를 사용하여 추출할 수 있다. 
다음은 쿼리 문자열 파라미터 예제이다.  

```scala
ServiceCall<NotUsed, PSequence<Item>> getItems(long orderId, int pageNo, int pageSize);

default Descriptor descriptor() {
    return named("orders").withCalls(
            pathCall("/order/:orderId/items?pageNo&pageSize", this::getItems)
    );
}
```

`call`, `namedCall`, `pathCall`을 사용할 때, REST에 매핑하면 라곰은 의미있는 방식으로 REST에 매핑하려고 최선의 노력을 다한다. 
즉, Request Message가 있을 경우 `POST`를 사용하고, 없으면 `GET`을 사용한다.  

* REST identifiers  

식별자의 마지막 타입은 REST 식별자이다. REST 식별자는 semantic REST API를 생성할 때 사용되도록 디자인되어 있다. 
그것은 경로기반 식별자와 같은 경로나 request 메소드를 사용하여 경로를 식별한다. 그것은 `restCall`로 설정할 수 있다.  

```scala
ServiceCall<Item, NotUsed> addItem(long orderId);
ServiceCall<NotUsed, Item> getItem(long orderId, String itemId);
ServiceCall<NotUsed, NotUsed> deleteItem(long orderId, String itemId);

default Descriptor descriptor() {
    return named("orders").withCalls(
            restCall(Method.POST,   "/order/:orderId/item",         this::addItem),
            restCall(Method.GET,    "/order/:orderId/item/:itemId", this::getItem),
            restCall(Method.DELETE, "/order/:orderId/item/:itemId", this::deleteItem)
    );
}
```

***

### Messages

라곰 서비스는 request와 response를 가지고 있다. request나 response가 사용되지 않을 때, `akka.NotUsed` 를 대신 사용할 수 있다. 
request와 response는 strict과 stream 두개의 카테고리로 분류 된다.  

* Strict messages  

strict 메세지는 간단한 자바 오브젝트로 표현할 수 있는 단일 메시지이다. 
그 메세지는 메모리에 버퍼링될 것이고 json과 같이 파싱된다. 
양쪽 메세지 타입이 모두 strict이면 동기방식으로 호출된다. 즉, request를 보내고 response를 받을 때까지 기다린다는 말이다. 

지금까지 본 모든 서비스 호출 예제는 모두 strict타입의 메세지를 사용했다. 
위의 주문 서비스 디스크립터는 아이템과 주문을 받고 반환한다. 그 입력 값이 서비스 호출에 직접 전달되고 번환되며 이러한 값은 JSON버퍼에 직렬화되고 JSON에서 역직렬화되기 전에 메모리로 읽혀진다.  

* Streamed messages  

streamed 메세지는 `Source` 타입의 메세지이다. `Source`는 비동기 스트리밍과 메세지 핸들링을 허용하는 Akka streams API이다. 
다음은 그 예제이다.  

```scala
ServiceCall<String, Source<String, ?>> tick(int interval);

default Descriptor descriptor() {
    return named("clock").withCalls(
        pathCall("/tick/:interval", this::tick)
    );
}
```
이 서비스 호출은 strict request 타입과 streamed response 타입을 가진다. 
이것의 구현은 `String`의 tick 메세지를 보내는 `Source`를 리턴하는 일이 있다. 
양방향으로 스트리밍된 호출은 다음과 같다.  

```scala
ServiceCall<Source<String, ?>, Source<String, ?>> sayHello();

default Descriptor descriptor() {
    return named("hello").withCalls(
        call(this::sayHello)
    );
}
```
서버는 request stream으로 받는 모든 메세지를 `Hello`를 접두어로 사용한 메세지로 변환하는 `Source`를 반환할 것이다.  

라곰은 스트림에 적합한 전송방법을 선택하는 데, 일반적으로 WebSockets(이하 웹소켓)을 선택한다. 
웹소켓은 양방향 스트리밍을 지원한다. 그래서 스트리밍에 좋은 일반적인 옵션이다. 
request 또는 response 메세지 중 하나만 스트리밍 되는 경우, 라곰은 단일 메세지를 보내거나 받아서 다른 방향이 닫힐 때까지 웹소켓을 열어 두어 strict 메세지 송수신을 구현할 것이다. 
그렇지 않고 다른 방향이 닫히면 웹소켓을 종료할 것이다.  

* Message serialization  

기본적으로 라곰은 request와 response의 직렬화, 역직렬화에 적합한 serializer를 선택한다. 
라곰은 json을 사용하여 통신하고, Jackson 라이브러리를 사용하여 메세지를 직렬화, 역직렬화한다.  

커스텀 메세지 serializer를 작성하고 설정하는 방법을 비롯하여 메세지 serializer에 대한 자세한 내용은 `Message Serializers`를 참조한다.  

