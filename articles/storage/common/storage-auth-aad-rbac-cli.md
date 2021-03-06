---
title: Azure CLI를 사용 하 여 데이터 액세스를 위한 Azure 역할 할당
titleSuffix: Azure Storage
description: Azure RBAC (역할 기반 액세스 제어)를 통해 Azure CLI를 사용 하 여 Azure Active Directory 보안 주체에 권한을 할당 하는 방법을 알아봅니다. Azure Storage는 Azure AD를 통해 인증에 대 한 기본 제공 및 Azure 사용자 지정 역할을 지원 합니다.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/07/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-azurecli
ms.openlocfilehash: 9b818b8b46b6ac3af98ff5f25ef69335231744cc
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96779042"
---
# <a name="use-azure-cli-to-assign-an-azure-role-for-access-to-blob-and-queue-data"></a>Azure CLI를 사용 하 여 blob 및 큐 데이터에 액세스 하기 위한 Azure 역할을 할당 합니다.

Azure AD (Azure Active Directory)는 azure [역할 기반 access control (AZURE RBAC)](../../role-based-access-control/overview.md)을 통해 보안 리소스에 대 한 액세스 권한을 부여 합니다. Azure Storage는 blob 또는 큐 데이터에 액세스 하는 데 사용 되는 일반 권한 집합을 포함 하는 Azure 기본 제공 역할 집합을 정의 합니다.

Azure AD 보안 주체에 azure 역할을 할당 하는 경우 Azure는 해당 보안 주체에 대 한 해당 리소스에 대 한 액세스 권한을 부여 합니다. 액세스 권한은 구독, 리소스 그룹, 스토리지 계정 또는 개별 컨테이너나 큐의 수준에 범위를 지정할 수 있습니다. Azure AD 보안 주체는 사용자, 그룹, 응용 프로그램 서비스 주체 또는 [azure 리소스에 대 한 관리 되는 id](../../active-directory/managed-identities-azure-resources/overview.md)일 수 있습니다.

이 문서에서는 Azure CLI를 사용 하 여 Azure 기본 제공 역할을 나열 하 고 사용자에 게 할당 하는 방법을 설명 합니다. Azure CLI 사용에 대 한 자세한 내용은 [CLI (Azure Command-Line Interface)](/cli/azure)를 참조 하세요.

## <a name="azure-roles-for-blobs-and-queues"></a>Blob 및 큐에 대 한 Azure 역할

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>리소스 범위 결정

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-azure-roles"></a>사용 가능한 Azure 역할 나열

Azure CLI에서 사용 가능한 Azure 기본 제공 역할을 나열 하려면 [az role definition list](/cli/azure/role/definition#az-role-definition-list) 명령을 사용 합니다.

```azurecli-interactive
az role definition list --out table
```

기본 제공 Azure Storage 데이터 역할이 Azure에 대 한 다른 기본 제공 역할과 함께 나열 됩니다.

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-azure-role-to-a-security-principal"></a>보안 주체에 Azure 역할 할당

보안 주체에 Azure 역할을 할당 하려면 [az role assign create](/cli/azure/role/assignment#az-role-assignment-create) 명령을 사용 합니다. 명령의 형식은 할당 범위에 따라 다를 수 있습니다. 다음 예에서는 다양 한 범위에서 사용자에 게 역할을 할당 하는 방법을 보여 주지만 동일한 명령을 사용 하 여 보안 주체에 역할을 할당할 수 있습니다.

> [!NOTE]
> Azure Storage 계정을 만들면 Azure AD를 통해 데이터에 액세스할 수 있는 권한이 자동으로 할당 되지 않습니다. Azure Storage에 대 한 Azure 역할을 명시적으로 할당 해야 합니다. 구독, 리소스 그룹, 스토리지 계정 또는 컨테이너나 큐 수준으로 지정할 수 있습니다.

### <a name="container-scope"></a>컨테이너 범위

컨테이너에 범위가 지정 된 역할을 할당 하려면 매개 변수의 컨테이너 범위를 포함 하는 문자열을 지정 `--scope` 합니다. 컨테이너의 범위는 다음과 같은 형식입니다.

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container>
```

다음 예에서는 컨테이너 수준으로 범위가 지정 된 사용자에 게 **저장소 Blob 데이터 참가자** 역할을 할당 합니다. 샘플 값과 대괄호 안의 자리 표시자 값을 고유한 값으로 바꿔야 합니다.

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container>"
```

### <a name="queue-scope"></a>큐 범위

범위가 지정 된 역할을 큐에 할당 하려면 매개 변수의 큐 범위를 포함 하는 문자열을 지정 `--scope` 합니다. 큐의 범위는 다음과 같은 형식입니다.

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue>
```

다음 예에서는 큐 수준으로 범위가 지정 된 사용자에 게 **저장소 큐 데이터 참가자** 역할을 할당 합니다. 샘플 값과 대괄호 안의 자리 표시자 값을 고유한 값으로 바꿔야 합니다.

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue>"
```

### <a name="storage-account-scope"></a>저장소 계정 범위

저장소 계정에 범위가 지정 된 역할을 할당 하려면 매개 변수에 대 한 저장소 계정 리소스의 범위를 지정 합니다 `--scope` . 저장소 계정의 범위는 다음과 같은 형식입니다.

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

다음 예제에서는 저장소 계정 수준에서 사용자에 게 **저장소 Blob 데이터 판독기** 역할을 할당 하는 방법을 보여 줍니다. 샘플 값을 고유한 값으로 바꾸어야 합니다.

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Reader" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>리소스 그룹 범위

리소스 그룹에 범위가 지정 된 역할을 할당 하려면 매개 변수의 리소스 그룹 이름 또는 ID를 지정 합니다 `--resource-group` . 다음 예에서는 리소스 그룹 수준에서 사용자에 게 **저장소 큐 데이터 판독기** 역할을 할당 합니다. 괄호 안의 샘플 값과 자리 표시자 값을 고유한 값으로 바꿔야 합니다.

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Reader" \
    --assignee <email> \
    --resource-group <resource-group>
```

### <a name="subscription-scope"></a>구독 범위

구독에 범위가 지정 된 역할을 할당 하려면 매개 변수에 대 한 구독의 범위를 지정 합니다 `--scope` . 구독의 범위는 다음과 같은 형식입니다.

```
/subscriptions/<subscription>
```

다음 예제에서는 저장소 계정 수준에서 사용자에 게 **저장소 Blob 데이터 판독기** 역할을 할당 하는 방법을 보여 줍니다. 샘플 값을 고유한 값으로 바꾸어야 합니다. 

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Reader" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>다음 단계

- [Azure PowerShell 모듈을 사용 하 여 Azure 역할 할당 추가 또는 제거](../../role-based-access-control/role-assignments-powershell.md)
- [Azure PowerShell 모듈을 사용 하 여 blob 및 큐 데이터에 액세스 하기 위한 Azure 역할을 할당 합니다.](storage-auth-aad-rbac-powershell.md)
- [Azure Portal를 사용 하 여 blob 및 큐 데이터에 액세스 하기 위한 Azure 역할을 할당 합니다.](storage-auth-aad-rbac-portal.md)
