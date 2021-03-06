---
title: 결과 세트 캐싱을 사용한 성능 조정
description: Azure Synapse Analytics의 전용 SQL 풀에 대 한 결과 집합 캐싱 기능 개요
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 10/10/2019
ms.author: xiaoyul
ms.reviewer: nidejaco;
ms.custom: azure-synapse
ms.openlocfilehash: d8c6c8d22c059c63fb4f84c84a02a70de30d4ebe
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98678528"
---
# <a name="performance-tuning-with-result-set-caching"></a>결과 세트 캐싱을 사용한 성능 조정

결과 집합 캐싱을 사용 하도록 설정 하면 전용 SQL 풀에서 자동으로 쿼리 결과를 사용자 데이터베이스에 자동으로 캐시 하 여 반복 해 서 사용 합니다.  이렇게 하면 후속 쿼리 실행 시 지속형 캐시에서 직접 결과를 가져올 수 있으므로 재계산이 필요하지 않습니다.   결과 세트 캐싱은 쿼리 성능을 향상시키고 컴퓨팅 리소스 사용량을 줄입니다.  또한 캐시된 결과 세트를 사용하는 쿼리는 동시성 슬롯을 사용하지 않으므로 기존 동시성 제한이 적용되지 않습니다. 보안을 위해 캐시된 결과를 만든 사용자와 동일한 데이터 액세스 권한이 있는 경우에만 캐시된 결과에 액세스할 수 있습니다.  

## <a name="key-commands"></a>주요 명령

[사용자 데이터베이스에 대한 결과 세트 캐싱 설정/해제](/sql/t-sql/statements/alter-database-transact-sql-set-options?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)

[세션에 대한 결과 세트 캐싱 설정/해제](/sql/t-sql/statements/set-result-set-caching-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)

[캐시된 결과 세트의 크기 확인](/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)  

[캐시 정리](/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)

## <a name="whats-not-cached"></a>캐시되지 않는 내용  

데이터베이스에 대해 결과 집합 캐싱이 켜진 경우, 다음 쿼리를 제외하고 캐시가 가득 찰 때까지 모든 쿼리에 대해 결과가 캐시됩니다.

- 기본 테이블의 데이터 또는 쿼리가 변경 되지 않은 경우에도 비결 정적 인 기본 제공 함수 또는 런타임 식으로 쿼리 합니다. 예를 들어 DateTime. Now (), GetDate ()입니다.
- 사용자 정의 함수를 사용하여 쿼리
- 행 수준 보안 또는 열 수준 보안이 설정된 테이블을 사용하는 쿼리
- 행 크기가 64KB보다 큰 데이터를 반환하는 쿼리
- 크기가 큰 데이터(10GB 초과)를 반환하는 쿼리 
>[!NOTE]
> - 명확 하지 않은 일부 함수 및 런타임 식은 동일한 데이터에 대 한 반복적인 쿼리를 결정적으로 수행할 수 있습니다. 예를 들어 ROW_NUMBER ()입니다.  
> - 쿼리 결과 집합의 행 순서/순서가 응용 프로그램 논리에 중요 한 경우 쿼리에서 ORDER BY를 사용 합니다.
> - ORDER BY 열의 데이터가 고유 하지 않은 경우 결과 집합 캐싱이 사용 되는지 여부에 관계 없이 ORDER BY 열에 동일한 값이 있는 행에 대 한 garanteed 행 순서는 없습니다.

> [!IMPORTANT]
> 결과 집합 캐시를 만들고 캐시에서 데이터를 검색 하는 작업은 전용 SQL 풀 인스턴스의 control 노드에서 수행 됩니다.
> 결과 세트 캐싱이 켜져 있을 때 큰 결과 세트(예: 1GB 초과)를 반환하는 쿼리를 실행하면 제어 노드에서 제한이 증가하고 인스턴스의 전체 쿼리 응답 속도가 느려질 수 있습니다.  이러한 쿼리는 일반적으로 데이터 탐색 또는 ETL 작업 중에 사용됩니다. 컨트롤 노드의 스트레스를 방지하고 성능 문제가 발생하는 것을 방지하려면 사용자는 해당 유형의 쿼리를 실행하기 전에 데이터베이스에서 결과 집합 캐싱을 해제해야 합니다.  

쿼리에 대한 결과 세트 캐싱 작업에 소요된 시간 동안 다음 쿼리를 실행합니다.

```sql
SELECT step_index, operation_type, location_type, status, total_elapsed_time, command
FROM sys.dm_pdw_request_steps
WHERE request_id  = <'request_id'>;
```

다음은 결과 세트 캐싱을 사용하지 않도록 설정한 상태로 실행되는 쿼리의 예제 출력입니다.

![위치 유형 및 명령을 포함 하 여 쿼리 결과를 보여 주는 스크린샷](./media/performance-tuning-result-set-caching/query-steps-with-rsc-disabled.png)

다음은 결과 세트 캐싱을 사용하도록 설정한 상태로 실행되는 쿼리의 예제 출력입니다.

![스크린샷은 선택 된 명령을 사용 하 여 쿼리 결과를 보여 줍니다. * from [D W ResultCache D b] 점 D b o는 out 이라고 합니다.](./media/performance-tuning-result-set-caching/query-steps-with-rsc-enabled.png)

## <a name="when-cached-results-are-used"></a>캐시된 결과가 사용되는 경우

캐시된 결과 집합은 다음 요구 사항이 모두 충족되는 경우 쿼리에 다시 사용됩니다.

- 쿼리를 실행하는 사용자는 쿼리에서 참조된 모든 테이블에 액세스할 수 있습니다.
- 새 쿼리와 결과 집합 캐시를 생성한 이전 쿼리가 정확히 일치합니다.
- 캐시된 결과 집합이 생성된 테이블에는 데이터 또는 스키마 변경 내용이 없습니다.

이 명령을 실행하여 결과 캐시 적중 또는 누락으로 쿼리를 실행했는지 확인합니다. result_cache_hit 열은 캐시 적중 시 1을 반환하고, 캐시 누락 시 0을 반환하고, 결과 집합 캐싱이 사용되지 않은 이유에 대해 음수 값을 반환합니다. 자세한 내용은 [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)를 참조하세요.

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

## <a name="manage-cached-results"></a>캐시된 결과 관리

결과 세트 캐시의 최대 크기는 데이터베이스당 1TB입니다.  기본 쿼리 데이터가 변경되면 캐시된 결과가 자동으로 무효화됩니다.  

캐시 제거는 다음 일정에 따라 자동으로 전용 SQL 풀에서 관리 됩니다.

- 결과 세트가 사용되지 않았거나 무효화된 경우 48시간 간격으로
- 결과 세트 캐시가 최대 크기에 가까워질 때

다음 옵션 중 하나를 사용하여 전체 결과 세트 캐시를 수동으로 비울 수 있습니다.

- 데이터베이스에 대한 결과 집합 캐시 기능을 해제
- 데이터베이스에 연결된 상태에서 DBCC DROPRESULTSETCACHE 실행

데이터베이스를 일시 중지해도 캐시된 결과 세트가 비워지지 않습니다.  

## <a name="next-steps"></a>다음 단계

더 많은 개발 팁은 [개발 개요](sql-data-warehouse-overview-develop.md)를 참조하세요.
