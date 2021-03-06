---
title: Advanced Threat Protection-Azure Portal-Azure Database for MySQL
description: 데이터베이스에 대 한 잠재적인 보안 위협을 나타내는 비정상적인 데이터베이스 활동을 검색 하도록 Advanced Threat Protection을 구성 하는 방법에 대해 알아봅니다.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: b30bd36dca6f866b8f3e6e8a0b133a6dd61b239b
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96012666"
---
# <a name="advanced-threat-protection-for-azure-database-for-mysql"></a>Azure Database for MySQL용 Advanced Threat Protection

Azure Database for MySQL용 Advanced Threat Protection은 비정상적이며 유해할 가능성이 있는 데이터베이스 액세스 또는 악용 시도를 나타내는 비정상적인 활동을 검색합니다.

Advanced Threat Detection은 고급 보안 기능용 통합 패키지인 Advanced Data Security 제품의 일부입니다. [Azure Portal](https://portal.azure.com)을 통해 액세스 및 관리할 수 있는 Advanced Threat Protection은 현재 미리 보기로 제공됩니다.

> [!NOTE]
> Advanced Threat Protection 기능을 사용할 수 **없는** Azure 정부 및 소버린 클라우드 지역은 US Gov 텍사스, US Gov 애리조나, US Gov 아이오와, US Gov 버지니아, US DoD 동부, US DoD 중부, 독일 중부, 독일 북부, 중국 동부, 중국 동부 2입니다. 전반적인 제품 사용 가능성을 확인하려면 [지역별 사용 가능한 제품](https://azure.microsoft.com/global-infrastructure/services/)을 참조하세요.
>

> [!NOTE]
> 이 기능은 범용 및 메모리 최적화 서버에 대해 Azure Database for MySQL이 배포된 모든 Azure 지역에서 사용할 수 있습니다.

## <a name="set-up-threat-detection"></a>위협 감지 설정
1. 에서 Azure Portal를 시작 [https://portal.azure.com](https://portal.azure.com) 합니다.
2. 보호할 Azure Database for MySQL 서버의 구성 페이지로 이동합니다. 보안 설정에서 **Advanced Threat Protection(미리 보기)** 을 선택합니다.
3. **Advanced Threat Protectio(미리 보기)** 구성 페이지에서 다음을 수행합니다.

   - 서버에서 Advanced Threat Protection을 사용하도록 설정.
   - **Advanced Threat Detection 설정** 의 **경고 전송 대상** 텍스트 상자에 비정상적인 데이터베이스 활동 발견 시 보안 경고를 받을 이메일 목록을 입력합니다.
  
   :::image type="content" source="./media/howto-database-threat-protection-portal/set-up-threat-protection.png" alt-text="위협 감지 설정":::

## <a name="explore-anomalous-database-activities"></a>비정상적인 데이터베이스 활동 살펴보기

비정상적인 데이터베이스 활동이 감지되면 메일 알림을 받습니다. 메일에는 비정상적인 활동의 특징, 데이터베이스 이름, 서버 이름, 애플리케이션 이름, 이벤트 시간을 비롯한 의심스러운 보안 이벤트에 대한 정보가 제공됩니다. 또한 전자 메일에는 가능한 원인에 대한 정보 및 데이터베이스에 대한 잠재적인 위협을 조사하고 완화시키기 위해 권장되는 조치가 제공됩니다.
 
1. 이메일에서 **최근 경고 보기** 링크를 클릭하여 Azure Portal을 시작하고, SQL 데이터베이스에서 검색된 활성 SQL 위협을 대략적으로 보여주는 Azure Security Center 경고 페이지를 표시합니다.
    
    :::image type="content" source="./media/howto-database-threat-protection-portal/anomalous-activity-report.png" alt-text="비정상적인 활동 보고서":::

    활성 위협 보기:

    :::image type="content" source="./media/howto-database-threat-protection-portal/active-threats.png" alt-text="활성 위협":::

2. 특정 경고를 클릭하여 이 위협을 조사하고 향후 위협을 수정하기 위한 추가 세부 정보 및 조치를 확인합니다.
    
    :::image type="content" source="./media/howto-database-threat-protection-portal/specific-alert.png" alt-text="특정 경고":::

## <a name="explore-threat-detection-alerts"></a>위협 검색 경고 살펴보기

SQL Database 위협 감지는 경고를 [Azure Security Center](https://azure.microsoft.com/services/security-center/)와 통합합니다. 라이브 SQL 위협 검색 타일은 데이터베이스 및 Azure Portal의 SQL ATP 페이지에서 활성 위협의 상태를 추적 합니다.

**보안 위협 경고** 를 클릭하면 Azure Security Center 경고 페이지가 열리고 데이터베이스에서 감지된 활성 SQL 위협에 대한 개요가 제공됩니다.

   :::image type="content" source="./media/howto-database-threat-protection-portal/threat-detection-alert-asc.png" alt-text="위협 감지 경고":::
   

## <a name="next-steps"></a>다음 단계

* [Azure Security Center](../security-center/security-center-introduction.md)에 대한 자세한 정보
* 가격 책정에 대한 자세한 내용은 [Azure Database for MySQL 가격 책정 페이지](https://azure.microsoft.com/pricing/details/mysql/)를 참조하세요.