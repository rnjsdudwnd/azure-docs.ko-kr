---
title: Azure virtual machines에서 32 비트 운영 체제에 대 한 지원 | Microsoft Docs
description: Azure virtual machines에서 지원 되는 운영 체제에 대 한 정보
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.author: v-miegge
ms.openlocfilehash: 81b7efdd6bca0471719c11d130be95405f4d54e1
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98210191"
---
# <a name="support-for-32-bit-operating-systems-in-azure-virtual-machines"></a>Azure 가상 머신에서 32비트 운영 체제 지원

이제 Microsoft Azure를 사용 하 여 사용자가 32 비트 Windows 운영 체제를 Azure에 제공할 수 있습니다. 특수화 된 Vhd만 지원 되며 일반화 된 이미지는 Azure에서 작동 하지 않습니다. 이러한 운영 체제 중 일부는 이미 수명 지원 계약의 끝에 도달 했으므로 Microsoft는 추가 지원을 제공 하지 않을 수 있습니다. Microsoft Azure VM (가상 컴퓨터)에서 실행 되는 Linux 기반 또는 BSD (Berkeley Software 배포판) 기반 운영 체제에 대해서도 지원이 제공 되지 않습니다.

> [!NOTE]
> Azure 플랫폼에는 VM에 1GB의 메모리만 사용할 수 있는 32 비트 운영 체제를 실행 하는 vm에 적용 되는 메모리 주소 공간 제한이 있습니다 (*특히 Win7 또는 Win10와 같은 클라이언트 sku*). vm에 대 한 나머지 메모리는 게스트 vm 내에서 예약 된 것으로 표시 됩니다. 이것은 알려진 문제 이며 현재 수정에 대해 에타가 없습니다. 64 비트 OS 버전으로 전환 하는 것이 좋습니다.
> 

## <a name="more-information"></a>추가 정보

Azure virtual machines에서 지원 되는 운영 체제에 대 한 자세한 내용은 다음 Microsoft 기술 자료 문서를 참조 하세요.

* [Microsoft Azure 가상 머신에 대한 Microsoft 서버 소프트웨어 지원](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)
* [Azure에서 Linux 및 오픈 소스 기술 지원](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure)

## <a name="references"></a>참조

* [Azure에서 Windows Server 2008/r 2의 무료 확장 된 보안 업데이트에 대 한 자세한 정보](https://www.microsoft.com/cloud-platform/windows-server-2008)
* [Azure의 Windows Server 2008 SP2 32 비트 전문 이미지 지원에 대 한 자세한 정보](/windows-server/get-started/uploading-specialized-ws08-image-to-azure)
* [Azure Site Recovery를 사용 하 여 Windows Server 2008 이미지를 Azure로 마이그레이션하기 위한 지원에 대 한 자세한 정보](../../site-recovery/migrate-tutorial-windows-server-2008.md)
* [Azure 확장 지원 운영 체제에 대 한 자세한 정보](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems)
* [Microsoft Azure에서 Windows Server 2003 실행에 대해 자세히 알아보세요.](https://support.microsoft.com/help/3206074/running-windows-server-2003-on-microsoft-azure)

## <a name="next-steps"></a>다음 단계

이 문서의 어느 시점에서 든 도움이 필요한 경우 [MSDN azure 및 Stack Overflow 포럼](https://azure.microsoft.com/support/forums/)에서 Azure 전문가에 게 문의 하세요.

또는 Azure 지원 인시던트를 파일에 제공 합니다. [Azure 지원 사이트](https://azure.microsoft.com/support/options/) 로 가서 **지원 받기** 를 선택합니다.
