---
title: Windows Azure 진단를 사용 하 여 이벤트 집계
description: Azure Service Fabric 클러스터 모니터링 및 진단을 위해 MAD를 사용하여 이벤트를 집계 및 수집하는 방법에 대해 알아봅니다.
author: srrengar
ms.topic: conceptual
ms.date: 04/03/2018
ms.author: srrengar
ms.openlocfilehash: bcb9ca9e73c0898dc778202eca036a5ae92bebf8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87076136"
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Miscrosoft Azure Diagnostics를 사용하여 이벤트 집계 및 수집
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Azure Service Fabric 클러스터를 실행할 때 모든 노드의 로그를 중앙 위치에 수집하는 것이 좋습니다. 중앙 위치에 로그를 두면 클러스터나 해당 클러스터에서 실행 중인 애플리케이션 및 서비스의 문제를 분석하고 해결하는 데 도움이 됩니다.

로그를 업로드 및 수집하는 방법 중 하나는 WAD(Windows Azure Diagnostics) 확장을 사용하여 Azure Storage에 로그를 업로드하고, Azure Application Insights 또는 Event Hubs에 로그를 보내는 것입니다. 외부 프로세스를 사용 하 여 저장소에서 이벤트를 읽고 [Azure Monitor 로그](./service-fabric-diagnostics-oms-setup.md) 또는 다른 로그 구문 분석 솔루션과 같은 분석 플랫폼 제품에 배치할 수도 있습니다.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>사전 요구 사항
이 문서에서는 다음 도구가 사용됩니다.

* [Azure Resource Manager](../azure-resource-manager/management/overview.md)
* [Azure PowerShell](/powershell/azure/)
* [Azure Resource Manager 템플릿](../virtual-machines/extensions/diagnostics-template.md?toc=/azure/virtual-machines/windows/toc.json)

## <a name="service-fabric-platform-events"></a>Service Fabric 플랫폼 이벤트
Service Fabric은 몇 가지 [기본 로깅 채널](service-fabric-diagnostics-event-generation-infra.md)을 제공하며, 다음 채널은 확장을 사용하여 모니터링 및 진단 데이터를 스토리지 테이블이나 다른 곳에 보내도록 사전 구성되어 있습니다.
  * [작업 이벤트](service-fabric-diagnostics-event-generation-operational.md): Service Fabric 플랫폼이 수행하는 상위 수준 작업입니다. 애플리케이션 및 서비스 만들기, 노드 상태 변경, 업그레이드 정보 등을 예로 들 수 있습니다. 이러한 이벤트를 ETW(Windows용 이벤트 추적) 로그로 내보냅니다.
  * [Reliable Actors 프로그래밍 모델 이벤트](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services 프로그래밍 모델 이벤트](service-fabric-reliable-services-diagnostics.md)

## <a name="deploy-the-diagnostics-extension-through-the-portal"></a>포털을 통해 진단 확장 배포
로그를 수집하는 첫 단계는 Service Fabric 클러스터의 가상 머신 확장 집합 노드에 진단 확장을 배포하는 것입니다. 진단 확장은 각 VM에서 로그를 수집하여 사용자가 지정하는 스토리지 계정에 업로드합니다. 다음 단계는 Azure Portal 및 Azure Resource Manager 템플릿을 통해 신규 및 기존 클러스터에 대해 이 작업을 수행하는 방법을 간략하게 설명합니다.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Azure Portal을 통해 클러스터 만들기의 일환으로 진단 확장 배포
클러스터를 만들 때 클러스터 구성 단계에서 선택적 설정을 확장하고 진단이 **켬**(기본 설정)으로 설정되었는지 확인합니다.

![클러스터를 만들기 위해 포털에서 Azure Diagnostics 설정](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics-new.png)

마지막 단계에서 **만들기를 클릭하기 전에** 템플릿을 다운로드할 것을 강력하게 권장합니다. 자세한 내용은 [Azure Resource Manager 템플릿을 사용하여 Service Fabric 클러스터 설정](service-fabric-cluster-creation-via-arm.md)을 참조하세요. 어떤 채널에서(위에 나열된 채널 중) 데이터를 수집할 것인지 변경하려면 템플릿이 필요합니다.

![클러스터 템플릿](media/service-fabric-diagnostics-event-aggregation-wad/download-cluster-template.png)

이제 Azure Storage에서 이벤트를 집계 하는 중 이므로 [Azure Monitor 로그를 설정](service-fabric-diagnostics-oms-setup.md) 하 여 정보를 얻고 Azure Monitor 로그 포털에서 쿼리 합니다.

>[!NOTE]
>현재 테이블로 전송되는 이벤트를 필터링하거나 영구 제거할 방법은 없습니다. 테이블에서 이벤트를 제거하는 프로세스를 구현하지 않으면 테이블이 계속 커집니다(기본 제한은 50GB). 제한을 변경하는 방법에 대한 지침은 [이 문서의 아래에 자세히 설명](service-fabric-diagnostics-event-aggregation-wad.md#update-storage-quota)되어 있습니다. 또한 [Watchdog 샘플](https://github.com/Azure-Samples/service-fabric-watchdog-service)에서 실행되는 데이터 그루밍 서비스의 예제가 있고, 30일 또는 90일 넘어서 로그를 저장해야 하는 적절한 이유가 없다면 직접 작성하는 것이 좋습니다.



## <a name="deploy-the-diagnostics-extension-through-azure-resource-manager"></a>Azure Resource Manager를 통해 진단 확장 배포

### <a name="create-a-cluster-with-the-diagnostics-extension"></a>진단 확장을 사용하여 클러스터 만들기
Resource Manager를 사용하여 클러스터를 만들려면 진단 구성 JSON을 전체 Resource Manager 템플릿에 추가해야 합니다. Resource Manager 템플릿 샘플의 일부로 진단 구성이 추가된 샘플 5VM 클러스터 Resource Manager 템플릿이 제공됩니다. Azure 샘플 갤러리의 [진단 Resource Manager 템플릿 샘플이 포함된 5노드 클러스터](https://azure.microsoft.com/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/)에서 샘플을 볼 수 있습니다.

Resource Manager 템플릿에서 진단 설정을 표시하려면 azuredeploy.json 파일을 열고 **IaaSDiagnostics**를 검색합니다. 이 템플릿을 사용하여 클러스터를 만들려면 이전 링크에 제공된 **Azure에 배포** 버튼을 선택합니다.

또는 리소스 관리자 샘플을 다운로드하여 변경한 후 Azure PowerShell 창에서 `New-AzResourceGroupDeployment` 명령을 사용하여 수정된 템플릿으로 클러스터를 만들 수 있습니다. 명령에 전달할 매개 변수는 다음 코드를 참조하세요. PowerShell을 사용하여 리소스 그룹을 배포하는 방법에 대한 자세한 내용은 [Azure Resource Manager 템플릿을 사용하여 리소스 그룹 배포](../azure-resource-manager/templates/deploy-powershell.md) 문서를 참조하세요.

### <a name="add-the-diagnostics-extension-to-an-existing-cluster"></a>기존 클러스터에 진단 확장 추가
진단이 배포되지 않은 기존 클러스터가 있는 경우 클러스터 템플릿을 통해 기존 클러스터를 추가하거나 업데이트할 수 있습니다. 기존 클러스터를 만드는 데 사용되는 Resource Manager 템플릿을 수정하거나 앞의 설명대로 포털에서 템플릿을 다운로드합니다. 다음 작업을 수행하여 template.json 파일을 수정합니다.

리소스 섹션에 추가하여 새 스토리지 리소스를 템플릿에 추가합니다.

```json
{
    "apiVersion": "2018-07-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
    "name": "[parameters('applicationDiagnosticsStorageAccountType')]"
    "tier": "standard"
  },
    "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 스토리지 계정 정의 바로 뒤 `supportLogStorageAccountName` 사이의 매개 변수 섹션에 추가합니다. 자리 표시자 텍스트 *저장소 계정 이름* 을 원하는 저장소 계정의 이름으로 바꿉니다.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
그런 다음 확장 배열 내에 다음 코드를 추가하여 template.json의 `VirtualMachineProfile` 섹션을 업데이트합니다. 삽입되는 위치에 따라 시작 부분 또는 끝 부분에 쉼표를 추가해야 합니다.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                },
                {
                    "provider": "02d06793-efeb-48c8-8f7f-09713309a810",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

template.json 파일을 설명대로 수정한 후에는 Resource Manager 템플릿을 다시 게시합니다. 템플릿을 내보낸 후 deploy.ps1 파일을 실행하면 템플릿이 다시 게시됩니다. 배포 후에는 **ProvisioningState**가 **성공**했는지 확인합니다.

> [!TIP]
> 클러스터에 컨테이너를 배포하려는 경우, **WadCfg > DiagnosticMonitorConfiguration** 섹션에 이 컨테이너를 추가하는 방식으로 WAD를 통해 Docker 통계를 선택할 수 있습니다.
>
>```json
>"DockerSources": {
>    "Stats": {
>        "enabled": true,
>        "sampleRate": "PT1M"
>    }
>},
>```

### <a name="update-storage-quota"></a>스토리지 할당량 업데이트

확장으로 채워진 테이블은 할당량에 도달할 때까지 계속 확장되므로 할당량 크기를 줄이는 방안을 고려해 볼 수 있습니다. 기본값은 50GB이며 `DiagnosticMonitorConfiguration` 아래의 `overallQuotaInMB` 필드에 있는 템플릿에서 구성할 수 있습니다.

```json
"overallQuotaInMB": "50000",
```

## <a name="log-collection-configurations"></a>로그 컬렉션 구성
컬렉션에 대한 추가 채널의 로그도 사용할 수 있습니다. 여기서는 Azure에서 실행되는 클러스터용 템플릿에서 수행할 수 있는 가장 일반적인 몇 가지 구성을 소개합니다.

* 작동 채널-기본: 노드에 대 한 이벤트, 배포 되는 새 응용 프로그램 또는 업그레이드 롤백 등을 비롯 하 여 Service Fabric 및 클러스터에서 수행 하는 상위 수준 작업을 기본적으로 사용 하도록 설정 되어 있습니다. 이벤트 목록은 [작업 채널 이벤트](./service-fabric-diagnostics-event-generation-operational.md)를 참조 하세요.
  
```json
      scheduledTransferKeywordFilter: "4611686018427387904"
  ```
* 작동 채널 - 상세: 기본 작동 채널의 모든 내용과 상태 보고서 및 부하 분산 결정이 포함됩니다. 이러한 이벤트는 시스템이나 코드에서 [ReportPartitionHealth](/previous-versions/azure/reference/mt645153(v=azure.100)) 또는 [ReportLoad](/previous-versions/azure/reference/mt161491(v=azure.100))와 같은 상태 또는 부하 보고 API를 사용하여 생성됩니다. Visual Studio의 Diagnostic Event Viewer에서 이 이벤트를 보려면 ETW 공급자 목록에 "Microsoft-ServiceFabric:4:0x4000000000000008"을 추가합니다.

```json
      scheduledTransferKeywordFilter: "4611686018427387912"
  ```

* 데이터 및 메시징 채널 - 기본: 상세 작동 채널 로그 외에 메시징(현재 ReverseProxy만) 및 데이터 경로에 생성된 중요 로그 및 이벤트입니다. 이러한 이벤트는 요청 처리 실패와 ReverseProxy 및 처리된 요청의 기타 중요 문제입니다. **포괄적인 로깅을 위한 권장 사항입니다**. Visual Studio의 진단 이벤트 뷰어에서 이 이벤트를 보려면 ETW 공급자 목록에 “Microsoft-ServiceFabric:4:0x4000000000000010”을 추가합니다.

```json
      scheduledTransferKeywordFilter: "4611686018427387928"
  ```

* 데이터 및 메시징 채널 - 상세: 클러스터 및 상세 작동 채널의 메시징 데이터에서 중요하지 않은 모든 로그를 포함하는 상세 채널입니다. 모든 역방향 프록시 이벤트의 문제 해결에 대한 자세한 내용은 [역방향 프록시 진단 가이드](service-fabric-reverse-proxy-diagnostics.md)를 참조하세요.  Visual Studio의 진단 이벤트 뷰어에서 이 이벤트를 보려면 ETW 공급자 목록에 “Microsoft-ServiceFabric:4:0x4000000000000020”을 추가합니다.

```json
      scheduledTransferKeywordFilter: "4611686018427387944"
  ```

>[!NOTE]
>이 채널에는 많은 이벤트가 있으므로 이 상세 채널에서 이벤트 수집을 설정하면 많은 추적이 빠르게 생성되어 스토리지 용량을 소비할 수 있습니다. 반드시 필요한 경우에만 이 옵션을 설정하세요.


**기본 작동 채널**을 사용하도록 설정하려면 노이즈가 가장 적은 포괄적인 로깅을 사용하는 것이 좋습니다. 템플릿의 `WadCfg`에 있는 `EtwManifestProviderConfiguration`은 다음과 같습니다.

```json
  "WadCfg": {
        "DiagnosticMonitorConfiguration": {
          "overallQuotaInMB": "50000",
          "EtwProviders": {
            "EtwEventSourceProviderConfiguration": [
              {
                "provider": "Microsoft-ServiceFabric-Actors",
                "scheduledTransferKeywordFilter": "1",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableActorEventTable"
                }
              },
              {
                "provider": "Microsoft-ServiceFabric-Services",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableServiceEventTable"
                }
              }
            ],
            "EtwManifestProviderConfiguration": [
              {
                "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                "scheduledTransferLogLevelFilter": "Information",
                "scheduledTransferKeywordFilter": "4611686018427387904",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricSystemEventTable"
                }
              },
              {
                "provider": "02d06793-efeb-48c8-8f7f-09713309a810",
                "scheduledTransferLogLevelFilter": "Information",
                "scheduledTransferKeywordFilter": "4611686018427387904",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                "eventDestination": "ServiceFabricSystemEventTable"
                }
              }
            ]
          }
        }
      },
```

## <a name="collect-from-new-eventsource-channels"></a>새 EventSource 채널에서 수집

배포하려는 새 애플리케이션을 나타내는 새 EventSource 채널에서 로그를 수집하도록 진단을 업데이트하려면 기존 클러스터에 대한 이전 설명과 동일한 단계를 수행해야 합니다.

새 EventSource 채널의 항목을 추가하도록 template.json 파일의 `EtwEventSourceProviderConfiguration` 섹션을 업데이트한 후 `New-AzResourceGroupDeployment` PowerShell 명령을 사용하여 구성 업데이트를 적용합니다. 이벤트 원본의 이름은 ServiceEventSource.cs 파일을 생성한 Visual Studio에서 코드의 일부로 정의됩니다.

예를 들어 이벤트 원본의 이름을 Eventsource라고 지정한 경우 My-Eventsource의 이벤트를 MyDestinationTableName이라는 테이블에 배치하는 다음 코드를 추가합니다.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

성능 카운터 또는 이벤트 로그를 수집하려면 [Azure Resource Manager 템플릿을 사용하여 모니터링 및 진단이 포함된 Windows 가상 머신 만들기](../virtual-machines/extensions/diagnostics-template.md?toc=/azure/virtual-machines/windows/toc.json)에 제공된 예제를 사용하여 Resource Manager 템플릿을 수정합니다. 그런 다음 Resource Manager 템플릿을 다시 게시합니다.

## <a name="collect-performance-counters"></a>성능 카운터 수집

클러스터에서 성능 메트릭을 수집하려면 클러스터에 대한 Resource Manager 템플릿에서 "WadCfg > DiagnosticMonitorConfiguration"에 성능 카운터를 추가합니다. 특정 성능 카운터를 수집하는 `WadCfg` 수정에 대한 단계는 [WAD를 사용하여 성능 모니터링](service-fabric-diagnostics-perf-wad.md)을 참조하세요. 수집을 권장하는 성능 카운터 목록에 대해서는 [Service Fabric 성능 카운터](service-fabric-diagnostics-event-generation-perf.md)를 참조하세요.
  
아래 섹션에 설명된 대로 Application Insights 싱크를 사용하고 있을 때 이러한 메트릭을 Application Insights에 표시하려면 위에 표시된 대로 "sinks" 섹션에 싱크 이름을 추가해야 합니다. Application Insights 리소스에 개별적으로 구성된 성능 카운터를 자동으로 보냅니다.


## <a name="send-logs-to-application-insights"></a>Application Insights에 로그 보내기

### <a name="configuring-application-insights-with-wad"></a>WAD로 Application Insights 구성

>[!NOTE]
>이는 해당 시점의 Windows 클러스터에만 적용됩니다.

WAD에서 Azure 애플리케이션 Insights로 데이터를 전송 하는 두 가지 기본 방법은 Azure Portal를 통해 또는 Azure Resource Manager 템플릿을 통해 Application Insights 싱크를 추가 하 여 수행 하는 것입니다.

#### <a name="add-an-application-insights-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Azure Portal에서 클러스터를 만들 때 Application Insights 계측 키 추가

![AIKey 추가](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

클러스터를 만들 때 진단이 "설정"으로 설정되면 Application Insights 계측 키를 입력할 수 있는 선택 필드가 나타납니다. 여기에 Application Insights 키를 붙여 넣으면 클러스터를 배포하는 데 사용되는 Resource Manager 템플릿에서 Application Insights 싱크가 자동으로 구성됩니다.

#### <a name="add-the-application-insights-sink-to-the-resource-manager-template"></a>Resource Manager 템플릿에 Application Insights 싱크 추가

Resource Manager 템플릿의 "WadCfg"에서 다음 두 가지 변경 사항을 포함하여 "Sink"를 추가합니다.

1. `DiagnosticMonitorConfiguration` 선언이 완료된 직후에 싱크 구성을 추가합니다.

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. `WadCfg`의 `DiagnosticMonitorConfiguration`에 다음 줄을 추가하여(`EtwProviders` 선언 직전) `DiagnosticMonitorConfiguration`에 Sink를 포함합니다.

    ```json
    "sinks": "applicationInsights"
    ```

위의 두 코드 조각에서 "applicationInsights"라는 이름은 싱크를 설명하는 데 사용되었습니다. 이는 요구 사항은 아니며 싱크의 이름이 "sinks"에 포함되어 있는 한 이름을 임의의 문자열로 설정할 수 있습니다.

현재 클러스터의 로그는 Application Insights의 로그 뷰어에 **추적**으로 표시됩니다. 플랫폼에서 발생 하는 대부분의 추적은 "정보" 수준 이므로 "경고" 또는 "오류" 유형의 로그만 보내도록 싱크 구성을 변경할 수도 있습니다. 이 작업은 [이 문서](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)에서 설명한 것처럼 싱크에 "채널"을 추가하여 수행할 수 있습니다.

>[!NOTE]
>포털 또는 Resource Manager 템플릿에서 잘못된 Application Insights 키를 사용하는 경우 수동으로 키를 변경하고 클러스터를 업데이트/재배포해야 합니다.

## <a name="next-steps"></a>다음 단계

Azure Diagnostics를 제대로 구성하면 Storage 테이블에서 ETW 및 EventSource 로그의 데이터를 확인할 수 있습니다. Azure Monitor 로그, Kibana 또는 리소스 관리자 템플릿에 직접 구성 되지 않은 기타 데이터 분석 및 시각화 플랫폼을 사용 하도록 선택 하는 경우 이러한 저장소 테이블에서 데이터를 읽도록 선택한 플랫폼을 설정 해야 합니다. Azure Monitor 로그에 대해이 작업을 수행 하는 것은 비교적 간단 하며 [이벤트 및 로그 분석](service-fabric-diagnostics-event-analysis-oms.md)에서 설명 합니다. Application Insights는 진단 확장 구성의 일부로 구성될 수 있으므로 이런 의미에서 약간 특별한 경우입니다. 따라서 AI를 사용하도록 선택할 경우 [관련 문서](service-fabric-diagnostics-event-analysis-appinsights.md)를 참조하세요.

>[!NOTE]
>현재 테이블로 전송되는 이벤트를 필터링하거나 영구 제거할 방법은 없습니다. 테이블에서 이벤트를 제거하는 프로세스를 구현하지 않으면 테이블이 계속 커집니다. 현재 [Watchdog 샘플](https://github.com/Azure-Samples/service-fabric-watchdog-service)에서 실행되는 데이터 그루밍 서비스의 예제가 있고, 30일 또는 90일 넘어서 로그를 저장해야 하는 적절한 이유가 없다면 직접 작성하는 것이 좋습니다.

* [진단 확장을 사용하여 성능 카운터 또는 로그를 수집하는 방법 알아보기](../virtual-machines/extensions/diagnostics-template.md?toc=/azure/virtual-machines/windows/toc.json)
* [Application Insights를 사용 하 여 이벤트 분석 및 시각화](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Azure Monitor 로그를 사용 하 여 이벤트 분석 및 시각화](service-fabric-diagnostics-event-analysis-oms.md)
* [Application Insights를 사용 하 여 이벤트 분석 및 시각화](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Azure Monitor 로그를 사용 하 여 이벤트 분석 및 시각화](service-fabric-diagnostics-event-analysis-oms.md)
