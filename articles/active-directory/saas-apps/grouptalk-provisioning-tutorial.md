---
title: '자습서: Azure Active Directory로 자동 사용자 프로 비전을 위한 GroupTalk 구성 Microsoft Docs'
description: 사용자 계정을 Azure AD에서 GroupTalk로 자동으로 프로 비전 및 프로 비전 해제 하는 방법을 알아봅니다.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: e537d393-2724-450f-9f5b-4611cdc9237c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/18/2020
ms.author: Zhchia
ms.openlocfilehash: 0af41127577c172cdab74ae908f0645733d49a42
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735761"
---
# <a name="tutorial-configure-grouptalk-for-automatic-user-provisioning"></a>자습서: 자동 사용자 프로 비전을 위한 GroupTalk 구성

이 자습서에서는 자동 사용자 프로 비전을 구성 하기 위해 GroupTalk와 Azure Active Directory (Azure AD)에서 수행 해야 하는 단계를 설명 합니다. 구성 된 경우 azure AD는 Azure AD 프로 비전 서비스를 사용 하 여 사용자 및 그룹을 [Grouptalk](https://www.grouptalk.com/) 수 있도록 자동으로 프로 비전 하 고 프로 비전 해제 합니다. 이 서비스의 기능, 작동 방법 및 질문과 대답에 대한 중요한 내용은 [Azure Active Directory를 사용하여 SaaS 애플리케이션의 사용자를 자동으로 프로비저닝 및 프로비저닝 해제](../app-provisioning/user-provisioning.md)를 참조하세요. 


## <a name="capabilities-supported"></a>지원되는 기능
> [!div class="checklist"]
> * GroupTalk에서 사용자 만들기
> * 더 이상 액세스 하지 않아도 되는 GroupTalk에서 사용자 제거
> * Azure AD와 GroupTalk 간에 사용자 특성을 동기화 상태로 유지
> * GroupTalk에서 그룹 및 그룹 멤버 자격 프로 비전

## <a name="prerequisites"></a>사전 요구 사항

이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 필수 구성 요소가 있다고 가정합니다.

* [Azure AD 테넌트](../develop/quickstart-create-new-tenant.md) 
* 프로비저닝을 구성할 [권한](../roles/permissions-reference.md)이 있는 Azure AD의 사용자 계정(예: 애플리케이션 관리자, 클라우드 애플리케이션 관리자, 애플리케이션 소유자 또는 전역 관리자). 
* GroupTalk의 사용자 계정은 관리자 권한과 통신 합니다.

## <a name="step-1-plan-your-provisioning-deployment"></a>1단계. 프로비저닝 배포 계획
1. [프로비저닝 서비스의 작동 방식](../app-provisioning/user-provisioning.md)에 대해 알아봅니다.
2. [프로비저닝 범위](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)에 있는 사용자를 결정합니다.
3. [AZURE AD와 GroupTalk 간에 매핑할](../app-provisioning/customize-application-attributes.md)데이터를 결정 합니다. 

## <a name="step-2-configure-grouptalk-to-support-provisioning-with-azure-ad"></a>2단계. Azure AD로 프로 비전을 지원 하도록 GroupTalk 구성

1. support@grouptalk.comAZURE AD와 통합 하려는 **테 넌 트 이름** 및 **ID** 를 사용 하 여에서 grouptalk 지원에 연결 합니다.
2. Azure AD 통합에 대 한 필수 설정이 준비 되었다는 알림이 표시 되 면 GroupTalk Admin에 로그인 하 여 조직 뷰로 이동 합니다. 
3. Azure AD 통합 구성 항목이 표시 됩니다. 이를 클릭 하 여 **테 넌 트 이름** 및 **ID**  를 확인 하 여 **JWT (비밀 토큰)** 를 가져옵니다. 
4. GroupTalk 테 넌 트 URL은 `https://api.grouptalk.com/api/scim/` 입니다. 이전 단계에서 검색 된 **테 넌 트 URL** 및 **암호 토큰** 은 Azure Portal에서 Grouptalk 응용 프로그램의 프로 비전 탭에 입력 됩니다. 

## <a name="step-3-add-grouptalk-from-the-azure-ad-application-gallery"></a>3단계. Azure AD 응용 프로그램 갤러리에서 GroupTalk 추가

Azure AD 응용 프로그램 갤러리에서 **Grouptalk** 를 추가 하 여 grouptalk에 대 한 프로 비전 관리를 시작 합니다.

1. Grouptalk **에 등록** 단추를 클릭 합니다. 그러면 grouptalk 관리 응용 프로그램으로 라우팅됩니다.
2. GroupTalk에 이미 로그인 한 경우 로그 아웃 하 여 로그인 화면으로 이동 합니다. Azure AD 탭을 선택 하 고 **로그인** 단추를 클릭 합니다.

    ![GroupTalk](media/grouptalk-provisioning-tutorial/login.png)

3. AD 관리 계정으로 로그인 하 고 GroupTalk 응용 프로그램의 액세스 권한을 수락 합니다. 이 작업이 완료 된 후 사용자가 없다는 것을 나타내는 오류 메시지가 표시 됩니다. 사용자가 GroupTalk에 아직 프로 비전 되지 않았지만 이제 그룹 대화를 테 넌 트에 추가 했으므로이는 예상 됩니다.
4. Azure Portal로 돌아가서 이제 엔터프라이즈 응용 프로그램에 **grouptalk** is가 추가 되었는지 확인 합니다.

[여기](../manage-apps/add-application-portal.md)를 클릭하여 갤러리에서 애플리케이션을 추가하는 방법에 대해 자세히 알아봅니다. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4단계. 프로비저닝 범위에 있는 사용자 정의 

Azure AD 프로비저닝 서비스를 사용하면 애플리케이션에 대한 할당 또는 사용자/그룹의 특성을 기반으로 프로비저닝되는 사용자의 범위를 지정할 수 있습니다. 할당을 기준으로 앱에 프로비저닝할 사용자의 범위를 선택하려면 다음 [단계](../manage-apps/assign-user-or-group-access-portal.md)를 사용하여 애플리케이션에 사용자 및 그룹을 할당할 수 있습니다. 사용자 또는 그룹의 특성만을 기준으로 프로비저닝할 사용자의 범위를 선택하려면 [여기](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) 설명된 대로 범위 지정 필터를 사용할 수 있습니다. 

* 사용자 및 그룹을 GroupTalk에 할당 하는 경우 **기본 액세스** 이외의 다른 역할을 선택 해야 합니다. 기본 액세스 역할이 있는 사용자는 프로비저닝에서 제외되고 프로비저닝 로그에 실질적으로 권한을 부여받지 않은 것으로 표시됩니다. 애플리케이션에서 사용할 수 있는 유일한 역할이 기본 액세스 역할인 경우에는 [애플리케이션 매니페스트를 업데이트](../develop/howto-add-app-roles-in-azure-ad-apps.md)하여 역할을 더 추가할 수 있습니다. 

* 소규모로 시작합니다. 모든 사용자에게 배포하기 전에 소수의 사용자 및 그룹 집합으로 테스트합니다. 할당된 사용자 및 그룹으로 프로비저닝 범위가 설정된 경우 앱에 하나 또는 두 개의 사용자 또는 그룹을 할당하여 범위를 제어할 수 있습니다. 모든 사용자 및 그룹으로 범위가 설정된 경우 [특성 기반 범위 지정 필터](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)를 지정할 수 있습니다. 


## <a name="step-5-configure-automatic-user-provisioning-to-grouptalk"></a>5단계. GroupTalk에 자동 사용자 프로 비전 구성 

이 섹션에서는 Azure AD의 사용자 및/또는 그룹 할당에 따라 TestApp에서 사용자 및/또는 그룹을 만들고, 업데이트하고, 사용 해제하도록 Azure AD 프로비저닝 서비스를 구성하는 단계를 안내합니다.

### <a name="to-configure-automatic-user-provisioning-for-grouptalk-in-azure-ad"></a>Azure AD에서 GroupTalk에 대 한 자동 사용자 프로 비전을 구성 하려면:

1. [Azure Portal](https://portal.azure.com)에 로그인합니다. **엔터프라이즈 애플리케이션**, **모든 애플리케이션** 을 차례로 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 응용 프로그램 목록에서 **Grouptalk** 를 선택 합니다.

    ![응용 프로그램 목록의 GroupTalk 링크](common/all-applications.png)

3. **프로비전** 탭을 선택합니다.

    ![프로비저닝 탭](common/provisioning.png)

4. **프로비전 모드** 를 **자동** 으로 설정합니다.

    ![프로비저닝 탭 자동](common/provisioning-automatic.png)

5. **관리자 자격 증명** 섹션 아래에서 2 단계에서 이전에 검색 한 Grouptalk 테 넌 트 URL 및 비밀 토큰을 입력 합니다. **연결 테스트** 를 클릭 하 여 Azure AD가 grouptalk에 연결할 수 있는지 확인 합니다. 연결에 실패 하면 GroupTalk 계정에 관리자 권한이 있는지 확인 하 고 다시 시도 합니다. 2 단계에 설명 된 대로 항상 새 비밀 토큰을 얻을 수 있습니다.

    ![토큰](common/provisioning-testconnection-tenanturltoken.png)

6. **알림 이메일** 필드에 프로비저닝 오류 알림을 받을 개인 또는 그룹의 메일 주소를 입력하고, **오류가 발생할 경우 메일 알림 보내기** 확인란을 선택합니다.

    ![알림 이메일](common/provisioning-notification-email.png)

7. **저장** 을 선택합니다.

8. **매핑** 섹션에서 **Azure Active Directory 사용자를 Grouptalk에 동기화를** 선택 합니다.

9. **특성 매핑** 섹션에서 Azure AD에서 grouptalk로 동기화 되는 사용자 특성을 검토 합니다. **일치** 속성으로 선택한 특성은 업데이트 작업 시 grouptalk의 사용자 계정을 일치 시키는 데 사용 됩니다. 일치 하는 [대상 특성](../app-provisioning/customize-application-attributes.md)을 변경 하도록 선택 하는 경우 GROUPTALK API가 해당 특성에 따라 사용자 필터링을 지원 하는지 확인 해야 합니다. **저장** 단추를 선택하여 변경 내용을 커밋합니다.

   |attribute|Type|필터링에 지원됨|
   |---|---|---|
   |userName|String|&check;|
   |phoneNumbers[type eq "mobile"].value|String|&check;|
   |emails[type eq "work"].value|String|&check;|
   |활성|부울|
   |name.givenName|String|
   |name.familyName|String|
   |externalId|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|String|
   |urn: ietf: params: scim: 스키마: 확장: grouptalk: 2.0: 사용자: label1|String|
   |urn: ietf: params: scim: 스키마: 확장: grouptalk: 2.0: 사용자: label2|String|
   |urn: ietf: params: scim: 스키마: 확장: grouptalk: 2.0: 사용자: 레이블 3|String|
   |urn: ietf: params: scim: 스키마: 확장: grouptalk: 2.0: 사용자: label4|String|
   |urn: ietf: params: scim: 스키마: 확장: grouptalk: 2.0: 사용자: label5|String|


10. **매핑** 섹션 아래에서 **grouptalk에 Azure Active Directory 그룹 동기화를** 선택 합니다.

11. **특성 매핑** 섹션에서 Azure AD에서 grouptalk로 동기화 되는 그룹 특성을 검토 합니다. **일치** 속성으로 선택한 특성은 업데이트 작업 시 grouptalk의 그룹을 일치 시키는 데 사용 됩니다. **저장** 단추를 선택하여 변경 내용을 커밋합니다.

      |attribute|Type|필터링에 지원됨|
      |---|---|---|
      |displayName|String|&check;|
      |members|참조|
      |externalId|String|      
      |urn: ietf: params: scim: 스키마: 확장: grouptalk: 2.0: 그룹: 설명|String|

12. 범위 지정 필터를 구성하려면 [범위 지정 필터 자습서](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)에서 제공하는 다음 지침을 참조합니다.

13. GroupTalk에 Azure AD 프로 비전 서비스를 사용 하도록 **설정 하려면 설정** 섹션에서 **프로 비전 상태** 를 **켜기** 로 변경 합니다.

    ![프로비전 상태 켜기로 전환](common/provisioning-toggle-on.png)

14. **설정** 섹션의 **범위** 에서 원하는 값을 선택 하 여 grouptalk에 프로 비전 하려는 사용자 및/또는 그룹을 정의 합니다.

    ![프로비전 범위](common/provisioning-scope.png)

15. 프로비전할 준비가 되면 **저장** 을 클릭합니다.

    ![프로비전 구성 저장](common/provisioning-configuration-save.png)

이 작업은 **설정** 의 **범위** 섹션에 정의된 모든 사용자 및/또는 그룹의 초기 동기화 주기를 시작합니다. 초기 주기는 Azure AD 프로비저닝 서비스가 실행되는 동안 약 40분마다 발생하는 후속 주기보다 더 많은 시간이 걸립니다. 

## <a name="step-6-monitor-your-deployment"></a>6단계. 배포 모니터링
프로비저닝을 구성한 후에는 다음 리소스를 사용하여 배포를 모니터링합니다.

1. [프로비저닝 로그](../reports-monitoring/concept-provisioning-logs.md)를 사용하여 어떤 사용자가 성공적으로 프로비저닝되었는지 확인합니다.
2. [진행률 표시줄](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)을 통해 프로비저닝 주기 상태와 완료 정도를 확인합니다.
3. 프로비저닝 구성이 비정상 상태로 보이면 애플리케이션이 격리됩니다. 격리 상태에 대한 자세한 내용은 [여기](../app-provisioning/application-provisioning-quarantine-status.md)를 참조하세요.
4. Grouptalk 관리자 내에서 사용자 지정 보고서로 설정 된 추가 프로 비전 로그에 대해 GroupTalk 지원에 문의할 수 있습니다. 사용자 및 그룹이 제대로 프로 비전 되지 않는 이유는 추가 힌트를 제공할 수 있습니다.

## <a name="additional-resources"></a>추가 리소스

* [엔터프라이즈 앱에 대한 사용자 계정 프로비전 관리](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>다음 단계

* [프로비저닝 작업에 대한 로그를 검토하고 보고서를 받아보는 방법을 알아봅니다](../app-provisioning/check-status-user-account-provisioning.md).