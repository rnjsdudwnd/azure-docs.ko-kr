---
title: 스마트 감지 - 성능 이상 | Microsoft Docs
description: Application Insights는 앱 원격 분석의 스마트 분석을 수행하여 잠재적 성능 문제에 대해 경고합니다. 이 기능에는 설정이 필요하지 않습니다.
ms.topic: conceptual
ms.date: 05/04/2017
ms.reviewer: antonfr
ms.openlocfilehash: ead71a5ac6e2cfa0df6654c7ba9aa7b5b9238d04
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763777"
---
# <a name="smart-detection---performance-anomalies"></a>스마트 감지 - 성능 이상

[Application Insights](./app-insights-overview.md)는 웹 애플리케이션의 성능을 자동으로 분석하고 잠재적 문제에 대해 경고할 수 있습니다. 스마트 검색 알림 중 하나를 수신했으므로 읽어볼 수 있습니다.

이 기능을 사용 하려면 [지원 되는 언어](./platforms.md)에 대 한 Application Insights 앱을 구성 하는 것 외에 특별 한 설정이 필요 하지 않습니다. 앱이 충분한 원격 분석을 생성하면 활성화됩니다.

## <a name="when-would-i-get-a-smart-detection-notification"></a>스마트 검색 알림은 언제 제공되나요?

Application Insights가 애플리케이션 성능이 다음과 같은 방식 중 하나로 저하되었음을 감지했습니다.

* **응답 시간 성능 저하** - 앱이 이전보다 더 느리게 요청에 응답하기 시작했습니다. 예를 들어 최신 배포에 문제가 있어서 이러한 변화가 급격하게 나타났을 수 있습니다. 또는 메모리 누수 등으로 인해 변화가 서서히 나타났을 수 있습니다. 
* **종속성 기간 저하** - 앱은 REST API, 데이터베이스 또는 기타 종속성을 호출합니다. 종속성이 이전보다 더 느리게 응답합니다.
* **성능 저하 패턴** - 앱에 일부 요청에만 영향을 미치는 성능 문제가 발생한 것 같습니다. 예를 들어 페이지가 어느 한 유형의 브라우저에서 다른 유형의 브라우저보다 훨씬 더 느리게 로드되거나 요청이 특정 서버에서 훨씬 더 느리게 처리됩니다. 현재 당사 알고리즘은 페이지 로드 시간, 요청 응답 시간 및 종속성 응답 시간을 확인합니다.  

스마트 검색 기능을 사용하려면 정상 성능 기준을 설정하기 위해 실행 가능한 볼륨에 대한 8일 이상의 원격 분석이 필요합니다. 따라서 해당 기간 동안 애플리케이션을 실행한 후 심각한 문제가 발생하면 알림이 제공됩니다.


## <a name="does-my-app-definitely-have-a-problem"></a>내 앱에 분명히 문제가 있나요?

아니요, 알림이 제공된다고 해서 앱에 반드시 문제가 있는 것은 아닙니다. 보다 밀접하게 확인하는 것이 좋겠다는 제안입니다.

## <a name="how-do-i-fix-it"></a>이 문제를 어떻게 해결하나요?

알림에는 진단 정보가 포함되어 있습니다. 예를 들면 다음과 같습니다.


![다음은 서버 응답 시간 성능 저하 검색의 예입니다.](media/proactive-performance-diagnostics/server_response_time_degradation.png)

1. **심사**. 이 알림은 영향을 받은 사용자 수 또는 작업의 수를 보여 줍니다. 문제에 우선 순위를 할당하는 데 도움이 될 수 있습니다.
2. **범위**. 이 문제가 모든 트래픽에 영향을 주나요? 아니면 일부 페이지에만 영향을 주나요? 특정 브라우저 또는 위치로 제한되나요? 이 정보는 알림에서 얻을 수 있습니다.
3. **진단**. 종종 알림의 진단 정보는 문제의 특성을 암시합니다. 예를 들어 요청 속도가 높을 때 응답 시간이 느려지면 서버 또는 종속성이 오버로드된 것을 암시합니다. 

    그렇지 않으면 Application Insights에서 성능 블레이드를 엽니다. 여기서 [프로파일러](profiler.md) 데이터를 찾을 수 있습니다. 예외가 throw되는 경우에는 [스냅샷 디버거](./snapshot-debugger.md)를 사용할 수 있습니다.



## <a name="configure-email-notifications"></a>전자 메일 알림 구성

스마트 검색 알림은 기본적으로 사용되도록 설정되며 Application Insights 리소스에 대해 [모니터링 리더](../../role-based-access-control/built-in-roles.md#monitoring-reader) 및 [모니터링 참가자](../../role-based-access-control/built-in-roles.md#monitoring-contributor) 액세스 권한이 있는 자들에게 전송됩니다. 이를 변경하려면 전자 메일 알림에서 **구성** 을 클릭하거나 Application Insights에서 스마트 검색 설정을 엽니다. 
  
  ![스마트 검색 설정](media/proactive-performance-diagnostics/smart_detection_configuration.png)
  
  * 스마트 검색 전자 메일에서 **구독 취소** 링크를 사용하여 전자 메일 알림 수신을 중지할 수 있습니다.

스마트 검색 성능 이상에 대한 전자 메일은 Application Insights 리소스당 매일 하나의 전자 메일로 제한됩니다. 이 전자 메일은 해당 날에 하나 이상의 새 문제가 검색된 경우에만 전송됩니다. 모든 메시지는 중복으로 보내지 않습니다. 

## <a name="faq"></a>FAQ

* *그렇다면, 내 데이터를 확인하나요?*
  * 아니요. 서비스는 완전 자동입니다. 사용자는 알림만 받게 됩니다. 사용자의 데이터는 [프라이빗](./data-retention-privacy.md)입니다.
* *Application Insights를 통해 수집되는 모든 데이터를 분석하나요?*
  * 현재는 암호화되지 않습니다. 현재는 요청 응답 시간, 종속성 응답 시간 및 페이지 로드 시간을 분석합니다. 추가 메트릭에 대한 분석은 현재 서비스를 위해 작업 중입니다.

* 이러한 분석 기능은 어떤 유형의 애플리케이션에서 사용할 수 있나요?
  * 이러한 성능 저하는 해당 원격 분석을 생성하는 모든 애플리케이션에서 검색됩니다. 웹앱에 Application Insights를 설치한 경우 요청 및 종속성이 자동으로 추적됩니다. 그렇지만 백 엔드 서비스 또는 다른 앱에서 [trackrequest ()](./api-custom-events-metrics.md#trackrequest) 또는 [TrackDependency](./api-custom-events-metrics.md#trackdependency) 호출을 삽입해도 스마트 검색이 동일한 방식으로 작동합니다.

* *나만의 이상 감지 규칙을 만들거나 기존 규칙을 사용자 지정할 수 있나요?*

  * 아직은 가능하지 않지만 다음 작업은 가능합니다.
    * 메트릭 임계값을 초과할 때 알리도록 [경고를 설정](../platform/alerts-log.md)할 수 있습니다.
    * 직접 분석할 수 있는 [데이터베이스](./code-sample-export-sql-stream-analytics.md), [Power BI](./export-power-bi.md)에 [원격 분석을 내보냅니다](./export-telemetry.md).
* *얼마나 자주 분석합니까?*

  * 원격 분석 데이터에 대한 분석은 전날부터 매일 실행됩니다(UTC 표준 시간대로 하루 종일).
* 이것이 *메트릭 경고 [를 대신하나요](../platform/alerts-log.md)?*
  * 아니요.  비정상이라고 간주할만한 동작을 모두 감지한다고 커밋할 수 없습니다.


* *알림에 대한 응답으로 아무 작업도 수행하지 않을 경우 미리 알림이 제공되나요?*
  * 아니요, 각 문제에 대해 한 번만 메시지가 나타납니다. 문제가 지속되면 스마트 검색 피드 블레이드에서 업데이트됩니다.
* *이메일이 삭제되었습니다. 포털의 어디에서 알림을 찾을 수 있습니까?*
  * 앱의 Application Insights 개요에서 **스마트 검색** 타일을 클릭합니다. 여기에서 90일 내의 모든 알림을 찾을 수 있습니다.

## <a name="how-can-i-improve-performance"></a>성능을 어떻게 향상시킬 수 있나요?
느리고 실패한 응답은 경험상 알고 계신대로 웹 사이트 사용자에게 있어 가장 큰 불만 중 하나입니다. 따라서 문제를 해결하는 것이 중요합니다.

### <a name="triage"></a>심사
첫째, 문제가 됩니까? 페이지가 로드하는 작업이 항상 느리지만 사이트의 사용자의 1%만이 페이지를 보았다면 생각해 봐야할 중요한 사항입니다. 반면에 사용자의 1%가 열지만 그 때마다 예외를 throw한다면 조사할 가치가 있습니다.

영향 설명(영향을 받은 사용자 또는 트래픽의 % 비율)을 일반적인 지침으로 사용하되, 이것이 전부가 아니라는 사실에 유의합니다. 확인하려면 다른 증거를 수집합니다.

문제의 매개 변수를 고려합니다. 지리적 위치에 종속적이면 해당 지역을 포함하여 [가용성 테스트](./monitor-web-app-availability.md)를 설정합니다. 단순히 해당 영역의 네트워크 문제일 수 있습니다.

### <a name="diagnose-slow-page-loads"></a>느린 페이지 로드를 진단합니다.
문제는 어디에 있습니까? 느리게 응답하는 서버, 매우 긴 페이지 또는 표시하는 데 많은 작업을 수행하는 브라우저입니까?

브라우저 메트릭 블레이드를 엽니다. 브라우저 페이지 로드 시간의 분할된 표시는 시간의 흐름을 보여줍니다. 

* **보내기 요청 시간** 이 높은 경우 서버가 느리게 응답하거나 데이터가 많은 게시물이 요청된 것입니다. [성능 메트릭](./performance-counters.md) 을 살펴 응답 시간을 조사합니다.
* [종속성 추적](./asp-net-dependencies.md) 을 설정하여 속도 저하의 원인이 외부 서비스인지 아니면 사용자의 데이터베이스인지를 확인합니다.
* **응답 수신** 이 주된 경우 페이지 및 종속 파트 즉, JavaScript, CSS, 이미지 등(그러나 비동기적으로 로드 된 데이터가 아닌)의 길이가 깁니다. [가용성 테스트](./monitor-web-app-availability.md)를 설정하고 종속 파트를 로드하는 옵션을 설정합니다. 일부 결과 얻을 때 결과의 세부 사항을 열고 확장하여 다른 파일의 로드 시간을 확인합니다.
* 긴 **클라이언트 처리 시간** 은 스크립트가 느리게 실행됨을 의미합니다. 이유가 명확하지 않다면 일부 타이밍 코드를 추가하는 것이 좋고 trackMetric 호출에 시간을 보냅니다.

### <a name="improve-slow-pages"></a>느린 페이지 개선
서버 응답 및 페이지 로드 시간을 향상시키는 데 대한 조언이 가득한 웹이 있으므로 여기서 모두 반복하지는 않겠습니다. 다음은 이미 알고 있는 몇 가지 팁을 상기시켜 드립니다.

* 큰 파일로 인한 느린 로딩: 스크립트 및 기타 부분을 비동기적으로 로드합니다. 스크립트 묶음을 사용합니다. 해당 데이터를 별도로 로드하는 위젯에 기본 페이지를 나눕니다. 긴 테이블에 이전의 단순한 HTML을 보내지 않습니다. JSON 또는 다른 압축 형식으로 데이터를 요청하는 스크립트를 사용한 다음 테이블을 채웁니다. 이 모든 것을 도울 유용한 프레임워크가 있습니다. (또한 스크립트가 크다는 제약이 따릅니다.)
* 느린 서버 종속성: 구성 요소의 지리적 위치를 고려합니다. 예를 들어 Azure를 사용하는 경우 웹 서버와 데이터베이스가 동일한 지역에 있는지를 확인합니다. 쿼리가 필요한 양 보다 많은 정보를 검색합니까? 캐싱 또는 배치가 도움이 됩니까?
* 용량 문제: 응답 시간 및 요청 수의 서버 메트릭을 살펴봅니다. 응답 시간이 요청 개수에서 불균형적으로 급증하면 서버가 늘어났을 가능성이 있습니다.


## <a name="server-response-time-degradation"></a>서버 응답 시간 성능 저하

응답 시간 성능 저하 알림은 다음을 알려줍니다.

* 이 작업에 대해 정상적인 응답 시간과 비교한 응답 시간
* 영향을 받은 사용자 수
* 검색한 날 및 이전 7일 동안 이 작업에 대한 평균 응답 시간 및 90 백분위수 응답 
* 검색한 날 및 이전 7일 동안 이 작업 요청의 수
* 이 작업의 성능 저하 및 관련 종속성의 성능 저하 간 상관 관계 
* 문제를 진단하는 데 도움이 되는 링크
  * 작업 시간이 소요된 위치를 파악하는 데 도움이 되는 프로파일러 추적(검색 기간 동안 이 작업에 대해 프로파일러 추적 예제가 수집된 경우 해당 링크를 사용할 수 있음) 
  * 이 작업에 대한 시간 범위/필터를 분석 및 분할할 수 있는 메트릭 탐색기의 성능 보고서
  * 이 호출을 검색하여 특정 호출 속성을 확인합니다.
  * 오류 보고 - 오류 수가 1보다 크면 성능 저하에 기여했을 수 있는 작업 실패가 발생한 것입니다.

## <a name="dependency-duration-degradation"></a>종속성 기간 저하

최신 애플리케이션이 점점 더 많이 마이크로 서비스 디자인 방식을 채택하게 되면서 외부 서비스에 과도하게 의존하는 경우가 많아지고 있습니다. 예를 들어 애플리케이션이 일부 데이터 플랫폼에 의존하거나 직접 bot 서비스를 구축하는 경우에도, 일부 Cognitive Services 공급자에 릴레이하여 bot이 좀 더 인간적인 방식으로 상호 작용하도록 설정하고, 일부 데이터 저장소 서비스에 릴레이하여 bot이 답변을 끌어오도록 할 수 있습니다.  

예제 종속성 성능 저하 알림:

![다음은 종속성 기간 저하 검색 예제입니다.](media/proactive-performance-diagnostics/dependency_duration_degradation.png)

여기서 알 수 있는 정보:

* 이 작업에 대해 정상적인 응답 시간과 비교한 기간
* 영향을 받은 사용자 수
* 검색한 날 및 이전 7일 동안 이 종속성의 평균 기간 및 90 백분위수 기간
* 검색한 날 및 이전 7일 동안의 종속성 호출 수
* 문제를 진단하는 데 도움이 되는 링크
  * 이 종속성에 대한 메트릭 탐색기의 성능 보고서
  * 이 종속성 호출을 검색하여 호출 속성 확인
  * 오류 보고 – 오류 수가 1보다 크면 검색 기간 동안 기간 장기화에 기여했을 수 있는 종속성 호출 실패가 발생한 것입니다. 
  * 이 종속성 기간 및 수를 계산하는 쿼리를 사용하여 분석 열기  

## <a name="smart-detection-of-slow-performing-patterns"></a>느린 성능 패턴에 대한 스마트 검색 

Application Insights는 일부 사용자에게만 영향을 주거나 일부 경우에만 사용자에 영향을 주는 성능 문제를 찾습니다. 예를 들어 페이지가 어느 한 유형의 브라우저에서 다른 유형의 브라우저보다 훨씬 더 느리게 로드되거나 요청이 특정 서버에서 훨씬 더 느리게 처리되는 경우에 대한 알림이 제공될 수 있습니다. 또한 특정 운영 체제를 사용하는 클라이언트에 대해 특정 지리적 영역의 느린 페이지 로드와 같은 속성의 조합과 관련된 문제를 검색할 수 있습니다.  

이러한 이상 상태는 데이터를 검사하여 바로 검색하기가 매우 어렵지만 생각보다 훨씬 일반적입니다. 고객이 불만을 제기할 때에만 알 수 있는 경우가 종종 있습니다. 그때는 너무 늦은 것입니다. 영향을 받은 사용자는 이미 경쟁 제품을 사용하게 됩니다.

현재 당사 알고리즘은 페이지 로드 시간, 서버에서의 요청 응답 시간 및 종속성 응답 시간을 확인합니다.  

임계값을 설정하거나 규칙을 구성할 필요가 없습니다. 기계 학습 및 데이터 마이닝 알고리즘이 비정상적인 패턴을 검색하는 데 사용됩니다.

![Azure에서 전자 메일 경고의 진단 보고서를 열려면 해당 링크를 클릭합니다.](./media/proactive-performance-diagnostics/03.png)

* **When(시간)** 에는 문제가 감지된 시간이 표시됩니다.
* **What(내용)** 에는 다음 설명이 표시됩니다.

  * 감지된 문제
  * 문제가 되는 동작을 표시하는 이벤트의 특징
* 테이블은 성능이 낮은 집합을 다른 모든 이벤트의 평균적인 동작과 비교합니다.

링크를 클릭하여 메트릭 탐색기를 열고 느리게 수행되는 집합의 시간 및 속성으로 필터링된 적적할 보고서를 검색합니다.

시간 범위와 원격 분석을 탐색하는 필터를 수정합니다.

## <a name="next-steps"></a>다음 단계
이러한 진단 도구를 사용하면 앱에서 원격 분석을 검사할 수 있습니다.

* [프로파일러](profiler.md) 
* [스냅숏 디버거](./snapshot-debugger.md)
* [분석](../log-query/log-analytics-tutorial.md)
* [분석 스마트 진단](../log-query/log-query-overview.md)

스마트 감지는 완전히 자동으로 수행됩니다. 하지만 보다 많은 경고를 설정하고 싶을 수 있습니다.

* [수동으로 구성된 메트릭 경고](../platform/alerts-log.md)
* [가용성 웹 테스트](./monitor-web-app-availability.md)