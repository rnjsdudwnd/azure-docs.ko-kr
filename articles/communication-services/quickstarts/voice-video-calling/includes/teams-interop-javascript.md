---
title: 빠른 시작 - Teams 모임 참가
author: chpalm
ms.author: mikben
ms.date: 10/10/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: dbba87be839d7f172d42827698a8e485c2edddd8
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98256570"
---
## <a name="prerequisites"></a>사전 요구 사항

- 작동하는 [Communication Services 호출 앱](../getting-started-with-calling.md)
- [Teams 배포](/deployoffice/teams-install)


## <a name="add-the-teams-ui-controls"></a>Teams UI 컨트롤 추가

index.html의 코드를 다음 코드 조각으로 바꿉니다.
텍스트 상자는 Teams 모임 컨텍스트를 입력하는 데 사용되며, 단추는 지정된 모임에 참가하는 데 사용됩니다.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Communication Client - Calling Sample</title>
</head>
<body>
    <h4>Azure Communication Services</h4>
    <h1>Teams meeting join quickstart</h1>
    <input id="teams-link-input" type="text" placeholder="Teams meeting link"
        style="margin-bottom:1em; width: 300px;" />
        <p>Call state <span style="font-weight: bold" id="call-state">-</span></p>
    <div>
        <button id="join-meeting-button" type="button" disabled="false">
            Join Teams Meeting
        </button>
        <button id="hang-up-button" type="button" disabled="true">
            Hang Up
        </button>
    </div>
    <script src="./bundle.js"></script>
</body>

</html>
```

## <a name="enable-the-teams-ui-controls"></a>Teams UI 컨트롤 사용

client.js 파일의 콘텐츠를 다음 코드 조각으로 바꿉니다.

```javascript
import { CallClient } from "@azure/communication-calling";
import { AzureCommunicationUserCredential } from '@azure/communication-common';

let call;
let callAgent;
const meetingLinkInput = document.getElementById('teams-link-input');
const hangUpButton = document.getElementById('hang-up-button');
const teamsMeetingJoinButton = document.getElementById('join-meeting-button');
const callStateElement = document.getElementById('call-state');

async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, {displayName: 'ACS user'});
    teamsMeetingJoinButton.disabled = false;
}
init();

hangUpButton.addEventListener("click", async () => {
    // end the current call
    await call.hangUp();
  
    // toggle button states
    hangUpButton.disabled = true;
    teamsMeetingJoinButton.disabled = false;
    callStateElement.innerText = '-';
  });

teamsMeetingJoinButton.addEventListener("click", () => {    
    // join with meeting link
    call = callAgent.join({meetingLink: meetingLinkInput.value}, {});
    
    call.on('callStateChanged', () => {
        callStateElement.innerText = call.state;
    })
    // toggle button states
    hangUpButton.disabled = false;
    teamsMeetingJoinButton.disabled = true;
});
```

## <a name="get-the-teams-meeting-link"></a>Teams 미팅 링크 가져오기

Teams 미팅 링크는 Graph API를 사용하여 검색할 수 있습니다. 자세한 내용은 [Graph 설명서](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta)에서 설명하고 있습니다.
Communication Services Calling SDK는 전체 Teams 미팅 링크를 수락합니다. 이 링크는 [`joinWebUrl` 속성](/graph/api/resources/onlinemeeting?view=graph-rest-beta)에서 액세스할 수 있는 `onlineMeeting` 리소스의 일부로 반환됩니다. Teams 미팅 초대 자체의 **미팅 조인** URL에서 필요한 미팅 정보를 가져올 수도 있습니다.

## <a name="run-the-code"></a>코드 실행

webpack 사용자는 `webpack-dev-server`를 사용하여 앱을 빌드하고 실행할 수 있습니다. 다음 명령을 실행하여 로컬 웹 서버에서 애플리케이션 호스트를 번들로 묶습니다.

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

브라우저를 열고 http://localhost:8080/로 이동합니다. 다음이 표시되어야 합니다.

:::image type="content" source="../media/javascript/acs-join-teams-meeting-quickstart.PNG" alt-text="완성된 JavaScript 애플리케이션의 스크린샷":::

Teams 컨텍스트를 텍스트 상자에 삽입하고, *Teams 모임 참가* 를 눌러 Communication Services 애플리케이션 내에서 Teams 모임에 참가합니다.
