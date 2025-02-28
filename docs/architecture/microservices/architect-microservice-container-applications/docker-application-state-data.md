---
title: Docker 애플리케이션의 상태 및 데이터
description: Docker 애플리케이션의 상태 및 데이터 관리. 마이크로 서비스 인스턴스는 소모용(그러나 데이터는 아님)이며 마이크로 서비스로 이를 처리하는 방법입니다.
ms.date: 09/20/2018
ms.openlocfilehash: bd0ac007479dcd51f2c639881273b81d1fd8b6d7
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71039588"
---
# <a name="state-and-data-in-docker-applications"></a>Docker 애플리케이션의 상태 및 데이터

대부분의 경우에 컨테이너를 프로세스의 인스턴스로 간주할 수 있습니다. 프로세스는 영구 상태를 유지하지 않습니다. 로컬 스토리지에 컨테이너를 작성할 수 있지만 인스턴스가 무기한 유지된다는 가정은 메모리의 단일 위치에 내구성이 있다고 가정하는 것과 같습니다. 프로세스와 같은 컨테이너 이미지는 여러 인스턴스가 있거나 결국 종료될 것이라고 가정해야 합니다. 컨테이너 오케스트레이터를 사용하여 관리되는 경우에 노드 또는 VM 간에 이동할 수 있다고 가정해야 합니다.

다음 솔루션은 Docker 애플리케이션에서 영구 데이터를 관리하는 데 사용됩니다.

Docker 호스트에서 [Docker 볼륨](https://docs.docker.com/engine/admin/volumes/)으로:

- **볼륨**은 Docker에서 관리되는 호스트 파일 시스템의 영역에 저장됩니다.

- **탑재 바인딩**은 호스트 파일 시스템의 모든 폴더에 매핑할 수 있으므로 Docker 프로세스에서 액세스를 제어할 수 없으며, 컨테이너가 중요한 OS 폴더에 액세스할 수 있기 때문에 보안 위험이 발생할 수 있습니다.

- **tmpfs 탑재**는 호스트 메모리에만 존재하고 파일 시스템에 기록되지 않는 가상 폴더와 같습니다.

원격 스토리지에서:

- [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)는 지역 배포 가능한 스토리지를 제공하여 컨테이너에 대한 장기 지속성 솔루션을 제공합니다.

- [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)와 같은 원격 관계형 데이터베이스 또는 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)와 같은 NoSQL 데이터베이스 또는 [Redis](https://redis.io/)와 같은 캐시 서비스입니다.

Docker 컨테이너에서:

> Docker는 *오버레이 파일 시스템*이라는 기능을 제공합니다. 컨테이너의 루트 파일 시스템에 업데이트된 정보를 저장하는 쓰기 중 복사 작업을 구현합니다. 해당 정보는 컨테이너가 기반으로 하는 원본 이미지에 추가됩니다. 시스템에서 컨테이너를 삭제하는 경우 해당 변경 내용이 손실됩니다. 따라서 로컬 스토리지 내에 컨테이너의 상태를 저장할 수 있지만 이렇게 시스템을 디자인하면 기본적으로는 상태 비저장인 컨테이너 디자인의 전제와 충돌합니다.
>
> 그러나 이전에 도입된 Docker 볼륨은 이제 로컬 데이터 Docker를 처리하는 기본 방법입니다. 컨테이너의 스토리지에 대한 자세한 정보가 필요한 경우 [Docker 스토리지 드라이버](https://docs.docker.com/storage/storagedriver/select-storage-driver/) 및 [스토리지 드라이버 정보](https://docs.docker.com/storage/storagedriver/)에서 확인하세요.

이러한 옵션에 대한 자세한 내용은 다음과 같습니다.

**볼륨**은 컨테이너에 있는 호스트 OS에서 디렉터리에 매핑된 디렉터리입니다. 컨테이너의 코드에 디렉터리에 대한 액세스 권한이 있는 경우 실제로 호스트 OS의 디렉터리에 액세스할 수 있습니다. 이 디렉터리는 컨테이너 자체의 수명과 관련이 없으며, 디렉터리는 Docker에 의해 관리되고 호스트 머신의 핵심 기능과 격리되어 있습니다. 따라서 데이터 볼륨은 컨테이너의 수명과 독립적으로 데이터를 유지하도록 설계되었습니다. Docker 호스트에서 컨테이너 또는 이미지를 삭제하는 경우 데이터 볼륨에서 유지되는 데이터는 삭제되지 않습니다.

볼륨은 이름을 지정하거나 익명화할 수 있습니다(기본값). 명명된 볼륨은 **데이터 볼륨 컨테이너**의 발전이며 컨테이너 간에 데이터를 쉽게 공유할 수 있도록 해줍니다. 또한 볼륨은 다른 옵션 중에서 원격 호스트에 데이터를 저장할 수 있는 볼륨 드라이버를 지원합니다.

**탑재 바인딩**은 오래 전부터 사용 가능하고 컨테이너의 탑재 지점에 모든 폴더를 매핑할 수 있습니다. 탑재 바인딩은 볼륨보다 더 많은 제한 사항과 몇 가지 중요한 보안 문제가 있으므로 볼륨이 권장되는 옵션입니다.

**tmpfs 탑재**는 기본적으로 호스트 메모리에만 존재하고 파일 시스템에 기록되지 않는 가상 폴더입니다. 이들은 빠르고 안전하지만 메모리를 사용하며 비영구적 데이터에만 사용됩니다.

그림 4-5와 같이 일반 Docker 볼륨은 호스트 서버 또는 VM의 물리적 장벽 내부가 아닌 컨테이너 외부에 저장될 수 있습니다. 그러나 Docker 컨테이너는 호스트 서버 또는 VM 간의 볼륨에 액세스할 수 없습니다. 즉, 이러한 볼륨을 사용하면 여러 Docker 호스트에서 실행되는 컨테이너 간에 공유되는 데이터를 관리할 수 없지만 원격 호스트를 지원하는 볼륨 드라이버를 사용하여 수행할 수 있습니다.

![볼륨을 컨테이너 간에 공유할 수 있지만 원격 호스트를 지원하는 원격 드라이버를 사용하지 않는 한 동일한 호스트에서만 공유할 수 있습니다.](./media/image5.png)

**그림 4-5** 컨테이너 기반 애플리케이션에 대한 볼륨 및 외부 데이터 원본

또한 오케스트레이터에서 Docker 컨테이너를 관리할 때 컨테이너는 클러스터에서 수행한 최적화에 따라 호스트 간에 "이동"할 수 있습니다. 따라서 비즈니스 데이터에 데이터 볼륨을 사용하는 것을 권장하지 않습니다. 그러나 추적 파일, 임시 파일 또는 비즈니스 데이터 일관성에 영향을 주지 않는 유사 파일로 작업하는 적합한 메커니즘입니다.

컨테이너 없이 개발할 때 사용하는 것과 동일한 방식으로 Azure SQL Database와 같은 **원격 데이터 원본 및 캐시** 도구, Azure Cosmos DB 또는 Redis와 같은 원격 캐시를 컨테이너화된 애플리케이션에서 사용할 수 있습니다. 비즈니스 애플리케이션 데이터를 저장하는 검증된 방법입니다.

**Azure Storage.** 일반적으로 비즈니스 데이터는 Azure Storage와 같은 외부 리소스 또는 데이터베이스에 배치되어야 합니다. 구체적으로 Azure Storage에서는 클라우드에서 다음 서비스를 제공합니다.

- Blob 스토리지는 구조화되지 않은 개체 데이터를 저장합니다. Blob는 문서 또는 미디어 파일(이미지, 오디오 및 비디오 파일)과 같은 텍스트 또는 이진 데이터 형식일 수 있습니다. Blob 스토리지를 개체 스토리지라고도 합니다.

- 파일 스토리지는 표준 SMB 프로토콜을 사용하여 레거시 애플리케이션에 대한 공유 스토리지를 제공합니다. Azure 가상 머신 및 클라우드 서비스는 탑재된 공유를 통해 애플리케이션 구성 요소 간에 파일 데이터를 공유할 수 있습니다. 온-프레미스 애플리케이션은 파일 서비스 REST API를 통해 공유된 파일 데이터에 액세스할 수 있습니다.

- 테이블 스토리지는 구조화된 데이터 세트를 저장합니다. 테이블 스토리지는 NoSQL 키 특성 데이터 저장소입니다. 이를 통해 많은 양의 데이터를 신속하게 개발하고 빠르게 액세스할 수 있습니다.

**관계형 데이터베이스 및 NoSQL 데이터베이스** SQL Server, PostgreSQL, Oracle과 같은 관계형 데이터베이스 또는 Azure Cosmos DB, MongoDB와 같은 NoSQL 데이터베이스 등 다양한 외부 데이터베이스가 있습니다. 이러한 데이터베이스는 주체가 완전히 다르므로 이 가이드의 일부로 설명하지 않습니다.

>[!div class="step-by-step"]
>[이전](containerize-monolithic-applications.md)
>[다음](service-oriented-architecture.md)
