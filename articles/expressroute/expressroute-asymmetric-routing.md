---
title: 'Azure Express 경로: 비대칭 라우팅'
description: 이 문서에서는 대상에 여러 개의 링크가 있는 네트워크에서 비대칭 라우팅을 사용하여 발생할 수 있는 문제를 살펴봅니다.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 0713c52c7f07db93d9ae9084062ef2c3b25a9074
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97560511"
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>여러 네트워크 경로를 포함한 비대칭 라우팅
이 문서에서는 네트워크 원본 및 대상 간에 여러 경로를 사용할 수 있는 경우 네트워크 트래픽이 다른 경로를 사용 하는 방법을 설명 합니다.

비대칭 라우팅을 이해 하기 위해 알아야 할 두 가지 개념이 있습니다. 첫 번째는 여러 네트워크 경로의 효과입니다. 다른 방법은 방화벽과 같은 장치가 상태를 유지 하는 방법입니다. 이러한 종류의 디바이스는 상태 저장 디바이스라고 합니다. 이러한 두 요소를 결합 하면 상태 저장 장치에서 네트워크 트래픽을 삭제 하는 시나리오를 만들 수 있습니다.  트래픽이 자체에서 생성 된 것을 감지 하지 못해 트래픽이 삭제 됩니다.

## <a name="multiple-network-paths"></a>여러 네트워크 경로
엔터프라이즈 네트워크에 인터넷 서비스 공급자를 통해 인터넷에 대 한 링크가 하나만 있는 경우 인터넷에서 들어오고 나가는 모든 트래픽이 동일한 경로를 이동 합니다. 회사는 네트워크 작동 시간을 개선 하기 위해 중복 경로를 만들기 위해 여러 회로를 구매 하는 것이 일반적입니다. 이 유형의 구성을 사용 하면 트래픽이 인터넷에 대 한 하나의 링크를 이동 하 여 다른 링크를 통해 반환 될 수 있습니다. 이 시나리오를 일반적으로 비대칭 라우팅 이라고 합니다. 비대칭 라우팅에서 반환 네트워크 트래픽은 원래 나가는 흐름과 다른 경로를 사용 합니다.

![여러 경로가 포함된 네트워크](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

일반적으로 비대칭 라우팅은 인터넷으로 이동 하는 경우에 발생 합니다. 여러 경로 조합이 도입 되는 경우에도 발생 합니다. 첫 번째 예는 동일한 대상으로 이동 하는 인터넷 경로와 개인 경로가 있는 경우입니다. 두 번째 예는 동일한 대상으로 이동 하는 여러 개인 경로가 있는 경우입니다.

원본과 대상 간의 경로를 따라 각 라우터는 대상에 도달 하는 데 가장 적합 한 경로를 계산 합니다. 라우터는 다음과 같은 두 가지 주요 요소를 기반으로 가능한 최적의 경로를 결정 합니다.

* 외부 네트워크 간의 라우팅은 BGP(경계 게이트웨이 프로토콜), 라우팅 프로토콜을 기반으로 합니다. BGP는 인접 라우터에서 보급하고 의도한 대상에 최상의 경로를 결정하는 여러 단계를 통해 실행합니다. 최상의 경로를 해당 라우팅 테이블에 저장합니다.
* 경로와 연결된 서브넷 마스크의 길이는 라우팅 경로에 영향을 줍니다. 라우터가 동일한 IP 주소에 대 한 여러 보급 알림을 수신 하는 경우 라우터는 더 긴 서브넷 마스크를 사용 하는 경로를 선택 합니다 .이 경로는 보다 구체적인 경로로 간주 됩니다.

## <a name="stateful-devices"></a>상태 저장 디바이스
라우터는 라우팅을 위해 패킷의 IP 헤더를 살펴봅니다. 패킷 내부를 훨씬 자세히 살펴보는 디바이스가 있습니다. 일반적으로 이러한 장치는 계층 4-TCP (전송 제어 프로토콜) 또는 UDP (사용자 데이터 그램 프로토콜) 또는 계층 7 (응용 프로그램 계층) 헤더를 확인 합니다. 이러한 종류의 디바이스는 보안 디바이스 또는 대역폭 최적화 디바이스입니다. 

방화벽은 상태 저장 디바이스의 일반적인 예입니다. 방화벽은 다양 한 조건에 따라 인터페이스를 통과 하는 패킷을 허용 하거나 거부 합니다. 이러한 조건에는 프로토콜, TCP/UDP 포트 및 URL 헤더가 포함 되지만이에 국한 되지 않습니다. 이 패킷 검사 수준은 장치에 처리량이 많은 부하를 추가할 수 있습니다. 

방화벽은 성능을 개선하기 위해 흐름의 첫 번째 패킷을 검사합니다. 패킷이 인터페이스를 통과할 수 있도록 허용 하는 경우에는 상태 테이블에 흐름 정보를 유지 합니다. 그런 다음 초기 결정에 따라이 흐름과 관련 된 모든 결과 패킷을 허용 합니다. 기존 흐름의 일부인 패킷은 시작 되지 않은 방화벽에 도착할 수 있습니다. 초기 흐름에 대 한 이전 상태 정보가 없으므로 방화벽이 패킷을 삭제 합니다.

## <a name="asymmetric-routing-with-expressroute"></a>ExpressRoute를 포함한 비대칭 라우팅
ExpressRoute를 통해 Microsoft에 연결할 때 네트워크에는 다음과 같은 변경 내용이 발생합니다.

* Microsoft에 대한 여러 링크가 있습니다. 한 링크는 기존 인터넷 연결이 고 다른 하나는 Express 경로 연결을 통해 진행 됩니다. Microsoft를 대상으로 하는 특정 트래픽은 인터넷 연결을 통과할 수 있지만 Express 경로 연결을 통해 반환 될 수 있습니다. 트래픽이 Express 경로를 통과 하지만 인터넷 경로를 통해 반환 하는 경우에도 마찬가지입니다.
* Express 경로 회로에서 더 많은 특정 IP 주소를 받았습니다. 따라서 Express 경로를 통해 제공 되는 서비스를 위해 네트워크의 트래픽이 Microsoft로 이동 하는 경우 라우터는 항상 Express 경로 연결을 선호 합니다.

이러한 두 가지 변경 사항이 네트워크에 미치는 영향을 이해 하기 위해 몇 가지 시나리오를 살펴보겠습니다. 예를 들어 인터넷에 대 한 회로가 있고 인터넷을 통해 모든 Microsoft 서비스를 사용 하는 경우를 예로 들 수 있습니다. 네트워크에서 Microsoft로 들어오고 나가는 트래픽은 동일한 인터넷 링크를 통과 하 여 방화벽을 통과 합니다. 방화벽은 첫 번째 패킷을 볼 때 흐름을 기록 합니다. 흐름이 상태 테이블에 있기 때문에 해당 대화의 모든 결과 패킷이 허용 됩니다.

![ExpressRoute를 포함한 비대칭 라우팅](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

그런 다음 express 경로 회로를 통해 Express 경로를 통해 Microsoft에서 제공 하는 서비스를 사용 합니다. Microsoft의 다른 모든 서비스는 인터넷을 통해 사용 됩니다. Express 경로 연결에 연결 된에 지에 별도의 방화벽을 배포 합니다. Microsoft는 특정 서비스에 대 한 Express 경로를 통해 네트워크에 더 구체적인 접두사를 알립니다. 라우팅 인프라는 해당 접두사에 대한 기본 경로로 ExpressRoute를 선택합니다. 

Express 경로를 통해 Microsoft에 공용 IP 주소를 보급 하지 않는 경우 Microsoft는 인터넷을 통해 공용 IP 주소와 통신 합니다. 네트워크에서 Microsoft로 전송 되는 트래픽은 Express 경로 연결을 사용 하지만 Microsoft의 반환 트래픽은 인터넷 경로를 사용 합니다. 에 지에 있는 방화벽에서 알지 못하는 흐름에 대 한 응답 패킷이 표시 되는 경우 해당 패킷은 삭제 됩니다.

Express 경로 및 인터넷에 대해 동일한 NAT (네트워크 주소 변환) 풀을 보급 하도록 선택 하는 경우 개인 IP 주소에서 네트워크의 클라이언트와 유사한 문제가 표시 됩니다. 이러한 서비스에 대 한 IP 주소는 Express 경로를 통해 보급 되지 않으므로 Windows 업데이트와 같은 서비스에 대 한 요청은 인터넷을 통해 진행 됩니다. 그러나 반환 트래픽은 Express 경로를 통해 다시 제공 됩니다. Microsoft에서 인터넷 및 Express 경로와 동일한 서브넷 마스크를 사용 하 여 IP 주소를 받았지만 기본 경로는 항상 Express 경로입니다. 네트워크에 지에서 Express 경로 연결을 향하는 방화벽 또는 다른 상태 저장 장치에 흐름에 대 한 이전 정보가 없는 경우 해당 패킷은 삭제 됩니다.

## <a name="asymmetric-routing-solutions"></a>비대칭 라우팅 솔루션
비대칭 라우팅 문제를 해결 하는 두 가지 옵션을 사용할 수 있습니다. 첫 번째는 라우팅을 통해, 두 번째는 SNAT (원본 기반 NAT)를 사용 하는 것입니다.

### <a name="routing"></a>라우팅
공용 IP 주소가 적절 한 WAN (광역 네트워크) 링크에 보급 되었는지 확인 합니다. 예를 들어, 메일 트래픽에 대 한 인증 트래픽 및 Express 경로에 인터넷을 사용 하려는 경우입니다. Express 경로를 통해 Active Directory Federation Services (AD FS) 공용 IP 주소를 알리지 마세요. 또한 Express 경로를 통해 라우터가 수신 하는 IP 주소에 온-프레미스 AD FS 서버를 노출 하지 않도록 합니다. ExpressRoute를 통해 수신된 경로는 구체적이므로 ExpressRoute를 Microsoft에 대한 인증 트래픽의 기본 경로로 만듭니다. 네트워크에서 라우팅을 수행 하는 방법에 주의 하지 않으면 비대칭 라우팅 문제가 발생할 수 있습니다.

인증에 대해 Express 경로를 사용 하려는 경우 NAT를 사용 하지 않고 Express 경로를 통해 AD FS 공용 IP 주소를 보급 하 고 있는지 확인 합니다. 이러한 방식으로 구성 된 경우 Microsoft에서 시작 하는 트래픽은 Express 경로를 통해 온-프레미스 AD FS 서버에 전달 됩니다. Microsoft로 이동 하는 네트워크의 반환 트래픽은 인터넷을 통한 기본 경로 이므로 Express 경로를 사용 합니다.

### <a name="source-based-nat"></a>원본 기반 NAT
비대칭 라우팅 문제를 해결 하는 또 다른 방법은 SNAT를 사용 하는 것입니다. 예를 들어 Express 경로를 통해 온-프레미스 SMTP (Simple Mail Transfer Protocol) 서버의 공용 IP 주소를 보급 하지 않도록 선택할 수 있습니다. 대신이 유형의 통신에 인터넷을 사용 하려고 합니다. 온-프레미스 SMTP 서버로 이동 하는 Microsoft에서 보낸 요청은 인터넷을 트래버스 합니다. SNAT는 내부 IP 주소에 대한 들어오는 요청입니다. SMTP 서버의 반환 트래픽은 Express 경로를 통해서가 아니라에 지 방화벽 (NAT에 사용 됨)으로 이동 합니다. 결과적으로 반환 트래픽은 인터넷 경로를 사용 합니다.

![원본 기반 NAT 네트워크 구성](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>비대칭 라우팅 감지
경로 추적은 네트워크 트래픽이 예상된 경로를 탐색하도록 하는 가장 좋은 방법입니다. 온-프레미스 SMTP 서버에서 Microsoft로 인터넷 경로를 사용 하는 트래픽이 예상 되는 경우 경로 추적는 SMTP 서버에서 Microsoft 365으로 예상 됩니다. 이 결과는 트래픽이 Express 경로를 중심으로 하는 것이 아니라 네트워크에서 인터넷으로 나가는 지를 확인 합니다.

