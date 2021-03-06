---
title: 'Azure Express 경로: Global Reach Express 경로 구성: CLI'
description: Express 경로 회로를 함께 연결 하 여 온-프레미스 네트워크 간에 개인 네트워크를 만들고 Azure CLI를 사용 하 여 Global Reach를 사용 하도록 설정 하는 방법을 알아봅니다.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 01/07/2021
ms.author: duau
ms.custom: devx-track-azurecli
ms.openlocfilehash: 55f1e4c030dc0e7c8d0011dcff0cc1c9f8fec06e
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99260234"
---
# <a name="configure-expressroute-global-reach-by-using-the-azure-cli"></a>Azure CLI를 사용 하 여 Express 경로 Global Reach 구성

이 문서는 Azure CLI를 사용하여 Azure ExpressRoute Global Reach를 구성하는 데 도움이 됩니다. 자세한 내용은 [ExpressRoute Global Reach](expressroute-global-reach.md)를 참조하세요.
 
구성을 시작하기 전에 다음 요구 사항을 완료합니다.

* 최신 버전의 Azure CLI를 설치합니다. [Azure CLI 설치](/cli/azure/install-azure-cli) 및 [Azure CLI 시작](/cli/azure/get-started-with-azure-cli)을 참조하세요.
* ExpressRoute 회로 프로비전 [워크플로](expressroute-workflows.md)를 이해합니다.
* ExpressRoute 회로가 '프로비저닝됨' 상태인지 확인합니다.
* Azure 프라이빗 피어링이 ExpressRoute 회로에 구성되어 있는지 확인합니다.  

### <a name="sign-in-to-your-azure-account"></a>Azure 계정에 로그인

구성을 시작하려면 Azure 계정에 로그인합니다. 다음 명령은 기본 브라우저를 열고 Azure 계정에 대한 로그인 자격 증명을 묻는 메시지를 표시합니다.  

```azurecli
az login
```

Azure 구독이 여러 개인 경우 해당 계정에 대한 구독을 확인합니다.

```azurecli
az account list
```

사용하려는 구독을 지정합니다.

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>구성에 대한 ExpressRoute 회로 식별

두 개의 Express 경로 회로 간에 Express 경로 Global Reach를 사용 하도록 설정할 수 있습니다. 회로는 지원 되는 국가/지역에 있어야 하며 서로 다른 피어 링 위치에서 만들어졌습니다. 구독에서 두 회로를 모두 소유 하는 경우에는 회로 중 하나를 선택 하 여 구성을 실행할 수 있습니다. 그러나 두 회로가 다른 Azure 구독에 있는 경우 회로 중 하나에서 인증 키를 만들어야 합니다. 첫 번째 회로에서 생성 된 권한 부여 키를 사용 하 여 두 번째 회로에서 Global Reach를 사용 하도록 설정할 수 있습니다.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>온-프레미스 네트워크 간 연결 사용

연결을 사용하도록 설정하는 명령을 실행하는 경우 매개 변수 값에 대한 다음 요구 사항에 유의하세요.

* *peer-circuit* 은 전체 리소스 ID여야 합니다. 예:

  > /subscriptions/{your_subscription_id}/Stggggggggggggs your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering

* *주소-접두사* 는 "/29" IPv4 서브넷 이어야 합니다 (예: "10.0.0.0/29"). 두 ExpressRoute 회로 간의 연결을 설정하는 데 이 서브넷의 IP 주소가 사용됩니다. Azure 가상 네트워크 또는 온-프레미스 네트워크에서이 서브넷의 주소를 사용할 수 없습니다.

다음 CLI 명령을 실행하여 두 ExpressRoute 회로를 연결합니다.

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

CLI 출력은 다음과 같습니다.

```output
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

이 작업이 완료되면 두 ExpressRoute 회로를 통해 양쪽에 있는 온-프레미스 네트워크 간에 연결됩니다.

## <a name="enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions"></a>다른 Azure 구독에서 ExpressRoute 회로 간의 연결 사용

두 회로가 동일한 Azure 구독에 있지 않으면 권한 부여가 필요합니다. 다음 구성에서는 회로 2의 구독에서 권한 부여를 생성 합니다. 그런 다음 인증 키를 회로 1에 전달 합니다.

1. 권한 부여 키를 생성합니다.

   ```azurecli
   az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
   ```

   CLI 출력은 다음과 같습니다.

   ```output
   {
     "authorizationKey": "<authorizationKey>",
     "authorizationUseStatus": "Available",
     "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
     "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
     "name": "<AuthorizationName>",
     "provisioningState": "Succeeded",
     "resourceGroup": "<Circuit2ResourceGroupName>",
     "type": "Microsoft.Network/expressRouteCircuits/authorizations"
   }
   ```

1. 회로 2에 대한 리소스 ID와 권한 부여 키를 모두 적어 둡니다.

1. 회로 2에 대한 리소스 ID와 권한 부여 키를 전달하여 회로 1에 대해 다음 명령을 실행합니다.

   ```azurecli
   az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
   ```

이 작업이 완료되면 두 ExpressRoute 회로를 통해 양쪽에 있는 온-프레미스 네트워크 간에 연결됩니다.

## <a name="get-and-verify-the-configuration"></a>구성을 가져와서 확인

다음 명령을 사용하여 구성이 수행된 회로(예: 앞 예제의 회로 1)의 구성을 확인합니다.

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

CLI 출력에서 *CircuitConnectionStatus* 가 표시됩니다. 두 회로 간의 연결이 설정되었는지("연결됨"), 아니면 설정되지 않았는지("연결 끊김") 여부를 알려줍니다. 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>온-프레미스 네트워크 간 연결을 사용 안 함

연결을 사용하지 않도록 설정하려면 구성이 수행된 회로(예: 앞 예제의 회로 1)에 대해 이 명령을 실행합니다.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

```show``` 명령을 사용하여 상태를 확인합니다.

이 작업이 완료되면 ExpressRoute 회로를 통해 온-프레미스 네트워크 간에 더 이상 연결되지 않습니다.

## <a name="next-steps"></a>다음 단계

* [ExpressRoute Global Reach에 대해 자세히 알아봅니다.](expressroute-global-reach.md)
* [Express 경로 연결 확인](expressroute-troubleshooting-expressroute-overview.md)
* [ExpressRoute 회로를 가상 네트워크에 연결](expressroute-howto-linkvnet-arm.md)
