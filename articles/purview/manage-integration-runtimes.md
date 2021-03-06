---
title: 통합 런타임 만들기 및 관리
description: 이 문서에서는 Azure 부서의 범위에서 통합 런타임을 만들고 관리 하는 단계를 설명 합니다.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 02/03/2021
ms.openlocfilehash: 9276f845c95b5e736180159b282ddedc33523c17
ms.sourcegitcommit: 706e7d3eaa27f242312d3d8e3ff072d2ae685956
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99980748"
---
# <a name="create-and-manage-a-self-hosted-integration-runtime"></a>자체 호스팅 통합 런타임 만들기 및 관리

이 문서에서는 Azure 부서의 범위에서 데이터 원본을 검색할 수 있는 자체 호스팅 integration runtime (SHIR)을 만들고 관리 하는 방법을 설명 합니다.

> [!NOTE]
> 부서의 범위 Integration Runtime는 동일한 컴퓨터의 Azure Synapse Analytics 또는 Azure Data Factory Integration Runtime와 공유할 수 없습니다. 분리 된 컴퓨터에 설치 해야 합니다.

## <a name="create-a-self-hosted-integration-runtime"></a>자체 호스팅 Integration Runtime 만들기

1. 부서의 범위 Studio 홈 페이지의 왼쪽 탐색 창에서 **관리 센터** 를 선택 합니다.

2. 왼쪽 창의 **원본 및 스캔** 에서 **Integration** runtime을 선택 하 고 **+ 새로 만들기** 를 선택 합니다.

   :::image type="content" source="media/manage-integration-runtimes/select-integration-runtimes.png" alt-text="IR을 클릭 합니다.":::

3. **Integration runtime 설정** 페이지에서 **자체 호스트** 를 선택 하 여 Self-Hosted IR을 만든 다음, **계속** 을 선택 합니다.

   :::image type="content" source="media/manage-integration-runtimes/select-self-hosted-ir.png" alt-text="새 SHIR을 만듭니다.":::

4. IR의 이름을 입력 하 고 만들기를 선택 합니다.

5. **Integration Runtime 설정** 페이지에서 **수동 설정** 섹션의 단계를 따릅니다. 다운로드 사이트에서 실행 하려는 VM 또는 컴퓨터에 통합 런타임을 다운로드 해야 합니다.

   :::image type="content" source="media/manage-integration-runtimes/integration-runtime-settings.png" alt-text="키 가져오기":::

   - 인증 키를 복사 하 여 붙여넣습니다.

   - 로컬 Windows 컴퓨터의 [Microsoft Integration Runtime](https://www.microsoft.com/download/details.aspx?id=39717) 에서 자체 호스팅 통합 런타임을 다운로드 합니다. 설치 관리자를 실행합니다.

   - **Integration Runtime(자체 호스팅) 등록** 페이지에서 이전에 저장 한 두 키 중 하나를 붙여넣고 **등록** 을 선택 합니다.

     :::image type="content" source="media/manage-integration-runtimes/register-integration-runtime.png" alt-text="입력 키입니다.":::

   - **새 통합 런타임(자체 호스팅) 노드** 페이지에서 **마침** 을 선택합니다.

6. 자체 호스팅 통합 런타임이 성공적으로 등록 되 면 다음 창이 표시 됩니다.

   :::image type="content" source="media/manage-integration-runtimes/successfully-registered.png" alt-text="성공적으로 등록 되었습니다.":::

## <a name="manage-a-self-hosted-integration-runtime"></a>자체 호스팅 integration runtime 관리

**관리 센터** 에서 **통합 런타임** 으로 이동 하 고 IR을 선택한 다음 편집을 클릭 하 여 자체 호스팅 통합 런타임을 편집할 수 있습니다. 이제 설명을 업데이트 하거나, 키를 복사 하거나, 새 키를 다시 생성할 수 있습니다.

:::image type="content" source="media/manage-integration-runtimes/edit-integration-runtime.png" alt-text="IR을 편집 합니다.":::

:::image type="content" source="media/manage-integration-runtimes/edit-integration-runtime-settings.png" alt-text="IR 정보를 편집 합니다.":::

관리 센터에서 **통합 런타임** 으로 이동 하 고 IR을 선택한 다음 **삭제** 를 클릭 하 여 자체 호스팅 통합 런타임을 삭제할 수 있습니다. IR이 삭제 되 면이에 의존 하는 모든 진행 중인 검사가 실패 합니다.

## <a name="next-steps"></a>다음 단계

[검사를 통해 삭제된 자산을 검색하는 방법](concept-detect-deleted-assets.md)
