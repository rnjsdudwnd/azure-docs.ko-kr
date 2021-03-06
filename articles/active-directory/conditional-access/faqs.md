---
title: 조건부 액세스 Faq Azure Active Directory | Microsoft Docs
description: Azure Active Directory의 조건부 액세스에 대 한 자주 묻는 질문에 대 한 대답을 확인 하세요.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: troubleshooting
ms.date: 10/16/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 8043e85db1ee1aadc814e98db12cab10ec17e129
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92145222"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>조건부 액세스 Faq Azure Active Directory

## <a name="which-applications-work-with-conditional-access-policies"></a>조건부 액세스 정책을 사용 하는 응용 프로그램은 무엇입니까?

조건부 액세스 정책을 사용 하는 응용 프로그램에 대 한 자세한 내용은 [Azure Active Directory에서 조건부 액세스 규칙을 사용 하는 응용 프로그램 및 브라우저](concept-conditional-access-cloud-apps.md)를 참조 하세요.

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>B2B 공동 작업 및 게스트 사용자에 게 조건부 액세스 정책이 적용 되나요?

B2B(Business-to-Business) 협업 사용자에 대한 정책이 적용됩니다. 그러나 경우에 따라 사용자가 정책 요구 사항을 충족하지 못할 수 있습니다. 예를 들어 게스트 사용자의 조직이 다단계 인증을 지원하지 않는 경우가 있습니다. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>SharePoint Online 정책도 비즈니스용 OneDrive에 적용됩니까?

예. SharePoint Online 정책은 비즈니스용 OneDrive에도 적용됩니다. 자세한 내용은 [조건부 액세스 서비스 종속성](service-dependencies.md) 문서를 참조 하 고 [Office 365 앱](concept-conditional-access-cloud-apps.md#office-365) 에 대 한 정책을 대상으로 지정 하는 것이 좋습니다.

## <a name="why-cant-i-set-a-policy-directly-on-client-apps-like-word-or-outlook"></a>Word 또는 Outlook과 같은 클라이언트 앱에서 정책을 직접 설정할 수 없는 이유는 무엇입니까?

조건부 액세스 정책은 서비스에 액세스 하기 위한 요구 사항을 설정 합니다. 해당 서비스에 인증하는 경우에 적용됩니다. 클라이언트 애플리케이션에서 직접 정책이 설정되는 것이 아니라 클라이언트가 서비스를 호출할 때 적용됩니다. 예를 들어 SharePoint에서 설정된 정책은 SharePoint를 호출하는 클라이언트에 적용됩니다. Exchange에서 설정된 정책은 Outlook에 적용됩니다. 자세한 내용은 [조건부 액세스 서비스 종속성](service-dependencies.md) 문서를 참조 하 고 [Office 365 앱](concept-conditional-access-cloud-apps.md#office-365) 에 대 한 정책을 대상으로 지정 하는 것이 좋습니다.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>조건부 액세스 정책이 서비스 계정에 적용 되나요?

조건부 액세스 정책은 모든 사용자 계정에 적용 됩니다. 여기에는 서비스 계정으로 사용되는 사용자 계정이 포함됩니다. 무인 모드로 실행 되는 서비스 계정은 조건부 액세스 정책의 요구 사항을 충족할 수 없는 경우가 많습니다. 예를 들어 다단계 인증이 필요할 수 있습니다. 서비스 계정은 [사용자 또는 그룹 제외](concept-conditional-access-users-groups.md#exclude-users)를 사용 하 여 정책에서 제외할 수 있습니다. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>지원되지 않는 디바이스 플랫폼에 대한 기본 제외 정책은 무엇인가요?

현재 iOS 및 Android 장치의 사용자에 대해 선택적으로 조건부 액세스 정책이 적용 됩니다. 다른 장치 플랫폼의 응용 프로그램은 기본적으로 iOS 및 Android 장치에 대 한 조건부 액세스 정책의 영향을 받지 않습니다. 테넌트 관리자는 지원되지 않는 플랫폼의 사용자에 대해 액세스를 차단하도록 전역 정책을 재정의할 수 있습니다.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Microsoft 팀에서 조건부 액세스 정책을 사용 하는 방법

Microsoft Teams는 모임, 일정, 파일 공유 등의 핵심 생산성 시나리오를 위해 Exchange Online 및 SharePoint Online을 많이 사용합니다. 이러한 클라우드 앱에 대해 설정 된 조건부 액세스 정책은 사용자가 Microsoft 팀에 직접 로그인 할 때 Microsoft 팀에 적용 됩니다.

Microsoft 팀도 조건부 액세스 정책에서 클라우드 앱으로 개별적으로 지원 됩니다. 클라우드 앱에 대해 설정 된 조건부 액세스 정책은 사용자가 로그인 할 때 Microsoft 팀에 적용 됩니다. 그러나 Exchange Online 및 SharePoint Online 등의 다른 앱에 대한 정확한 정책이 없으면 사용자가 해당 리소스에 계속하여 직접 액세스할 수 있습니다.

Windows 및 Mac용 Microsoft Teams 데스크톱 클라이언트는 최신 인증을 지원합니다. 최신 인증은 ADAL(Azure Active Directory Authentication Library) 기반의 로그인을 플랫폼 전체의 Microsoft Office 클라이언트 애플리케이션에 제공합니다.

자세한 내용은 [조건부 액세스 서비스 종속성](service-dependencies.md) 문서를 참조 하 고 [Office 365 앱](concept-conditional-access-cloud-apps.md#office-365) 에 대 한 정책을 대상으로 지정 하는 것이 좋습니다.

## <a name="why-are-some-tabs-not-working-in-microsoft-teams-after-enabling-conditional-access-policies"></a>조건부 액세스 정책을 사용 하도록 설정한 후 일부 탭이 Microsoft 팀에서 작동 하지 않는 이유는 무엇 인가요?

Microsoft 팀의 테 넌 트에서 일부 조건부 액세스 정책을 사용 하도록 설정 하면 특정 탭이 더 이상 데스크톱 클라이언트에서 제대로 작동 하지 않을 수 있습니다. 그러나 Microsoft 팀 웹 클라이언트를 사용 하는 경우 영향을 받는 탭이 작동 합니다. 영향을 받는 탭에는 Power BI, 폼, VSTS, PowerApps 및 SharePoint 목록이 포함 될 수 있습니다.

영향을 받는 탭을 확인 하려면 Windows 10 계정 확장이 설치 된 Edge, Internet Explorer 또는 Chrome에서 팀 웹 클라이언트를 사용 해야 합니다. 일부 탭은 조건부 액세스를 사용 하도록 설정 된 경우 Microsoft 팀 데스크톱 클라이언트에서 작동 하지 않는 웹 인증에 종속 됩니다. Microsoft는 파트너와 협력 하 여 이러한 시나리오를 가능 하 게 합니다. 지금까지 Planner, OneNote 및 스트림과 관련 된 시나리오를 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- 사용자 환경에 대 한 조건부 액세스 정책을 구성 하려면 [조건부 액세스 배포 계획](plan-conditional-access.md)문서를 참조 하세요. 
