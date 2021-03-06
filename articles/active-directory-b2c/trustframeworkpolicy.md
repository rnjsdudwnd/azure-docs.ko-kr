---
title: TrustFrameworkPolicy - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C에서 사용자 지정 정책의 TrustFrameworkPolicy 요소를 지정합니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/31/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a87b4c6b845006a9f9f3cf82815277c67c09bef0
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98178843"
---
# <a name="trustframeworkpolicy"></a>TrustFrameworkPolicy

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

사용자 지정 정책은 계층적 체인에서 서로를 참조하는 하나 이상의 XML 형식 파일로 표시됩니다. XML 요소는 클레임 스키마, 클레임 변환, 콘텐츠 정의, 클레임 공급자, 기술 프로필, 사용자 경험 및 오케스트레이션 단계와 같은 정책의 요소를 정의합니다. 각 정책 파일은 정책 파일의 최상위 수준 **TrustFrameworkPolicy** 요소 내에서 정의됩니다.

```xml
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
  ...
```


**TrustFrameworkPolicy** 요소에는 다음 특성이 포함됩니다.

| attribute | 필수 | Description |
|---------- | -------- | ----------- |
| PolicySchemaVersion | Yes | 정책을 실행하는 데 사용할 스키마 버전입니다. 값은 `0.3.0.0`이어야 합니다. |
| TenantObjectId | No | Azure Active Directory B2C (Azure AD B2C) 테 넌 트의 고유 개체 식별자입니다. |
| TenantId | Yes | 이 정책이 속한 테넌트의 고유 식별자입니다. |
| PolicyId | Yes | 정책의 고유 식별자입니다. 이 식별자 앞에는 *B2C_1A_* 가 접두사로 추가되어야 합니다. |
| PublicPolicyUri | Yes | 테넌트 ID와 정책 ID의 조합인 정책 URI입니다. |
| DeploymentMode | No | 가능한 값은 `Production` , 또는 `Development` 입니다. 기본값은 `Production`입니다. 정책을 디버그하려면 이 특성을 사용하세요. 자세한 내용은 [로그 수집](troubleshoot-with-application-insights.md)을 참조 하세요. |
| UserJourneyRecorderEndpoint | No | 로깅에 사용 되는 끝점입니다. 특성이 있으면 값을로 설정 해야 합니다 `urn:journeyrecorder:applicationinsights` . 자세한 내용은 [로그 수집](troubleshoot-with-application-insights.md)을 참조 하세요. |


다음 예제는 **TrustFrameworkPolicy** 요소를 지정하는 방법을 보여 줍니다.

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="https://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkBase"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
```

**TrustFrameworkPolicy** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | Description |
| ------- | ----------- | ----------- |
| BasePolicy| 0:1| 기본 정책의 식별자입니다. |
| [BuildingBlocks](buildingblocks.md) | 0:1 | 정책의 구성 요소입니다. |
| [ClaimsProviders](claimsproviders.md) | 0:1 | 클레임 공급자의 컬렉션입니다. |
| [UserJourneys](userjourneys.md) | 0:1 | 사용자 경험의 컬렉션입니다. |
| [RelyingParty](relyingparty.md) | 0:1 | 신뢰 당사자 정책의 정의입니다. |

다른 정책에서 정책을 상속하려면 **BasePolicy** 요소를 정책 파일의 **TrustFrameworkPolicy** 요소 아래에서 선언해야 합니다. **BasePolicy** 요소는 이 정책이 파생된 기본 정책에 대한 참조입니다.

**BasePolicy** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | Description |
| ------- | ----------- | --------|
| TenantId | 1:1 | Azure AD B2C 테넌트의 식별자입니다. |
| PolicyId | 1:1 | 부모 정책의 식별자입니다. |


다음 예제는 기본 정책을 지정하는 방법을 보여 줍니다. 이 **B2C_1A_TrustFrameworkExtensions** 정책은 **B2C_1A_TrustFrameworkBase** 정책에서 파생됩니다.

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="https://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkExtensions"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkExtensions">

  <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkBase</PolicyId>
  </BasePolicy>
  ...
</TrustFrameworkPolicy>
```

