---
title: Azure Media Services 이벤트에 대응
description: 이 문서에서는 Azure Event Grid를 사용 하 여 Media Services 이벤트를 구독 하는 방법을 설명 합니다.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 0d4479914ffee6cf667a5f6db2fd665baf2b857c
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98895648"
---
# <a name="handling-event-grid-events"></a>Event Grid 이벤트 처리

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Services 이벤트를 사용하면 애플리케이션이 최신 서버리스 아키텍처를 사용하여 다른 이벤트(예: 작업 상태 변경 이벤트)에 대응할 수 있습니다. 복잡한 코드나 비용이 많이 들고 비효율적인 폴링 서비스가 없어도 이렇게 할 수 있습니다. 대신, 이벤트는 [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)를 통해 [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)와 같은 이벤트 처리기로 푸시 되거나 Webhook으로도 푸시 되며, 요금은 사용한 만큼만 청구됩니다. 가격 책정에 대한 자세한 내용은 [Event Grid 가격 책정](https://azure.microsoft.com/pricing/details/event-grid/)을 참조하세요.

Media Services 이벤트의 가용성은 Event Grid [가용성](../../event-grid/overview.md)과 연관되어 있으며, Event Grid가 사용 가능하면 다른 지역에서도 사용 가능해집니다.  

## <a name="media-services-events-and-schemas"></a>이벤트 및 스키마 Media Services

Event Grid는 [이벤트 구독](../../event-grid/concepts.md#event-subscriptions)을 사용하여 이벤트 메시지를 구독자에게 라우팅합니다. Media Services 이벤트에는 데이터 변경에 대응하는 데 필요한 모든 정보가 포함되어 있습니다. Media Services 이벤트는 eventType 속성이 "Microsoft.Media"로 시작하는 것으로 식별할 수 있습니다.

자세한 내용은 [Media Services 이벤트 스키마](media-services-event-schemas.md)를 참조하세요.

## <a name="practices-for-consuming-events"></a>이벤트 사용에 관한 지침

Media Services 이벤트를 처리하는 애플리케이션은 아래 권장되는 몇 가지 지침을 따라야 합니다.

* 동일한 이벤트 처리기로 이벤트를 라우팅하도록 여러 구독이 구성될 수 있으므로, 이벤트가 특정 원본에서 온 것이라고 가정하지 않고 메시지의 토픽을 확인하여 예상하는 스토리지 계정에서 왔음을 확실히 아는 것이 중요합니다.
* 마찬가지로, eventType이 본인이 처리하려는 형식인지 확인하고, 수신된 모든 이벤트가 예상하는 형식일 것이라고 간주하지 않도록 합니다.
* 이해할 수 없는 필드는 무시합니다.  이 지침은 나중에 추가될 수 있는 새로운 기능에 적용하는 데도 도움이 됩니다.
* "제목" 접두사 및 접미사 일치를 사용하여 이벤트를 특정 이벤트로 제한합니다.

> [!NOTE]
> 이벤트는 [SLA (Event Grid Service Level Agreement(서비스 수준 약정))](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/)의 영향을 받습니다. Api를 사용 하 여 이벤트 알림을 받으려면 [.NET sdk](https://github.com/Azure-Samples/media-services-v3-dotnet) 또는 [Java sdk](https://github.com/Azure-Samples/media-services-v3-java)를 사용 하 여 이벤트를 사용 하는 방법에 대 한 예제를 참조 하세요.

## <a name="next-steps"></a>다음 단계

* [이벤트 모니터링-포털](monitor-events-portal-how-to.md)
* [이벤트 모니터링 - CLI](job-state-events-cli-how-to.md)
