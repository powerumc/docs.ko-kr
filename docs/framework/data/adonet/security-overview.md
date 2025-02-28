---
title: 보안 Overview2
ms.date: 03/30/2017
ms.assetid: 33e09965-61d5-48cc-9e8c-3b047cc4f194
ms.openlocfilehash: 4aac564e55b24b2499f861938082a32f30247f91
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70794347"
---
# <a name="security-overview"></a>보안 개요
애플리케이션 보안은 지속적인 프로세스입니다. 새로운 기술로 인해 앞으로 어떠한 공격이 가해질지 예측할 수 없기 때문에 개발자가 애플리케이션이 모든 공격으로부터 안전하다고 장담하는 것은 불가능합니다. 반대로, 아직까지 아무도 시스템의 보안 허점을 발견(또는 발표)하지 않았더라도 허점이 없다고 할 수는 없습니다. 보안 계획은 프로젝트 디자인 단계에서 수립해야 하며 아울러 애플리케이션의 수명이 다할 때까지 이러한 보안을 어떻게 유지할지도 계획해야 합니다.  
  
## <a name="design-for-security"></a>보안을 위한 디자인  
 안전한 애플리케이션을 개발하는 데 있어서 가장 큰 문제 중 하나는 보안이 프로젝트의 코드가 완성된 후에야 구현되는 사후 대책이라는 점입니다. 개발 초기부터 애플리케이션에 보안 기능을 포함해야지 그렇지 않으면 보안에 대해 제대로 고려하지 않게 되므로 결국 안전하지 않은 애플리케이션이 만들어집니다.  
  
 보안을 마지막 단계에 구현하면 소프트웨어가 새로운 제한에 부딪혀 제 기능을 못하거나 예기치 않은 기능을 수용하기 위해 다시 작성되어야 하므로 버그가 많아집니다. 코드 한 줄을 수정할 때마다 새로운 버그가 발생할 가능성이 있습니다. 따라서 개발 프로세스 초기에 보안을 고려하여 새로운 기능 개발과 함께 진행할 수 있도록 해야 합니다.  
  
### <a name="threat-modeling"></a>위협 모델링  
 노출되어 있는 모든 잠재적인 공격을 이해하지 못하면 시스템을 공격으로부터 보호할 수 없습니다. ADO.NET 응용 프로그램에서 보안 위반의 가능성 및 결과를 확인 하려면 *위협 모델링*이라고 하는 보안 위협 평가 프로세스가 필요 합니다.  
  
 위협 모델링은 크게 공격자의 의도 파악, 시스템 보안 특성 결정, 위협 요소 확인이라는 세 단계로 구성됩니다.  
  
 위협 모델링은 애플리케이션의 취약성을 평가하여 가장 위험한 영역, 즉 가장 중요한 데이터가 노출되는 영역을 찾아내기 위한 반복되는 작업입니다. 취약한 영역을 식별한 후에는 심각도 순으로 영역에 등급을 매기고, 위협에 대한 대응책을 마련한 다음 여기에도 우선 순위를 지정합니다.  
  
 자세한 내용은 다음 리소스를 참조하세요.  
  
|리소스|설명|  
|--------------|-----------------|  
|MSDN 보안 개발자 센터의 [위협 모델링](https://go.microsoft.com/fwlink/?LinkId=98353) 사이트|이 페이지의 리소스는 위협 모델링 프로세스를 이해하고 애플리케이션을 안전하게 보호하는 데 사용할 수 있는 위협 모델을 만드는 데 유용한 정보를 제공합니다.|  
  
## <a name="the-principle-of-least-privilege"></a>최소 권한의 원칙  
 애플리케이션을 디자인하고 빌드하고 배포할 때는 애플리케이션이 언제든지 공격을 받을 수 있다는 가능성을 염두에 두어야 합니다. 대개 이러한 공격은 코드를 실행하는 사용자의 권한으로 실행되는 악의적인 코드에서 시작됩니다. 또한 악의적이지 않은 코드를 공격자가 악용하여 공격이 시작되는 경우도 있습니다. 보안 계획을 수립할 때는 항상 발생 가능한 최악의 상황을 가정해야 합니다.  
  
 최소 권한으로 코드를 실행함으로써 코드 주변에 가능한 한 많은 방어벽을 치는 것도 한 가지 방법입니다. 최소 권한의 원칙이란 특정 작업을 수행하는 데 필요한 최소한의 시간 동안 필요한 최소한의 코드에 대해서만 권한을 부여하는 것을 의미합니다.  
  
 안전한 애플리케이션을 만드는 가장 좋은 방법은 아무런 권한 없이 시작한 다음 수행하는 특정 작업에 필요한 최소한의 권한만 추가하는 것입니다. 반대로, 모든 권한으로 시작한 다음 필요하지 않은 개별 권한을 거부하면 무심결에 부여한 필요 이상의 권한으로 인해 보안 허점이 생길 수 있으므로 결국에는 테스트 및 유지 관리가 어려운 안전하지 않은 애플리케이션이 만들어지게 됩니다.  
  
 애플리케이션 보안에 대한 자세한 내용은 다음 리소스를 참조하세요.  
  
|리소스|Description|  
|--------------|-----------------|  
|[응용 프로그램 보안](/visualstudio/ide/securing-applications)|일반 보안 항목에 대한 링크를 제공합니다. 분산 애플리케이션, 웹 애플리케이션, 모바일 애플리케이션 및 데스크톱 애플리케이션을 보호하는 방법에 대해 설명하는 항목에 대한 링크도 제공합니다.|  
  
## <a name="code-access-security-cas"></a>CAS(코드 액세스 보안)  
 CAS(코드 액세스 보안)는 보호된 리소스 및 작업에 대한 코드의 액세스를 제한하는 데 도움이 되는 메커니즘입니다. .NET Framework에서 CAS는 다음 기능을 수행합니다.  
  
- 다양한 시스템 리소스에 액세스하는 데 필요한 권한을 나타내는 권한 및 권한 집합을 정의합니다.  
  
- 권한 집합을 코드 그룹과 연결하여 관리자가 보안 정책을 구성할 수 있도록 합니다.  
  
- 코드가 유용한 권한 및 실행되는 데 필요한 권한을 요청할 수 있도록 하며, 코드가 가질 수 없는 권한을 지정합니다.  
  
- 코드에서 요청한 권한과 보안 정책에서 허용한 작업에 따라 로드된 각 어셈블리에 권한을 부여합니다.  
  
- 코드에서 해당 코드의 호출자가 특정 권한을 갖도록 요구할 수 있도록 합니다.  
  
- 코드의 호출자가 디지털 서명을 갖도록 해당 코드가 요구하여 특정 조직이나 사이트의 호출자만이 보호된 코드를 호출할 수 있도록 합니다.  
  
- 호출 스택에 있는 모든 호출자에게 부여된 권한과 해당 호출자가 가져야 하는 권한을 비교하여 런타임에 코드를 제한합니다.  
  
 공격이 감행될 경우 발생할 수 있는 피해를 최소화하려면 코드 실행에 꼭 필요한 리소스에 대한 액세스 권한만 부여하는 코드 보안 컨텍스트를 선택합니다.  
  
 자세한 내용은 다음 리소스를 참조하세요.  
  
|리소스|설명|  
|--------------|-----------------|  
|[코드 액세스 보안 및 ADO.NET](code-access-security.md)|코드 액세스 보안, 역할 기반 보안, 부분적으로 신뢰할 수 있는 환경 간의 상호 작용을 ADO.NET 애플리케이션의 측면에서 설명합니다.|  
|[코드 액세스 보안](../../misc/code-access-security.md)|.NET Framework의 CAS에 대해 설명하는 추가 항목 링크를 제공합니다.|  
  
## <a name="database-security"></a>데이터베이스 보안  
 데이터 소스에도 최소 권한의 원칙이 적용됩니다. 데이터베이스 보안을 위한 일반적인 지침 중 일부가 아래에 나와 있습니다.  
  
- 최소 권한을 사용하여 계정을 만듭니다.  
  
- 코드 실행 목적만을 위해서는 사용자에게 관리자 계정에 대한 액세스를 허용하지 않습니다.  
  
- 서버측 오류 메시지를 클라이언트 애플리케이션에 반환하지 않습니다.  
  
- 클라이언트와 서버 모두에서 모든 입력의 유효성을 검사합니다.  
  
- 매개 변수화된 명령을 사용하고 동적 SQL 문은 사용하지 않습니다.  
  
- 보안 위험에 대한 알림을 받을 수 있도록 사용 중인 데이터베이스에 대해 보안 감사 및 로깅 기능을 설정합니다.  
  
 자세한 내용은 다음 리소스를 참조하세요.  
  
|리소스|Description|  
|--------------|-----------------|  
|[SQL Server 보안](./sql/sql-server-security.md)|SQL Server를 대상으로 하는 안전한 ADO.NET 애플리케이션을 만드는 지침을 제공하는 애플리케이션 시나리오와 함께 SQL Server 보안에 대해 간략하게 설명합니다.|  
|[데이터 액세스 전략에 대 한 권장 사항](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/8fxztkff(v=vs.90))|데이터 액세스 및 데이터베이스 작업 수행에 대한 권장 방법을 제공합니다.|  
  
## <a name="security-policy-and-administration"></a>보안 정책 및 관리  
 CAS(코드 액세스 보안) 정책을 잘못 관리하면 보안 허점이 생길 수 있습니다. 애플리케이션을 배포한 후에는 새로운 위협이 출현할 때마다 보안 모니터링 기술을 사용하고 위험을 평가해야 합니다.  
  
 자세한 내용은 다음 리소스를 참조하세요.  
  
|리소스|Description|  
|--------------|-----------------|  
|[보안 정책 관리](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/c1k0eed6(v=vs.100))|보안 정책을 만들고 관리하는 작업에 대한 정보를 제공합니다.|  
|[보안 정책 모범 사례](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/sa4se9bc(v=vs.100))|보안 정책을 관리하는 방법에 대해 설명하는 링크를 제공합니다.|  
  
## <a name="see-also"></a>참고자료

- [ADO.NET 응용 프로그램 보안](securing-ado-net-applications.md)
- [.NET의 보안](../../../standard/security/index.md)
- [SQL Server 보안](./sql/sql-server-security.md)
- [ADO.NET 개요](ado-net-overview.md)
