---
title: Custom Speech 모델 학습 및 배포-음성 서비스
titleSuffix: Azure Cognitive Services
description: Custom Speech 모델을 학습 하 고 배포 하는 방법을 알아봅니다. 음성 텍스트 모델을 학습 하면 Microsoft 기준선 모델이 나 사용자 지정 모델에 대해 인식 정확도를 향상 시킬 수 있습니다.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 41fdb3d2e69ae39dbe80f21a953fd9fdaa6d1127
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97968469"
---
# <a name="train-and-deploy-a-custom-speech-model"></a>Custom Speech 모델 학습 및 배포

이 문서에서는 Custom Speech 모델을 학습 하 고 배포 하는 방법을 알아봅니다. 음성 텍스트 모델을 학습 하면 Microsoft 기준선 모델에 대 한 인식 정확도를 향상 시킬 수 있습니다. 모델 학습을 위해 사람이 레이블 지정 된 텍스트 및 관련 텍스트를 사용 합니다. 이러한 데이터 집합은 이전에 업로드 된 오디오 데이터와 함께 음성 텍스트 모델을 구체화 하 고 학습 하는 데 사용 됩니다.

## <a name="use-training-to-resolve-accuracy-problems"></a>학습을 사용 하 여 정확도 문제 해결

기본 모델에서 인식 문제가 발생 하는 경우 사용자 지정 된 기록 및 관련 데이터를 사용 하 여 사용자 지정 모델을 학습 하 고 정확도를 향상 시킬 수 있습니다. 이 표를 사용 하 여 문제를 해결 하는 데 사용할 데이터 집합을 결정 합니다.

| 사용 사례 | 데이터 형식 |
| -------- | --------- |
| 의료 용어 또는 IT 전문 용어와 같은 업계 별 어휘 및 문법에 대 한 인식 정확도 향상 | 관련 텍스트 (문장/길이 발언) |
| 제품 이름 또는 머리글자어와 같이 비표준 발음을 포함 하는 단어 또는 용어의 발음 및 표시 된 형태를 정의 합니다. | 관련 텍스트 (발음) |
| 말하는 스타일, 악센트 또는 특정 배경 소음에 대 한 인식 정확도 향상 | 오디오 + 사람이 레이블 지정 된 성적 증명서 |

## <a name="train-and-evaluate-a-model"></a>모델 학습 및 평가

모델을 학습 하는 첫 번째 단계는 학습 데이터를 업로드 하는 것입니다. 사용자에 게 레이블이 지정 된 텍스트 (길이 발언 및 발음)를 준비 하는 단계별 지침은 [데이터 준비 및 테스트](./how-to-custom-speech-test-and-train.md) 를 참조 하세요. 학습 데이터를 업로드 한 후 다음 지침에 따라 모델에 대 한 학습을 시작 합니다.

1. [Custom Speech 포털](https://speech.microsoft.com/customspeech)에 로그인 합니다. 오디오 + 사람이 레이블이 지정 된 기록 데이터 집합을 사용 하 여 모델을 학습 하려면 교육을 위한 [전용 하드웨어가 있는 지역](custom-speech-overview.md#set-up-your-azure-account) 에서 음성 구독을 선택 합니다.
2. **음성 텍스트**  >  **Custom Speech**  >  **[프로젝트 이름]**  >  **학습** 으로 이동 합니다.
3. **모델 학습** 을 선택 합니다.
4. 교육에 **이름** 및 **설명을** 제공 합니다.
5. **시나리오 및 기준 모델** 목록에서 도메인에 가장 적합 한 시나리오를 선택 합니다. 선택할 시나리오를 잘 모르는 경우에는 **일반** 을 선택 합니다. 기준선 모델은 학습을 위한 시작점입니다. 일반적으로 최신 모델을 선택 하는 것이 가장 좋습니다.
6. **학습 데이터 선택** 페이지에서 학습에 사용할 하나 이상의 관련 텍스트 데이터 집합 또는 오디오 + 사람이 레이블 지정 된 기록 데이터 집합을 선택 합니다. 새 모델을 학습 하는 경우 관련 텍스트를 사용 하 여 시작 합니다. 오디오 + 사용자 레이블이 지정 된 기록을 사용한 교육은 훨씬 더 오래 걸릴 수 있습니다 (최대 [며칠](how-to-custom-speech-evaluate-data.md#improve-model-recognition)).
7. 학습을 완료 한 후에는 새로 학습 된 모델에 대 한 정확도 테스트를 수행할 수 있습니다. 이 단계는 선택 사항입니다.
8. 만들기를 선택 하 여 사용자 지정 모델을 **만듭니다** .

**학습** 테이블에 새 모델에 해당 하는 새 항목이 표시 됩니다. 테이블에는 **처리** 중, **성공** 또는 **실패** 상태가 표시 됩니다.

Custom Speech 모델 정확도를 평가 하 고 개선 [하는 방법을](how-to-custom-speech-evaluate-data.md) 참조 하세요. 정확도를 테스트 하도록 선택 하는 경우 모델에서 사용 하는 것과 다른 음향 데이터 집합을 선택 하 여 모델의 성능을 사실적으로 파악 하는 것이 중요 합니다.

> [!NOTE]
> 기본 모델과 사용자 지정 모델은 모두 특정 날짜 까지만 사용할 수 있습니다 ( [모델 수명 주기](custom-speech-overview.md#model-lifecycle)참조). Speech Studio는 각 모델 및 끝점에 대 한 **만료** 열에이 날짜를 표시 합니다. 끝점 또는 일괄 처리에 대 한 해당 날짜 요청이 실패 하거나 기본 모델로 대체 될 수 있습니다.
>
> 가장 최근의 기본 모델을 사용 하 여 모델을 다시 학습 하 여 정확성 향상을 활용 하 고 모델이 만료 되는 것을 방지 합니다.

## <a name="deploy-a-custom-model"></a>사용자 지정 모델 배포

데이터를 업로드 및 검사 하 고 정확도를 평가 하 고 사용자 지정 모델을 학습 한 후에는 앱, 도구 및 제품과 함께 사용할 사용자 지정 끝점을 배포할 수 있습니다. 

사용자 지정 끝점을 만들려면 [Custom Speech 포털](https://speech.microsoft.com/customspeech)에 로그인 합니다. 페이지 맨 위에 있는 **Custom Speech** 메뉴에서 **배포** 를 선택 합니다. 처음 실행 하는 경우에는 표에 나열 된 끝점이 없다는 것을 알 수 있습니다. 끝점을 만든 후이 페이지를 사용 하 여 배포 된 각 끝점을 추적할 수 있습니다.

그런 다음 **끝점 추가** 를 선택 하 고 사용자 지정 끝점에 대 한 **이름** 및 **설명** 을 입력 합니다. 그런 다음 끝점과 연결 하려는 사용자 지정 모델을 선택 합니다.  이 페이지에서 로깅을 사용 하도록 설정할 수도 있습니다. 로깅을 사용 하면 끝점 트래픽을 모니터링할 수 있습니다. 로깅을 사용 하지 않도록 설정 하면 트래픽이 저장 되지 않습니다.

![새 끝점 페이지를 보여 주는 스크린샷](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> 사용 약관 및 가격 정보에 동의 해야 합니다.

다음 **만들기** 를 선택합니다. 이 작업을 수행 하면 **배포** 페이지로 돌아갑니다. 이제 테이블에 사용자 지정 끝점에 해당 하는 항목이 포함 됩니다. 끝점의 상태는 현재 상태를 표시 합니다. 사용자 지정 모델을 사용 하 여 새 끝점을 인스턴스화하는 데 최대 30 분 정도 걸릴 수 있습니다. 배포 상태가 **완료** 로 변경 되 면 끝점을 사용할 준비가 된 것입니다.

끝점이 배포 된 후에는 끝점 이름이 링크로 표시 됩니다. 끝점 키, 끝점 URL 및 샘플 코드와 같이 끝점과 관련 된 정보를 보려면 링크를 선택 합니다. 만료 날짜를 기록 하 고이 날짜 이전에 끝점의 모델을 업데이트 하 여 중단 없는 서비스를 확인 합니다.

## <a name="view-logging-data"></a>로깅 데이터 보기

**배포** 아래의 끝점 페이지로 이동 하면 로깅 데이터를 내보낼 수 있습니다.
> [!NOTE]
>Microsoft 소유의 저장소에서 30 일 동안 로깅 데이터를 사용할 수 있습니다. 이후에 제거 됩니다. 고객이 소유한 저장소 계정이 Cognitive Services 구독에 연결 된 경우 로깅 데이터가 자동으로 삭제 되지 않습니다.

## <a name="next-steps"></a>다음 단계

* [사용자 지정 모델을 사용 하는 방법 알아보기](how-to-specify-source-language.md)

## <a name="additional-resources"></a>추가 자료

- [데이터 준비 및 테스트](./how-to-custom-speech-test-and-train.md)
- [데이터 검사](how-to-custom-speech-inspect-data.md)
- [데이터 평가](how-to-custom-speech-evaluate-data.md)
