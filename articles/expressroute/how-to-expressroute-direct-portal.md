---
title: 'Azure Express 경로: Express 경로 직접 구성: 포털'
description: 이 페이지는 포털을 사용 하 여 Express 경로 다이렉트를 구성 하는 데 도움이 됩니다.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: acbd5c3aa88c2c8c14407ebda0c42d228aa6c9e3
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98018941"
---
# <a name="create-expressroute-direct-using-the-azure-portal"></a>Azure Portal를 사용 하 여 Express 경로 직접 만들기

이 문서에서는 Azure Portal를 사용 하 여 Express 경로 직접을 만드는 방법을 보여 줍니다.
Express 경로 직접 사용을 통해 전 세계에 분산 된 피어 링 위치에서 Microsoft의 글로벌 네트워크에 직접 연결할 수 있습니다. 자세한 내용은 [ExpressRoute Direct 정보](expressroute-erdirect-about.md)를 참조하세요.

## <a name="before-you-begin"></a><a name="before"></a>시작하기 전 주의 사항

Express 경로 다이렉트를 사용 하기 전에 먼저 구독을 등록 해야 합니다. 등록하려면 다음 세부 사항을 포함하여 구독 ID로 <ExpressRouteDirect@microsoft.com>에 이메일을 전송합니다.

* **ExpressRoute Direct** 를 사용하여 수행하려는 시나리오
* 위치 기본 설정 - 모든 위치의 전체 목록은 [파트너 및 피어링 위치](expressroute-locations-providers.md) 참조
* 구현을 위한 타임라인
* 기타 궁금한 점이 있는 경우

등록 한 후에는 **Microsoft 네트워크** 리소스 공급자가 구독에 등록 되어 있는지 확인 합니다. 리소스 공급자를 등록하면 구독이 리소스 공급자에서 작동하도록 구성됩니다.

1. [Azure 리소스 공급자 및 형식](../azure-resource-manager/management/resource-providers-and-types.md)에 설명 된 대로 구독 설정에 액세스 합니다.
1. 구독에서 **리소스 공급자** 에 대해 **Microsoft 네트워크** 공급자가 **등록** 된 상태를 표시 하는지 확인 합니다. 등록 된 공급자 목록에 Microsoft 네트워크 리소스 공급자가 없는 경우 추가 합니다.

## <a name="create-expressroute-direct"></a><a name="create-erdir"></a>Express 경로 직접 만들기

1. [Azure Portal](https://portal.azure.com) 메뉴 또는 **홈** 페이지에서 **리소스 만들기** 를 선택 합니다.

1. **새로 만들기** 페이지의 **_Marketplace 검색_*_ 필드에서 _* Express 경로 Direct를 입력** 한 다음 **Enter 키** 를 선택 하 여 검색 결과를 가져옵니다.

1. 결과에서 **express 경로 직접** 을 선택 합니다.

1. Express 경로 **직접** 페이지에서 **만들기** 를 선택 하 여 **express 경로 직접 만들기** 페이지를 엽니다.

1. **기본** 페이지의 필드를 완료 하 여 시작 합니다.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/basics.png" alt-text="기본 페이지":::

    * **구독**: 새 Express 경로 다이렉트를 만드는 데 사용할 Azure 구독입니다. ExpressRoute Direct 리소스와 ExpressRoute 회로가 동일한 구독에 있어야 합니다.
    * **리소스 그룹**: 새 Express 경로 직접 리소스가 생성 되는 Azure 리소스 그룹입니다. 기존 리소스 그룹이 없는 경우 새 리소스 그룹을 만들 수 있습니다.
    * **지역**: 리소스가 만들어질 Azure 공용 지역입니다.
    * **이름**: 새 Express 경로 직접 리소스의 이름입니다.

1. 그런 다음 **구성** 페이지의 필드를 완료 합니다.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/configuration.png" alt-text="' 구성 ' 탭이 선택 된 ' Express 경로 직접 만들기 ' 페이지가 표시 된 스크린샷":::

    * **피어 링 위치**: Express 경로 직접 리소스에 연결 하는 피어 링 위치입니다. 피어 링 위치에 대 한 자세한 내용은 [express 경로 위치](expressroute-locations-providers.md)를 검토 하십시오.
   * **대역폭**: 예약 하려는 포트 쌍 대역폭입니다. Express 경로 다이렉트는 10gb 및 100 Gb 대역폭 옵션을 모두 지원 합니다. 지정 된 피어 링 위치에서 원하는 대역폭을 사용할 수 없는 경우 [Azure Portal에서 지원 요청을 엽니다](https://aka.ms/azsupt).
   * **캡슐화**: Express 경로 Direct는 QinQ 및 Dot1Q 캡슐화를 모두 지원 합니다.
     * QinQ를 선택한 경우 ExpressRoute Direct 리소스 전체에서 고유하게 식별되는 S-Tag가 각 ExpressRoute 회로에 동적으로 할당됩니다.
     *  회로의 각 C-Tag는 회로에서 고유해야 하지만 ExpressRoute Direct 전체에서 고유할 필요는 없습니다.
     * Dot1Q 캡슐화가 선택된 경우 전체 ExpressRoute Direct 리소스에서 C-Tag(VLAN)의 고유성을 관리해야 합니다.
     >[!IMPORTANT]
     >ExpressRoute Direct에는 한 가지 유형으로만 캡슐화할 수 있습니다. ExpressRoute Direct를 만든 후에는 캡슐화를 변경할 수 없습니다.
     >

1. 리소스 태그를 지정 하 고 **검토 + 만들기** 를 선택 하 여 Express 경로 직접 리소스 설정의 유효성을 검사 합니다.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/validate.png" alt-text="' 검토 + 만들기 ' 탭이 선택 된 ' Express 경로 만들기 ' 페이지를 보여 주는 스크린샷":::

1. **만들기** 를 선택합니다. 배포가 진행되고 있음을 알리는 메시지가 표시됩니다. 리소스가 생성되면 이 페이지에 상태가 표시됩니다. 

## <a name="generate-the-letter-of-authorization-loa"></a><a name="authorization"></a>권한 부여의 문자를 생성 합니다 (LOA).

지금은 포털에서 권한 부여의 문자를 생성할 수 없습니다. 권한 부여의 문자를 얻으려면 **[Azure PowerShell](expressroute-howto-erdirect.md#authorization)** 를 사용 합니다.

## <a name="change-admin-state-of-links"></a><a name="state"></a>링크의 관리 상태 변경

이 프로세스를 사용하여 계층 1 테스트를 수행하고 각 교차 연결이 1차 및 2차 포트에 대한 각 라우터에 제대로 패치되도록 합니다.

1. Express 경로 직접 리소스 **개요** 페이지의 **링크** 섹션에서 **link1** 를 선택 합니다.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/link.png" alt-text="링크 1" lightbox="./media/how-to-expressroute-direct-portal/link-expand.png":::

1. **관리 상태** 설정을 **사용** 으로 전환 하 고 **저장** 을 선택 합니다.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/state.png" alt-text="관리자 상태":::

    >[!IMPORTANT]
    >각 링크에서 관리 상태를 사용 하도록 설정 하면 청구가 시작 됩니다.
    >

1. **Link2** 에 대해 동일한 프로세스를 반복 합니다.

## <a name="create-a-circuit"></a><a name="circuit"></a>회로 만들기

기본적으로 ExpressRoute Direct 리소스가 있는 구독에서 10개의 회로를 만들 수 있습니다. 이 숫자는 지원에 따라 증가할 수 있습니다. 사용자는 프로비전된 대역폭과 사용된 대역폭을 둘 다 추적할 책임이 있습니다. 프로 비전 된 대역폭은 Express 경로 직접 리소스의 모든 회로에 대 한 대역폭의 합계입니다. 사용된 대역폭은 기본 실제 인터페이스의 실제 사용량입니다.

* 위에 설명된 시나리오를 지원에 한해 ExpressRoute Direct에서 사용할 수 있는 추가 회로 대역폭은 40 Gbps 및 100 Gbps입니다.

* 지역, 표준 또는 프리미엄 일 수 있습니다.

* 고가 Unlimiteddata 여야 합니다. Express 경로 직접에서는 무제한이 지원 되지 않습니다.

다음 단계를 통해 Express 경로 직접 워크플로에서 Express 경로 회로를 만들 수 있습니다. 대신, 일반 회로 워크플로를 사용 하 여 회로를 만들 수도 있습니다. 단,이 구성에 대 한 일반 회로 워크플로 단계를 사용 하는 경우에는 장점이 없습니다. [Express 경로 회로 만들기 및 수정](expressroute-howto-circuit-portal-resource-manager.md)을 참조 하세요.

1. Express 경로 직접 **설정** 섹션에서 **회로** 를 선택 하 고 **+ 추가** 를 선택 합니다. 

    :::image type="content" source="./media/how-to-expressroute-direct-portal/add.png" alt-text="스크린샷에 선택 된 회로와 추가 강조 표시 된 Express 경로 설정을 보여 줍니다." lightbox="./media/how-to-expressroute-direct-portal/add-expand.png":::

1. **구성** 페이지에서 설정을 구성 합니다.

   :::image type="content" source="./media/how-to-expressroute-direct-portal/configuration2.png" alt-text="구성 페이지-Express 경로 직접":::

1. 리소스를 만들기 전에 값의 유효성을 검사 하기 위해 리소스 태그를 지정 하 고 **검토 + 만들기** 를 선택 합니다.

   :::image type="content" source="./media/how-to-expressroute-direct-portal/review.png" alt-text="검토 및 만들기-Express 경로 직접":::

1. **만들기** 를 선택합니다. 배포가 진행되고 있음을 알리는 메시지가 표시됩니다. 리소스가 생성되면 이 페이지에 상태가 표시됩니다. 

## <a name="next-steps"></a>다음 단계

Express 경로 직접에 대 한 자세한 내용은 [개요](expressroute-erdirect-about.md)를 참조 하세요.
