---
title: 마이크로 서비스 아키텍처의 통신
description: 동기 및 비동기 방식의 의미를 이해하고 마이크로 서비스 간의 다양한 통신 방법을 탐색합니다.
ms.date: 09/20/2018
ms.openlocfilehash: 25d99d3d9b00b8c20c5ded6d8b40c77fcbe0eb46
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68673300"
---
# <a name="communication-in-a-microservice-architecture"></a>마이크로 서비스 아키텍처의 통신

단일 프로세스에서 실행 중인 모놀리식 애플리케이션에서 구성 요소는 언어 수준 메서드 또는 함수 호출을 사용하여 서로 호출합니다. 코드(예:`new ClassName()`)를 사용하여 개체를 만드는 경우, 또는 구체적인 개체 인스턴스보다는 추상화를 참조하여 종속성 주입을 사용하는 경우 분리된 방식으로 호출할 수 있습니다. 어느 방법이든 개체는 동일한 프로세스 내에서 실행됩니다. 모놀리식 애플리케이션에서 마이크로 서비스 기반 애플리케이션으로 변경할 때의 가장 큰 문제는 통신 메커니즘을 변경하는 것입니다. RPC 호출에 대한 in-process 메서드 호출에서 서비스로의 직접 변환은 분산 환경에서 원활히 수행되지 않는, 번거롭고 비효율적인 통신을 야기합니다. 분산 시스템을 제대로 디자인하는 데 따르는 과제는 개발자가 모놀리식 디자인에서 분산 디자인으로 이동할 때 흔히 수행하는 가정을 나열하는 [분산 컴퓨팅에 관한 오류](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)라고 알려진 캐논도 있다는 것은 충분히 잘 알려져 있습니다.

하나의 솔루션이 아니라 여러 가지가 있습니다. 한 가지 해결책에는 최대한 많은 비즈니스 마이크로 서비스를 격리하는 것이 포함됩니다. 그런 다음, 내부 마이크로 서비스 간의 비동기 통신을 사용하고, 개체 간의 프로세스 내에서 일반적인 세분화된 통신을 성긴 통신으로 바꿉니다. 이 작업은 호출을 그룹화하고, 클라이언트에 여러 내부 호출의 결과 집계하는 데이터를 반환하여 수행할 수 있습니다.

마이크로 서비스 기반 애플리케이션은 보통 여러 서버 또는 호스트에 걸쳐있더라도 여러 프로세스 또는 서비스에서 실행되는 분산 시스템입니다. 각 서비스 인스턴스는 일반적으로 프로세스입니다. 따라서 서비스는 각 서비스의 성격에 따라 HTTP, AMQP 또는 TCP와 같은 이진 프로토콜과 같은 프로세스 내 통신 프로토콜을 사용하여 상호 작용해야 합니다.

마이크로 서비스 커뮤니티는 "[스마트 엔드포인트 및 단순 파이프](https://simplicable.com/new/smart-endpoints-and-dumb-pipes)"의 원리를 장려합니다. 이 표어는 마이크로 서비스 간 가능한 분리되고, 단일 마이크로 서비스 내에서는 가능한 화합하는 디자인을 권장합니다. 앞에서 설명한 대로 각 마이크로 서비스는 자체 데이터 및 자체 도메인 논리를 소유합니다. 엔드투엔드 애플리케이션을 작성하는 마이크로 서비스는 보통 WS-\*와 같은 복잡한 프로토콜 대신 REST 통신, 그리고 중앙 집중화된 비즈니스 프로세스 오케스트레이터 대신 유연한 이벤트 구동 통신을 사용하여 간단히 조율됩니다.

일반적으로 사용되는 두 가지 프로토콜은 리소스 API를 통한 HTTP 요청/응답(대부분 쿼리할 때)과 여러 마이크로 서비스 간 업데이트를 통신할 때의 간단한 비동기 메시징입니다. 이 내용은 다음 섹션에서 더 자세히 설명합니다.

## <a name="communication-types"></a>통신 유형

클라이언트와 서비스는 각각 다른 시나리오 및 목표를 대상으로 지정하여 다양한 유형의 통신을 통해 통신합니다. 처음에, 그러한 통신 유형은 두 가지 축으로 분류할 수 있습니다.

첫 번째 축은 프로토콜이 동기 또는 비동기인지를 정의합니다.

- 동기 프로토콜. HTTP는 동기 프로토콜입니다. 클라이언트가 요청을 보내고 서비스로부터 응답을 기다립니다. 이는 동기(스레드 차단됨) 또는 비동기(스레드 차단 안 됨, 응답은 결국 콜백에 도달함)가 될 수 있는 클라이언트 코드 실행과는 관계가 없습니다. 여기서 중요한 점은 프로토콜(HTTP/HTTPS)이 동기화되었으며 클라이언트 코드는 HTTP 서버 응답을 받을 때 해당 작업을 계속할 수 있다는 점입니다.

- 비동기 프로토콜. AMQP와 같은 다른 프로토콜(많은 운영 체제 및 클라우드 환경에서 지원되는 프로토콜)은 비동기 메시지를 사용합니다. 일반적으로 클라이언트 코드 또는 메시지를 보낸 사람은 응답을 기다리지 않습니다. 단순히 RabbitMQ 큐나 다른 메시지 브로커에 메시지를 보낼 때처럼 메시지를 보냅니다.

두 번째 축은 통신에 단일 수신기 또는 여러 수신기가 있는지 정의합니다.

- 단일 수신기. 각 요청은 정확히 하나의 수신기 또는 서비스에서 처리되어야 합니다. 이 통신의 예는 [명령 패턴](https://en.wikipedia.org/wiki/Command_pattern)입니다.

- 여러 수신기. 각 요청을 처리하는 수신기는 없거나 여러 개일 수 있습니다. 이 유형의 통신은 비동기여야 합니다. [이벤트 기반 아키텍처](https://microservices.io/patterns/data/event-driven-architecture.html)와 같은 패턴에서 사용되는 [게시/구독](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) 메커니즘이 그 예입니다. 이는 데이터 업데이트를 이벤트를 통해 여러 마이크로 서비스 사이에 전파하는 경우 이벤트 버스 인터페이스 또는 메시지 브로커를 기반으로 합니다. 보통 Service Bus 또는 [항목과 구독](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)을 사용하여 [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)와 같은 비슷한 아티팩트를 통해 구현됩니다.

마이크로 서비스 기반 애플리케이션은 이러한 통신 스타일의 조합을 종종 사용합니다. 가장 일반적인 유형은 일반 Web API HTTP 서비스를 호출할 때 HTTP/HTTPS와 같은 동기 프로토콜을 사용하는 단일 수신기 통신입니다. 또한 마이크로 서비스는 마이크로 서비스 간 비동기 통신을 위해 일반적으로 메시징 프로토콜을 사용합니다.

이러한 축은 가능한 통신 메커니즘을 확실히 아는 데 유용하지만, 마이크로 서비스를 빌드할 때 중요한 고려 사항은 아닙니다. 클라이언트 스레드 실행의 비동기 특성은 물론, 선택한 프로토콜의 비동기 특성조차도 마이크로 서비스를 통합할 때 중요한 사안이 아닙니다. 중요한 *것은* 다음 섹션에 설명되어 있듯이 마이크로 서비스의 독립성을 유지 관리하면서 마이크로 서비스를 비동기적으로 통합할 수 있다는 점입니다.

## <a name="asynchronous-microservice-integration-enforces-microservices-autonomy"></a>비동기 마이크로 서비스 통합은 마이크로 서비스의 자율성을 적용합니다.

언급했듯이 마이크로 서비스 기반 애플리케이션을 빌드할 때 중요한 점은 마이크로 서비스를 통합하는 방법입니다. 이상적으로 내부 마이크로 서비스 간의 통신을 최소화하려고 해야 합니다. 마이크로 서비스 간의 통신이 적을수록 더 좋습니다. 하지만 대부분의 경우 어떻게든 마이크로 서비스를 통합해야 합니다. 이를 수행해야 할 때 여기서 중요한 규칙은 마이크로 서비스 간 통신은 비동기여야 한다는 점입니다. 특정 프로토콜(예를 들어 비동기 메시징 대 동기 HTTP)을 사용해야 한다는 의미는 아닙니다. 마이크로 서비스 간의 통신은 데이터를 비동기적으로 전파해서 수행되어야 하지만, 초기 서비스의 HTTP 요청/응답 작업의 일환으로 다른 내부 마이크로 서비스에 종속되게 하지는 말라는 의미입니다.

가능하면 심지어 쿼리의 경우에도 여러 마이크로 서비스 간 동기 통신(요청/응답)에 종속되지 마십시오. 각 마이크로 서비스의 목표는 엔드투엔드 애플리케이션의 일부로 다른 서비스가 다운되거나 비정상인 경우에도 자율적이며 클라이언트 고객에게 제공되는 것입니다. 클라이언트 애플리케이션에 응답을 제공하기 위해 하나의 마이크로 서비스에서 다른 마이크로 서비스로 호출해야 한다고 생각한다면(예: 데이터 쿼리에 대한 HTTP 요청 수행), 일부 마이크로 서비스가 실패하는 경우 회복되지 않는 아키텍처가 있습니다.

또한 그림 4-15의 첫 부분에 나와 있듯이 HTTP 요청 체인을 통해 긴 요청/응답 주기를 만들 때처럼 마이크로 서비스 간 HTTP 종속성이 있으면 마이크로 서비스가 자치적이지 않게 되며, 해당 체인의 서비스 중 하나가 제대로 수행되지 않는 즉시 성능에 영향을 받습니다.

쿼리 요청과 같은 마이크로 서비스 간 동기 종속성을 추가하면 할수록 클라이언트 앱에 대한 전체 응답 시간은 악화됩니다.

![동기 통신에서 클라이언트 요청을 처리하는 동안 마이크로 서비스 간에 요청의 "체인"이 생성됩니다. 이는 안티 패턴입니다. 비동기 통신 마이크로 서비스에서는 비동기 메시지 또는 http 폴링을 사용하여 다른 마이크로 서비스와 통신하지만, 클라이언트 요청은 즉시 처리됩니다.](./media/image15.png)

**그림 4-15**. 마이크로 서비스 간 통신의 안티 패턴 및 패턴

마이크로 서비스를 다른 마이크로 서비스의 추가 기능으로 만드는 경우, 가능하면 작업을 동기적으로, 그리고 원래 마이크로 서비스 요청 및 응답 작업의 일환으로 수행하지 마십시오. 대신, 비동기적으로 수행합니다(비동기 메시징 또는 통합 이벤트, 큐 등을 사용). 단, 되도록 원래 동기 요청 및 응답 작업의 일환으로 작업을 동기적으로 호출하지 마십시오.

마지막으로(그리고 마이크로 서비스를 빌드할 때 여기서 대부분의 문제가 발생함), 원래 다른 마이크로 서비스가 소유하는 데이터가 초기 마이크로 서비스에 필요한 경우 해당 데이터에 대한 동기 요청 만들기에 의존하지 마세요. 대신, 최종 일관성(일반적으로 통합 이벤트를 사용, 다음 섹션에 설명됨)을 사용하여 해당 데이터(필요한 특성만)를 초기 서비스의 데이터베이스에 복제하거나 전파합니다.

[각 마이크로 서비스의 도메인 모델 경계 식별](identify-microservice-domain-model-boundaries.md) 섹션에서 앞서 설명한 것처럼, 여러 마이크로 서비스에서 일부 데이터의 중복은 잘못된 디자인이 아닙니다. 반면, 이 경우 데이터를 특정 언어 또는 해당 추가 도메인 또는 바인딩된 컨텍스트의 용어로 변환할 수 있습니다. 예를 들어 [eShopOnContainers 애플리케이션](https://github.com/dotnet-architecture/eShopOnContainers)에 User(사용자)라는 엔터티가 있는 사용자의 데이터 대부분을 담당하는 identity.api라는 마이크로 서비스가 있습니다. 단, Ordering(주문) 마이크로 서비스 내에서 사용자에 대한 데이터를 저장해야 하는 경우 Buyer(구매자)라는 다른 엔터티로 저장합니다. Buyer(구매자) 엔터티는 원래 User(사용자) 엔터티와 동일한 ID를 공유하지만, 전체 사용자 프로필이 아닌 Ordering(주문) 도메인에 필요한 몇몇 특성만 포함할 수 있습니다.

최종 일관성을 가지기 위해 프로토콜을 사용하여 마이크로 서비스 사이에 데이터를 비동기적으로 통신하고 전파할 수 있습니다. 언급했듯이 이벤트 버스 또는 메시지 브로커를 사용하여 통합 이벤트를 사용하거나, 다른 서비스를 대신 폴링하여 HTTP를 사용할 수도 있습니다. 이는 중요하지 않습니다. 중요한 규칙은 마이크로 서비스 간 동기 종속성을 만들지 않는 것입니다.

다음 섹션에서는 마이크로 서비스 기반 애플리케이션에서 사용을 고려할 수 있는 여러 통신 스타일에 대해 설명합니다.

## <a name="communication-styles"></a>통신 스타일

사용하려는 통신 유형에 따라 통신에 사용할 수 있는 여러 프로토콜 및 선택 항목이 있습니다. 동기 요청/응답 기반 통신 메커니즘을 사용하는 경우, HTTP 및 REST 방식과 같은 프로토콜이 특히 Docker 호스트나 마이크로 서비스 클러스터 외부에 서비스를 게시하는 경우 가장 일반적입니다. 내부적으로 서비스 간에 통신하는 경우(Docker 호스트나 마이크로서비스 클러스터 내) 이진 형식 통신 메커니즘을 사용할 수도 있습니다(예: TCP 및 이진 형식을 사용하는 WCF). 또는 AMQP 같은 비동기 메시지 기반 통신 메커니즘을 사용할 수 있습니다.

또한 JSON 또는 XML과 같은 여러 메시지 형식, 더 효율적일 수 있는 이진 형식까지 있습니다. 선택한 이진 형식이 표준이 아닌 경우 해당 형식을 사용하여 서비스를 공개적으로 게시하는 것은 좋지 않을 수도 있습니다. 마이크로 서비스 간의 내부 통신에 비표준 형식을 사용할 수도 있습니다. 이를 Docker 호스트나 마이크로서비스 클러스터 내 마이크로서비스 간 통신할 때(예: Docker 오케스트레이터) 또는 마이크로서비스에 통신하는 고유의 클라이언트 애플리케이션에서 수행할 수 있습니다.

### <a name="requestresponse-communication-with-http-and-rest"></a>HTTP와 REST를 사용한 요청/응답 통신

클라이언트가 요청/응답 통신을 사용하는 경우 요청을 서비스에 보낸 다음, 서비스가 요청을 처리하고 다시 응답을 보냅니다. 요청/응답 통신은 특히 클라이언트 앱에서 실시간 UI(라이브 사용자 인터페이스)에 대한 데이터를 쿼리하는 데 적합합니다. 따라서 그림 4-16에 표시된 것처럼 마이크로 서비스 아키텍처에서는 대부분의 쿼리에 이 통신 메커니즘을 사용하게 됩니다.

![마이크로 서비스의 응답이 매우 짧은 시간 내에 도착한다는 가정 하에 클라이언트가 요청을 API 게이트웨이에 보낼 때 라이브 쿼리에 요청/응답 통신을 사용할 수 있습니다.](./media/image16.png)

**그림 4-16**. HTTP 요청/응답 통신 사용(동기 또는 비동기)

클라이언트가 요청/응답 통신을 사용하는 경우 응답이 짧은 시간에(일반적으로 1초 미만이거나 최대 수 초) 도착한다고 가정합니다. 지연된 응답의 경우 다음 섹션에서 설명하는 다른 접근 방식인 [메시징 패턴](https://docs.microsoft.com/azure/architecture/patterns/category/messaging) 및 [메시징 기술](https://en.wikipedia.org/wiki/Message-oriented_middleware)을 기반으로 한 비동기 통신을 구현해야 합니다.

요청/응답 통신을 위한 인기 있는 아키텍처 스타일은 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer)입니다. 이 방법은 [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) 프로토콜을 기반으로 하며 밀접하게 결합되어 있으며 GET, POST 및 PUT과 같은 HTTP 동사를 수용합니다. REST는 서비스를 만들 때 가장 흔히 사용되는 아키텍처 통신 방법입니다. ASP.NET Core Web API 서비스를 개발하는 경우 REST 서비스를 구현할 수 있습니다.

인터페이스 정의 언어로 HTTP REST 서비스를 사용하는 경우 추가 값이 있습니다. 예를 들어, [Swagger 메타데이터](https://swagger.io/)를 사용하여 서비스 API를 설명하는 경우 서비스를 직접 검색하고 사용할 수 있는 클라이언트 스텁을 생성하는 도구를 사용할 수 있습니다.

### <a name="additional-resources"></a>추가 자료

- **Martin Fowler. Richardson Maturity 모델** REST 모델의 설명입니다. \
  <https://martinfowler.com/articles/richardsonMaturityModel.html>

- **Swagger** 공식 사이트입니다. \
  <https://swagger.io/>

### <a name="push-and-real-time-communication-based-on-http"></a>HTTP 기반 푸시 및 실시간 통신

다른 가능성(일반적으로 REST와는 다른 용도용)은 [ASP.NET SignalR](https://www.asp.net/signalr)과 같은 더 높은 수준의 프레임워크와 [Websocket](https://en.wikipedia.org/wiki/WebSocket)과 같은 프로토콜을 사용한 실시간 및 일대다 통신입니다.

그림 4-17에서 볼 수 있듯이 실시간 HTTP 통신은 서버가 클라이언트에서 새 데이터를 요청하길 기다리도록 하는 대신, 서버 코드가 콘텐츠를 연결된 클라이언트로 푸시하도록 할 수 있음을 의미합니다.

![SignalR은 백 엔드 서버에서 클라이언트로 콘텐츠를 푸시하기 위한 실시간 통신을 달성하는 좋은 방법입니다.](./media/image17.png)

**그림 4-17**. 일대일 실시간 비동기 메시지 통신

통신이 실시간이므로 클라이언트 앱은 변경 내용을 거의 즉시 표시합니다. 이는 일반적으로 많은 Websocket 연결을 사용하여(클라이언트당 하나) Websocket과 같은 프로토콜에서 처리됩니다. 일반적인 예는 서비스가 스포츠 경기 점수의 변경 내용을 여러 클라이언트 웹앱에 동시에 전달하는 경우입니다.

>[!div class="step-by-step"]
>[이전](direct-client-to-microservice-communication-versus-the-api-gateway-pattern.md)
>[다음](asynchronous-message-based-communication.md)
