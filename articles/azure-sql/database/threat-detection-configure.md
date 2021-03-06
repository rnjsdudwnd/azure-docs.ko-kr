---
title: Advanced Threat Protection 구성
description: Advanced Threat Protection은의 데이터베이스에 대 한 잠재적인 보안 위협을 나타내는 비정상적인 데이터베이스 활동을 검색 Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-dt-2019, sqldbrb=1
ms.topic: how-to
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 12/01/2020
ms.openlocfilehash: 1425003c718ca52c0bea712e9d25cd3e4c035cf1
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96453971"
---
# <a name="configure-advanced-threat-protection-for-azure-sql-database"></a>Azure SQL Database에 대 한 Advanced Threat Protection 구성
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure SQL Database에 대 한 [Advanced Threat Protection](threat-detection-overview.md) 은 비정상적인 활동을 감지 하 여 데이터베이스에 액세스 하거나 악용 하려는 비정상적인 시도를 감지 합니다. Advanced Threat Protection은 **잠재적 sql 삽입**, **비정상적인 위치 또는 데이터 센터에서 액세스**, **익숙하지 않은 사용자 또는 잠재적으로 유해한 응용 프로그램에서 액세스**, **무차별 암호 대입 SQL 자격 증명** 등을 식별할 수 있습니다. [advanced threat protection 경고](threat-detection-overview.md#alerts)에서 자세한 정보를 참조 하세요.

[전자 메일 알림을](threat-detection-overview.md#explore-detection-of-a-suspicious-event) 통해 검색 된 위협에 대 한 알림을 받거나 [Azure Portal](threat-detection-overview.md#explore-alerts-in-the-azure-portal) 수 있습니다.

[Advanced Threat Protection](threat-detection-overview.md) 은 고급 sql 보안 기능을 위한 통합 패키지인 [Sql 용 Azure Defender](azure-defender-for-sql.md) 제공의 일부입니다. 고급 위협 방지는 SQL portal 용 중앙 Azure Defender를 통해 액세스 하 고 관리할 수 있습니다.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Azure Portal에서 Advanced Threat Protection 설정

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. 보호 하려는 서버의 구성 페이지로 이동 합니다. 보안 설정에서 **Security Center** 를 선택 합니다.
3. **Azure Defender FOR SQL** 구성 페이지에서 다음을 수행 합니다.

   - 서버에서 **SQL 용 Azure Defender를** 사용 하도록 설정 합니다.
   - **Advanced Threat Protection 설정** 에서 **경고 보내기** 텍스트 상자에서 비정상 데이터베이스 작업을 검색할 때 보안 경고를 받을 전자 메일 목록을 제공 합니다.
   
   :::image type="content" source="media/azure-defender-for-sql/set-up-advanced-threat-protection.png" alt-text="advanced threat protection 설정":::

## <a name="set-up-advanced-threat-protection-using-powershell"></a>PowerShell을 사용하여 Advanced Threat Protection 설정

스크립트 예제는 [PowerShell을 사용 하 여 감사 및 고급 위협 방지 구성](scripts/auditing-threat-detection-powershell-configure.md)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [Advanced Threat Protection](threat-detection-overview.md)에 대해 자세히 알아보세요.
- [SQL Managed Instance의 Advanced Threat Protection](../managed-instance/threat-detection-configure.md)에 대해 자세히 알아보세요.  
- [SQL 용 Azure Defender](azure-defender-for-sql.md)에 대해 자세히 알아보세요.
- [감사](../../azure-sql/database/auditing-overview.md)에 대해 자세히 알아보기
- [Azure security center](../../security-center/security-center-introduction.md) 에 대 한 자세한 정보
- 가격 책정에 대 한 자세한 내용은 [SQL Database 가격 책정 페이지](https://azure.microsoft.com/pricing/details/sql-database/) 를 참조 하세요.