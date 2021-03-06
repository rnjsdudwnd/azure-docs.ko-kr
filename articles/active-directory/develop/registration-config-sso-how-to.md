---
title: 응용 프로그램 Single Sign-On 구성
description: Azure AD로 개발 및 등록 중인 사용자 지정 애플리케이션에 대해 Single Sign-On을 구성하는 방법.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: ba1e4305785d0ff4db85f805a780719ce179ab94
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99050299"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>응용 프로그램에 대 한 Single Sign-On를 구성 하는 방법

앱에서 페더레이션된 SSO(Single Sign-On) 사용은 OpenID Connect, SAML 2.0 또는 WS-Fed에 대해 Azure AD를 통해 페더레이션할 때 자동으로 설정됩니다. Azure AD에 기존 세션이 이미 있음에도 불구하고 최종 사용자가 로그인해야 하는 경우 앱이 잘못 구성되었을 수 있습니다.

* ADAL/MSAL을 사용하는 경우 **PromptBehavior** 를 **항상** 이 아니라 **자동** 으로 설정했는지 확인하세요.

* 모바일 앱을 빌드하는 경우 조정되었거나 조정되지 않은 SSO를 사용하려면 추가 구성이 필요할 수 있습니다.

Android의 경우 [Android에서 앱 간 SSO를 사용하도록 설정](../azuread-dev/howto-v1-enable-sso-android.md)을 참조하세요.<br>

iOS의 경우 [iOS에서 앱 간 SSO를 사용하도록 설정](../azuread-dev/howto-v1-enable-sso-ios.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

[Azure AD SSO](../manage-apps/what-is-single-sign-on.md)<br>

[Android에서 앱 간 SSO를 사용하도록 설정](../azuread-dev/howto-v1-enable-sso-android.md)<br>

[iOS에서 앱 간 SSO를 사용하도록 설정](../azuread-dev/howto-v1-enable-sso-ios.md)<br>

[AzureAD와 앱 통합](./quickstart-register-app.md)<br>

[Microsoft ID 플랫폼의 권한 및 동의](./v2-permissions-and-consent.md)<br>

[AzureAD Microsoft Q&A](https://docs.microsoft.com/answers/topics/azure-active-directory.html)