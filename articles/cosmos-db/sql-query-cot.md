---
title: Azure Cosmos DB 쿼리 언어의 COT
description: Azure Cosmos DB에서 코탄젠트 (COT) SQL 시스템 함수가 지정 된 숫자 식에서 지정 된 각도의 삼각 코탄젠트를 라디안으로 반환 하는 방법에 대해 알아봅니다.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c247ac477f92985fc722f9c06655b6a7474876fb
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339043"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 지정된 숫자 식의 라디안에서 지정된 각도의 삼각 코탄젠트를 반환합니다.  
  
## <a name="syntax"></a>구문
  
```sql
COT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>인수
  
*numeric_expr*  
   숫자 식입니다.  
  
## <a name="return-types"></a>반환 형식
  
  숫자 식을 반환합니다.  
  
## <a name="examples"></a>예
  
  다음 예에서는 지정 된 `COT` 각도의를 계산 합니다.  
  
```sql
SELECT COT(124.1332) AS cot  
```  
  
 결과 집합은 다음과 같습니다.  
  
```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="remarks"></a>설명

이 시스템 함수는 인덱스를 활용 하지 않습니다.

## <a name="next-steps"></a>다음 단계

- [수치 연산 함수 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [시스템 함수 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 소개](introduction.md)
