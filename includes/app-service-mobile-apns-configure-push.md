---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 852a3b00e8513f9ce68c5aae54c974505d0c9af6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67182701"
---
1. Mac에서 **Keychain Access**를 시작합니다. 왼쪽 탐색 모음의 **범주** 아래에서 **내 인증서**를 엽니다. 이전 섹션에서 다운로드한 SSL 인증서를 찾은 다음, 해당 콘텐츠를 공개합니다. 인증서만 선택합니다.(프라이빗 키를 선택하지 않습니다.) 그런 다음, [내보내기](https://support.apple.com/kb/PH20122?locale=en_US) 합니다.
2. [Azure Portal](https://portal.azure.com/)에서 **모두 찾아보기** > **App Services**를 선택합니다. 그런 다음, Mobile Apps 백엔드를 선택합니다. 
3. **설정**에서 **App Service Push**를 선택합니다. 그런 다음, 알림 허브 이름을 선택합니다. 
4. Apple **푸시 알림 서비스** > **업로드 인증서로**이동합니다. .p12 파일을 업로드하고 올바른 **모드를** 선택합니다(이전의 클라이언트 SSL 인증서가 프로덕션 또는 샌드박스인지 여부에 따라 다름). 변경 내용을 저장합니다.

이제 iOS의 푸시 알림과 작동하도록 서비스가 구성되었습니다.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
