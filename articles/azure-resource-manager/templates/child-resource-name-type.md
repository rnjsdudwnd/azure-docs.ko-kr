---
title: 템플릿의 자식 리소스
description: Azure Resource Manager 템플릿 (ARM 템플릿)에서 자식 리소스의 이름 및 형식을 설정 하는 방법을 설명 합니다.
ms.topic: conceptual
ms.date: 12/21/2020
ms.openlocfilehash: a950d72751b829c0a2aa3ba5ca27316a0544d9cc
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97963915"
---
# <a name="set-name-and-type-for-child-resources"></a>자식 리소스의 이름 및 형식 설정

자식 리소스는 다른 리소스의 컨텍스트 내에만 존재 하는 리소스입니다. 예를 들어 가상 [머신을](/azure/templates/microsoft.compute/virtualmachines)사용 하지 않고 [가상 머신 확장](/azure/templates/microsoft.compute/virtualmachines/extensions) 을 사용할 수 없습니다. 확장 리소스는 가상 컴퓨터의 자식입니다.

각 부모 리소스는 특정 리소스 종류만 자식 리소스로 허용합니다. 자식 리소스의 리소스 형식은 부모 리소스에 대 한 리소스 형식을 포함 합니다. 예를 들어 `Microsoft.Web/sites/config` 및 `Microsoft.Web/sites/extensions` 는 둘 다의 자식 리소스 `Microsoft.Web/sites` 입니다. 허용되는 리소스 종류는 부모 리소스의 [템플릿 스키마](https://github.com/Azure/azure-resource-manager-schemas)에서 지정됩니다.

Azure Resource Manager 템플릿 (ARM 템플릿)에서 부모 리소스 또는 부모 리소스 외부에서 자식 리소스를 지정할 수 있습니다. 다음 예제에서는 부모 리소스의 resources 속성 내에 포함 된 자식 리소스를 보여 줍니다.

```json
"resources": [
  {
    <parent-resource>
    "resources": [
      <child-resource>
    ]
  }
]
```

자식 리소스는 5개 수준 깊이까지만 정의할 있습니다.

다음 예제에서는 부모 리소스 외부의 자식 리소스를 보여 줍니다. 부모 리소스가 동일한 템플릿에 배포 되지 않은 경우 또는 [복사](copy-resources.md) 를 사용 하 여 둘 이상의 자식 리소스를 만드는 경우이 방법을 사용할 수 있습니다.

```json
"resources": [
  {
    <parent-resource>
  },
  {
    <child-resource>
  }
]
```

리소스 이름 및 유형에 대해 제공 하는 값은 자식 리소스가 부모 리소스의 내부에 정의 되는지 아니면 외부에 정의 되는지에 따라 달라 집니다.

## <a name="within-parent-resource"></a>부모 리소스 내

부모 리소스 형식 내에서 정의 된 경우 형식 및 이름 값을 슬래시 없이 단일 단어로 서식 지정 합니다.

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

다음 예제에서는 가상 네트워크 및 서브넷을 보여 줍니다. 서브넷은 가상 네트워크에 대 한 리소스 배열에 포함 되어 있습니다. 이름은 **Subnet1** 로 설정 되 고 유형은 **서브넷** 으로 설정 됩니다. 부모 리소스는 자식 리소스를 배포 하기 전에 존재 해야 하므로 자식 리소스가 부모 리소스에 종속 된 것으로 표시 되어 있습니다.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2018-10-01",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    },
    "resources": [
      {
        "type": "subnets",
        "apiVersion": "2018-10-01",
        "name": "Subnet1",
        "location": "[parameters('location')]",
        "dependsOn": [
          "VNet1"
        ],
        "properties": {
          "addressPrefix": "10.0.0.0/24"
        }
      }
    ]
  }
]
```

전체 리소스 형식이 여전히 `Microsoft.Network/virtualNetworks/subnets` 입니다. `Microsoft.Network/virtualNetworks/`는 부모 리소스 종류에서 유추되므로 입력하지 않습니다.

자식 리소스 이름이 **Subnet1** 로 설정 되어 있지만 전체 이름에는 부모 이름이 포함 됩니다. 부모 리소스에서 가정 하기 때문에 **VNet1** 을 제공 하지 않습니다.

## <a name="outside-parent-resource"></a>부모 리소스 외부

부모 리소스 외부에서 정의 된 경우 형식 및 이름을 슬래시로 지정 하 여 부모 형식 및 이름을 포함 합니다.

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

다음 예제에서는 루트 수준에서 정의 된 가상 네트워크 및 서브넷을 보여 줍니다. 서브넷은 가상 네트워크에 대 한 리소스 배열 내에 포함 되지 않습니다. 이름은 **VNet1/Subnet1** 로 설정 되 고 형식은로 설정 됩니다 `Microsoft.Network/virtualNetworks/subnets` . 부모 리소스는 자식 리소스를 배포 하기 전에 존재 해야 하므로 자식 리소스가 부모 리소스에 종속 된 것으로 표시 되어 있습니다.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2018-10-01",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    }
  },
  {
    "type": "Microsoft.Network/virtualNetworks/subnets",
    "apiVersion": "2018-10-01",
    "location": "[parameters('location')]",
    "name": "VNet1/Subnet1",
    "dependsOn": [
      "VNet1"
    ],
    "properties": {
      "addressPrefix": "10.0.0.0/24"
    }
  }
]
```

## <a name="next-steps"></a>다음 단계

* ARM 템플릿을 만드는 방법에 대 한 자세한 내용은 [arm 템플릿의 구조 및 구문 이해](template-syntax.md)를 참조 하세요.
* 리소스를 참조할 때 리소스 이름 형식에 대 한 자세한 내용은 [참조 함수](template-functions-resource.md#reference)를 참조 하세요.
