---
title: 모델
description: Azure Remote Rendering에서 모델의 정의를 설명합니다.
author: jakrams
ms.author: jakras
ms.date: 02/05/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: 0d1e66d09db3e3934871ed15493feb685d1cbe6a
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99593877"
---
# <a name="models"></a>모델

Azure Remote Rendering의 ‘모델’은 [엔터티](entities.md) 및 [구성 요소](components.md)로 구성된 전체 개체 표현을 나타냅니다. 모델은 원격 렌더링 서비스에 사용자 지정 데이터를 가져오는 주요 방법입니다.

## <a name="model-structure"></a>모델 구조

모델에는 루트 노드인 [엔터티](entities.md)가 하나만 있습니다. 그 아래에 자식 엔터티의 임의 계층 구조가 있을 수 있습니다. 모델을 로드하면 이 루트 엔터티에 대한 참조가 반환됩니다.

각 엔터티에는 연결된 [구성 요소](components.md)가 있을 수 있습니다. 일반적으로 엔터티는 [메시 리소스](meshes.md)를 참조하는 *MeshComponents* 를 포함합니다.

## <a name="creating-models"></a>모델 만들기

FBX 및 GLTF와 같은 파일 형식의 [입력 모델을 변환](../how-tos/conversion/model-conversion.md)하여 런타임용 모델을 만들 수 있습니다. 변환 프로세스는 질감, 재질 및 메시와 같은 모든 리소스를 추출하고 최적화된 런타임 형식으로 변환합니다. 또한 구조적 정보를 추출하고 이를 ARR의 엔터티/구성 요소 그래프 구조로 변환합니다.

> [!IMPORTANT]
> [모델 변환](../how-tos/conversion/model-conversion.md)은 [메시](meshes.md)를 만드는 유일한 방법입니다. 메시는 런타임에 엔터티 간에 공유될 수 있지만 모델을 로드하는 것 외에는 다른 방법으로 메시를 런타임에 가져올 수 없습니다.

## <a name="loading-models"></a>모델 로드

모델을 변환한 후에는 Azure Blob Storage에서 런타임으로 로드할 수 있습니다.

Blob Storage에서 자산을 처리하는 방법에 따라 다른 두 개의 개별 로딩 함수가 있습니다.

* [Blob Storage가 계정에 연결되어 있으면](../how-tos/create-an-account.md#link-storage-accounts) Blob Storage 매개 변수로 직접 모델을 처리할 수 있습니다. 이 경우 매개 변수 `LoadModelOptions`와 관련된 로딩 함수는 `LoadModelAsync`입니다.
* 모델은 SAS URI를 통해 처리됩니다. 매개 변수 `LoadModelFromSasOptions`와 관련된 로딩 함수는 `LoadModelFromSasAsync`입니다. [기본 제공 모델](../samples/sample-model.md)을 로드하는 경우에도 이 변형을 사용합니다.

다음 코드 조각은 두 함수 중 하나를 사용하여 모델을 로드하는 방법을 보여 줍니다. Blob 저장소 매개 변수를 사용 하 여 모델을 로드 하려면 아래와 같은 코드를 사용 합니다.


```cs
async void LoadModel(RenderingSession session, Entity modelParent, string storageAccount, string containerName, string assetFilePath)
{
    // load a model that will be parented to modelParent
    var modelOptions = new LoadModelOptions(
        storageAccount, // storage account name + '.blob.core.windows.net', e.g., 'mystorageaccount.blob.core.windows.net'
        containerName,  // name of the container in your storage account, e.g., 'mytestcontainer'
        assetFilePath,  // the file path to the asset within the container, e.g., 'path/to/file/myAsset.arrAsset'
        modelParent
    );

    var loadOp = session.Connection.LoadModelAsync(modelOptions, (float progress) =>
    {
        Debug.WriteLine($"Loading: {progress * 100.0f}%");
    });

    await loadOp;
}
```

```cpp
void LoadModel(ApiHandle<RenderingSession> session, ApiHandle<Entity> modelParent, std::string storageAccount, std::string containerName, std::string assetFilePath)
{
    LoadModelOptions modelOptions;
    modelOptions.Parent = modelParent;
    modelOptions.Blob.StorageAccountName = std::move(storageAccount);
    modelOptions.Blob.BlobContainerName = std::move(containerName);
    modelOptions.Blob.AssetPath = std::move(assetFilePath);

    ApiHandle<LoadModelResult> result;
    session->Connection()->LoadModelAsync(modelOptions,
        // completion callback
        [](Status status, ApiHandle<LoadModelResult> result)
        {
            printf("Loading: finished.");
        },
        // progress callback
        [](float progress)
        {
            printf("Loading: %.1f%%", progress * 100.f);
        }
    );
}
```

SAS 토큰을 사용 하 여 모델을 로드 하려면 다음 코드 조각과 비슷한 코드를 사용 합니다.

```cs
async void LoadModel(RenderingSession session, Entity modelParent, string modelUri)
{
    // load a model that will be parented to modelParent
    var modelOptions = new LoadModelFromSasOptions(modelUri, modelParent);

    var loadOp = session.Connection.LoadModelFromSasAsync(modelOptions, (float progress) =>
    {
        Debug.WriteLine($"Loading: {progress * 100.0f}%");
    });

    await loadOp;
}
```

```cpp
void LoadModel(ApiHandle<RenderingSession> session, ApiHandle<Entity> modelParent, std::string modelUri)
{
    LoadModelFromSasOptions modelOptions;
    modelOptions.ModelUri = modelUri;
    modelOptions.Parent = modelParent;

    ApiHandle<LoadModelResult> result;
    session->Connection()->LoadModelFromSasAsync(modelOptions,
        // completion callback
        [](Status status, ApiHandle<LoadModelResult> result)
        {
            printf("Loading: finished.");
        },
        // progress callback
        [](float progress)
        {
            printf("Loading: %.1f%%", progress * 100.f);
        }
    );
}
```

이후에 엔터티 계층 구조를 트래버스하고 엔터티 및 구성 요소를 수정할 수 있습니다. 동일한 모델을 여러 번 로드하면 각각 엔터티/구성 요소 구조의 자체 복사본이 있는 여러 인스턴스가 생성됩니다. 메시, 재질 및 질감이 [공유된 리소스](../concepts/lifetime.md)이므로 데이터는 다시 로드되지 않습니다. 따라서 모델을 두 번 이상 인스턴스화하면 메모리 오버헤드가 비교적 적게 발생합니다.

## <a name="api-documentation"></a>API 설명서

* [C # RenderingConnection ()](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.loadmodelasync)
* [C # RenderingConnection ()](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.loadmodelfromsasasync)
* [C + + RenderingConnection:: LoadModelAsync ()](/cpp/api/remote-rendering/renderingconnection#loadmodelasync)
* [C + + RenderingConnection:: Loadmodel Msasasync ()](/cpp/api/remote-rendering/renderingconnection#loadmodelfromsasasync)

## <a name="next-steps"></a>다음 단계

* [엔터티](entities.md)
* [메시](meshes.md)