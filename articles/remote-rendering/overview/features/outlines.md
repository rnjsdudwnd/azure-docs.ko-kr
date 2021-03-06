---
title: 윤곽 렌더링
description: 선택 영역 윤곽 렌더링을 수행하는 방법을 설명합니다.
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: c04f2312926d3b6d668dff712eedb57d816c8bf3
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99592010"
---
# <a name="outline-rendering"></a>윤곽 렌더링

[계층 구조 상태 재정의 구성 요소](../../overview/features/override-hierarchical-state.md)를 통해 윤곽 렌더링을 추가하여 선택한 개체를 시각적으로 강조 표시할 수 있습니다. 이 챕터에서는 클라이언트 API를 통해 윤곽 렌더링의 글로벌 매개 변수를 변경하는 방법을 설명합니다.

윤곽 속성은 글로벌 설정입니다. 윤곽 렌더링을 사용하는 모든 개체는 동일한 설정을 사용 합니다. 즉, 개체당 윤곽 색을 사용할 수 없습니다.

## <a name="parameters-for-outlinesettings"></a>`OutlineSettings`에 대한 매개 변수

클래스 `OutlineSettings`에는 글로벌 윤곽 속성과 관련된 설정이 포함됩니다. 다음과 같은 멤버를 노출합니다.

| 매개 변수      | Type    | Description                                             |
|----------------|---------|---------------------------------------------------------|
| `Color`          | Color4Ub | 윤곽을 그리는 데 사용되는 색입니다. 알파 부분은 무시됩니다.         |
| `PulseRateHz`    | float   | 초당 윤곽 진동 속도입니다.|
| `PulseIntensity` | float   | 윤곽 펄스 효과의 강도입니다. 펄스가 없는 경우의 0.0과 전체 펄스인 경우의 1.0 사이여야 합니다. 강도는 암시적으로 윤곽의 최소 불투명도를 `MinOpacity = 1.0 - PulseIntensity`로 설정합니다. |

![다른 윤곽선 매개 변수를 사용 하 여 세 번 렌더링 된 개체는 ](./media/outlines.png) `color` 매개 변수를 노랑 (왼쪽)에서 마젠타 (가운데)로, `pulseIntensity` 0에서 0.8 (오른쪽)으로 변경 하는 효과를 의미 합니다.

## <a name="example"></a>예제

다음 코드에서는 API를 통해 윤곽 매개 변수를 설정하는 예제를 보여줍니다.

```cs
void SetOutlineParameters(RenderingSession session)
{
    OutlineSettings outlineSettings = session.Connection.OutlineSettings;
    outlineSettings.Color = new Color4Ub(255, 255, 0, 255);
    outlineSettings.PulseRateHz = 2.0f;
    outlineSettings.PulseIntensity = 0.5f;
}
```

```cpp
void SetOutlineParameters(ApiHandle<RenderingSession> session)
{
    ApiHandle<OutlineSettings> outlineSettings = session->Connection()->GetOutlineSettings();
    Color4Ub outlineColor;
    outlineColor.channels = { 255, 255, 0, 255 };
    outlineSettings->SetColor(outlineColor);
    outlineSettings->SetPulseRateHz(2.0f);
    outlineSettings->SetPulseIntensity(0.5f);
}
```

## <a name="performance"></a>성능

윤곽 렌더링은 렌더링 성능에 상당한 영향을 줄 수 있습니다. 이러한 영향은 지정된 프레임에서 선택한 개체와 선택하지 않은 개체 사이의 화면 공간 관계에 따라 달라집니다.

## <a name="api-documentation"></a>API 설명서

* [C # RenderingConnection OutlineSettings 속성](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.outlinesettings)
* [C + + RenderingConnection:: OutlineSettings ()](/cpp/api/remote-rendering/renderingconnection#outlinesettings)

## <a name="next-steps"></a>다음 단계

* [계층 상태 재정의 구성 요소](../../overview/features/override-hierarchical-state.md)