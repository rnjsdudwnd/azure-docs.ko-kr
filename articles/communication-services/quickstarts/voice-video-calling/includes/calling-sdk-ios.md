---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: a8cfdc76694d52acee70cde0e3f1697cd8129d06
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691971"
---
## <a name="prerequisites"></a>사전 요구 사항

- 활성 구독이 있는 Azure 계정. [체험 계정을 만듭니다](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- 배포된 Communication Services 리소스. [Communication Services 리소스를 만듭니다](../../create-communication-resource.md).
- 호출 클라이언트를 사용하도록 설정하는 `User Access Token`입니다. [`User Access Token`를 가져오는 방법](../../access-tokens.md)에 대한 자세한 정보
- 선택 사항: [응용 프로그램에 대 한 호출 추가를 시작](../getting-started-with-calling.md) 하기 위한 빠른 시작을 완료 합니다.

## <a name="setting-up"></a>설치

### <a name="creating-the-xcode-project"></a>Xcode 프로젝트 만들기

Xcode에서 새 iOS 프로젝트를 만들고 **단일 보기 앱** 템플릿을 선택합니다. 이 빠른 시작에서는 [SwiftUI 프레임 워크](https://developer.apple.com/xcode/swiftui/)를 사용 하므로 **언어** 를 **Swift** 로 설정 하 고 **사용자 인터페이스** 를 **SwiftUI** 로 설정 해야 합니다. 이 빠른 시작 중에는 단위 테스트 또는 UI 테스트를 만들지 않습니다. **단위 테스트 포함** 을 선택 취소 하 고 **UI 테스트 포함** 의 선택을 취소할 수 있습니다.

:::image type="content" source="../media/ios/xcode-new-ios-project.png" alt-text="Xcode 내에서 새 프로젝트 만들기 창을 보여주는 스크린샷":::

### <a name="install-the-package-and-dependencies-with-cocoapods"></a>CocoaPods를 사용하여 패키지 및 종속성 설치

1. 다음과 같이 애플리케이션에 대한 Podfile을 만듭니다.

   ```
   platform :ios, '13.0'
   use_frameworks!
   target 'AzureCommunicationCallingSample' do
     pod 'AzureCommunicationCalling', '~> 1.0.0-beta.5'
     pod 'AzureCommunication', '~> 1.0.0-beta.5'
     pod 'AzureCore', '~> 1.0.0-beta.5'
   end
   ```

2. `pod install`을 실행합니다.
3. XCode를 `.xcworkspace` 사용 하 여를 엽니다.

### <a name="request-access-to-the-microphone"></a>마이크에 대한 액세스 요청

디바이스의 마이크에 액세스하려면 앱의 정보 속성 목록을 `NSMicrophoneUsageDescription`으로 업데이트해야 합니다. 연결된 값을 시스템이 사용자의 액세스 요청을 요청하는 데 사용하는 대화 상자에 포함될 `string`으로 설정합니다.

프로젝트 트리의 `Info.plist` 항목을 마우스 오른쪽 단추로 클릭하고 **다음 형식으로 열기** > **소스 코드** 를 선택합니다. 최상위 `<dict>` 섹션에 다음 줄을 추가한 다음, 파일을 저장합니다.

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Need microphone access for VOIP calling.</string>
```

### <a name="set-up-the-app-framework"></a>앱 프레임워크 설정

프로젝트의 **ContentView.swift** 파일을 열고 파일 맨 위에 `import` 선언을 추가하여 `AzureCommunicationCalling library`를 가져옵니다. 또한 가져오기 `AVFoundation` , 코드에서 오디오 권한 요청에 필요 합니다.

```swift
import AzureCommunicationCalling
import AVFoundation
```

## <a name="object-model"></a>개체 모델

다음 클래스와 인터페이스는 iOS 용 클라이언트 라이브러리를 호출 하는 Azure Communication Services의 주요 기능 중 일부를 처리 합니다.


| 속성                                  | 설명                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ACSCallClient | ACSCallClient는 호출 하는 클라이언트 라이브러리에 대 한 주 진입점입니다.|
| ACSCallAgent | ACSCallAgent는 호출을 시작 하 고 관리 하는 데 사용 됩니다. |
| CommunicationUserCredential | CommunicationUserCredential은 CallAgent를 인스턴스화하기 위한 토큰 자격 증명으로 사용됩니다.| 
| CommunicationIndentifier | CommunicationIndentifier는 사용자의 ID를 나타내는 데 사용되며 다음 중 하나일 수 있습니다. CommunicationUser/PhoneNumber/CallingApplication. |

> [!NOTE]
> 이벤트 대리자를 구현할 때 응용 프로그램은 이벤트 구독을 필요로 하는 개체에 대 한 강력한 참조를 보유 해야 합니다. 예를 들어 메서드를 `ACSRemoteParticipant` 호출 하 여 개체가 반환 되 `call.addParticipant` 고 응용 프로그램이에서 수신 대기 하도록 대리자를 설정 하는 경우 `ACSRemoteParticipantDelegate` 응용 프로그램은 개체에 대 한 강력한 참조를 보유 해야 합니다 `ACSRemoteParticipant` . 그렇지 않으면이 개체가 수집 될 경우 호출 SDK에서 개체를 호출 하려고 할 때 대리자는 심각한 예외를 throw 합니다.

## <a name="initialize-the-acscallagent"></a>ACSCallAgent를 초기화 합니다.

`ACSCallAgent`에서 인스턴스를 만들려면 `ACSCallClient` `callClient.createCallAgent` 초기화 된 후 비동기적으로 개체를 반환 하는 메서드를 사용 해야 합니다. `ACSCallAgent`

호출 클라이언트를 만들려면 개체를 전달 해야 `CommunicationUserCredential` 합니다.

```swift

import AzureCommunication

let tokenString = "token_string"
var userCredential: CommunicationUserCredential?
do {
    userCredential = try CommunicationUserCredential(
        initialToken: tokenString, refreshProactively: true,
        tokenRefresher: self.fetchTokenSync
    )
} catch {
    print("Failed to create CommunicationCredential object")
}

// tokenProvider needs to be implemented by contoso which fetches new token
public func fetchTokenSync(then onCompletion: TokenRefreshOnCompletion) {
    let newToken = self.tokenProvider!.fetchNewToken()
    onCompletion(newToken, nil)
}
```

위에서 만든 CommunicationUserCredential 개체를 ACSCallClient에 전달 하 고 표시 이름을 설정 합니다.

```swift

callClient = CallClient()
let callAgentOptions:CallAgentOptions = CallAgentOptions()
options.displayName = "ACS iOS User"

callClient?.createCallAgent(userCredential: userCredential!,
    options: callAgentOptions,
    completionHandler: { (callAgent, error) in
        if error == nil {
            print("Create agent succeeded")
            self.callAgent = callAgent
        } else {
            print("Create agent failed")
        }
})

```

## <a name="place-an-outgoing-call"></a>나가는 호출을 수행 합니다.

호출을 만들고 시작 하려면에서 Api 중 하나를 호출 하 `ACSCallAgent` 고 통신 서비스 관리 클라이언트 라이브러리를 사용 하 여 프로 비전 한 사용자의 통신 서비스 id를 제공 해야 합니다.

호출 생성 및 시작은 동기식입니다. 호출에서 모든 이벤트를 구독할 수 있는 call 인스턴스를 받게 됩니다.

### <a name="place-a-11-call-to-a-user-or-a-1n-call-with-users-and-pstn"></a>사용자에 게 1:1 호출 또는 사용자 및 PSTN을 사용 하 여 1: n 호출

```swift

let callees = [CommunicationUser(identifier: 'acsUserId')]
let oneToOneCall = self.callAgent.call(participants: callees, options: StartCallOptions())

```

### <a name="place-a-1n-call-with-users-and-pstn"></a>사용자 및 PSTN을 사용 하 여 1: n 호출
PSTN에 대 한 호출을 수행 하려면 통신 서비스를 사용 하 여 얻은 전화 번호를 지정 해야 합니다.
```swift

let pstnCallee = PhoneNumber('+1999999999')
let callee = CommunicationUser(identifier: 'acsUserId')
let groupCall = self.callAgent.call(participants: [pstnCallee, callee], options: StartCallOptions())

```

### <a name="place-a-11-call-with-with-video"></a>비디오를 사용 하 여 1:1 호출
장치 관리자 인스턴스를 가져오려면 [여기](#device-management) 를 참조 하세요.

```swift

let camera = self.deviceManager!.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: camera)
let videoOptions = VideoOptions(localVideoStream: localVideoStream)

let startCallOptions = StartCallOptions()
startCallOptions?.videoOptions = videoOptions

let callee = CommunicationUser(identifier: 'acsUserId')
let call = self.callAgent?.call(participants: [callee], options: startCallOptions)

```

### <a name="join-a-group-call"></a>그룹 통화 참가
호출을 조인 하려면 *Callagent* 에서 api 중 하나를 호출 해야 합니다.

```swift

let groupCallContext = GroupCallContext()
groupCallContext?.groupId = UUID(uuidString: "uuid_string")!
let call = self.callAgent?.join(with: groupCallContext, joinCallOptions: JoinCallOptions())

```

### <a name="accept-an-incoming-call"></a>들어오는 호출 수락
호출을 수락 하려면 호출 개체에서 ' accept ' 메서드를 호출 합니다.
CallAgent로 대리자 설정 
```swift
final class CallHandler: NSObject, CallAgentDelegate
{
    public var incomingCall: Call?
 
    public func onCallsUpdated(_ callAgent: CallAgent!, args: CallsUpdatedEventArgs!) {
        if let incomingCall = args.addedCalls?.first(where: { $0.isIncoming }) {
            self.incomingCall = incomingCall
        }
    }
}

let firstCamera: VideoDeviceInfo? = self.deviceManager?.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: firstCamera)
let acceptCallOptions = AcceptCallOptions()
acceptCallOptions!.videoOptions = VideoOptions(localVideoStream:localVideoStream!)
if let incomingCall = CallHandler().incomingCall {
   incomingCall.accept(options: acceptCallOptions,
                          completionHandler: { (error) in
                           if error == nil {
                               print("Incoming call accepted")
                           } else {
                               print("Failed to accept incoming call")
                           }
                       })
} else {
   print("No incoming call found to accept")
}
```

## <a name="push-notification"></a>푸시 알림

모바일 푸시 알림은 모바일 장치에 표시 되는 팝업 알림입니다. 을 호출 하는 경우 VoIP (Voice over Internet Protocol) 푸시 알림에 집중 하 게 됩니다. 푸시 알림을 등록 하 고, 푸시 알림을 처리 하 고, 푸시 알림을 등록 취소 하는 기능을 제공 합니다.

### <a name="prerequisite"></a>필수 조건

- 1 단계: Xcode > 서명 & 기능-"푸시 알림" 기능을 > 추가 >
- 2 단계: Xcode > 서명 & 기능-> 기능 추가-> "백그라운드 모드"
- 3 단계: "백그라운드 모드"-> "Voip (Voice over IP)" 및 "원격 알림" 선택

:::image type="content" source="../media/ios/xcode-push-notification.png" alt-text="Xcode의 기능을 추가 하는 방법을 보여 주는 스크린샷" lightbox="../media/ios/xcode-push-notification.png":::

#### <a name="register-for-push-notifications"></a>푸시 알림 등록

푸시 알림을 등록 하려면 장치 등록 토큰이 포함 된 *Callagent* 인스턴스에 대해 registerPushNotification ()를 호출 합니다.

초기화를 성공한 후 푸시 알림에 대 한 등록을 호출 해야 합니다. `callAgent`개체가 제거 되 면 `logout` 푸시 알림을 자동으로 등록 취소 하는가 호출 됩니다.


```swift

let deviceToken: Data = pushRegistry?.pushToken(for: PKPushType.voIP)
callAgent.registerPushNotifications(deviceToken: deviceToken,
                completionHandler: { (error) in
    if(error == nil) {
        print("Successfully registered to push notification.")
    } else {
        print("Failed to register push notification.")
    }
})

```

#### <a name="push-notification-handling"></a>푸시 알림 처리
들어오는 호출 푸시 알림을 받으려면 사전 페이로드가 있는 *Callagent* 인스턴스에 대해 *handlepushnotification ()* 를 호출 합니다.

```swift

let dictionaryPayload = pushPayload?.dictionaryPayload
callAgent.handlePushNotification(payload: dictionaryPayload, completionHandler: { (error) in
    if (error != nil) {
        print("Handling of push notification failed")
    } else {
        print("Handling of push notification was successful")
    }
})

```
#### <a name="unregister-push-notification"></a>푸시 알림 등록 취소

응용 프로그램은 언제 든 지 푸시 알림을 등록 취소할 수 있습니다. `unRegisterPushNotification` *Callagent* 에서 메서드를 호출 하기만 하면 됩니다.
> [!NOTE]
> 응용 프로그램은 로그 아웃 시 푸시 알림에서 자동으로 등록 취소 되지 않습니다.

```swift

callAgent.unRegisterPushNotifications(completionHandler: { (error) in
    if (error != nil) {
        print("Unregister of push notification failed, please try again")
    } else {
        print("Unregister of push notification was successful")
    }
})

```

## <a name="mid-call-operations"></a>중간 호출 작업

호출 중에 비디오 및 오디오와 관련 된 설정을 관리 하는 다양 한 작업을 수행할 수 있습니다.

### <a name="mute-and-unmute"></a>음소거 및 음소거 해제

로컬 끝점을 음소거 하거나 음소거를 해제 하려면 `mute` 및 비동기 api를 사용할 수 있습니다 `unmute` .

```swift
call!.mute(completionHandler: { (error) in
    if error == nil {
        print("Successfully muted")
    } else {
        print("Failed to mute")
    }
})

```

비동기로 로컬 음소거 해제

```swift
call!.unmute(completionHandler:{ (error) in
    if error == nil {
        print("Successfully un-muted")
    } else {
        print("Failed to unmute")
    }
})
```

### <a name="start-and-stop-sending-local-video"></a>로컬 비디오 보내기 시작 및 중지

호출에서 다른 참가자에 게 로컬 비디오 전송을 시작 하려면 API를 사용 하 고 다음을 사용 하 여를 전달 합니다. `startVideo` `localVideoStream``camera`

```swift

let firstCamera: VideoDeviceInfo? = self.deviceManager?.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: firstCamera)

call!.startVideo(stream: localVideoStream) { (error) in
    if (error == nil) {
        print("Local video started successfully")
    }
    else {
        print("Local video failed to start")
    }
}

```

비디오 전송을 시작 하면 `ACSLocalVideoStream` 인스턴스는 `localVideoStreams` 호출 인스턴스에 컬렉션을 추가 합니다.

```swift

call.localVideoStreams[0]

```

비동기로 로컬 비디오를 중지 하려면 `localVideoStream` 의 호출에서 반환 된를 전달 합니다 `call.startVideo` .

```swift

call!.stopVideo(stream: localVideoStream) { (error) in
    if (error == nil) {
        print("Local video stopped successfully")
    } else {
        print("Local video failed to stop")
    }
}

```

## <a name="remote-participants-management"></a>원격 참여자 관리

모든 원격 참가자는 형식으로 표시 되며 `ACSRemoteParticipant` `remoteParticipants` 호출 인스턴스의 컬렉션을 통해 사용할 수 있습니다.

### <a name="list-participants-in-a-call"></a>호출의 참가자 나열

```swift

call.remoteParticipants

```

### <a name="remote-participant-properties"></a>원격 참가자 속성

```swift

// [ACSRemoteParticipantDelegate] delegate - an object you provide to receive events from this ACSRemoteParticipant instance
var remoteParticipantDelegate = remoteParticipant.delegate

// [CommunicationIdentifier] identity - same as the one used to provision token for another user
var identity = remoteParticipant.identity

// ACSParticipantStateIdle = 0, ACSParticipantStateEarlyMedia = 1, ACSParticipantStateConnecting = 2, ACSParticipantStateConnected = 3, ACSParticipantStateOnHold = 4, ACSParticipantStateInLobby = 5, ACSParticipantStateDisconnected = 6
var state = remoteParticipant.state

// [ACSError] callEndReason - reason why participant left the call, contains code/subcode/message
var callEndReason = remoteParticipant.callEndReason

// [Bool] isMuted - indicating if participant is muted
var isMuted = remoteParticipant.isMuted

// [Bool] isSpeaking - indicating if participant is currently speaking
var isSpeaking = remoteParticipant.isSpeaking

// ACSRemoteVideoStream[] - collection of video streams this participants has
var videoStreams = remoteParticipant.videoStreams // [ACSRemoteVideoStream, ACSRemoteVideoStream, ...]

```

### <a name="add-a-participant-to-a-call"></a>호출에 참가자 추가

호출 (사용자 또는 전화 번호)에 참가자를 추가 하려면를 호출 하면 `addParticipant` 됩니다. 그러면 원격 참가자 인스턴스가 동기적으로 반환 됩니다.

```swift

let remoteParticipantAdded: RemoteParticipant = call.add(participant: CommunicationUser(identifier: "userId"))

```

### <a name="remove-a-participant-from-a-call"></a>호출에서 참가자 제거
통화 (사용자 또는 전화 번호)에서 참가자를 제거 하려면 API를 호출할 수 있습니다  `removeParticipant` . 이렇게 하면 비동기적으로 확인 됩니다.

```swift

call!.remove(participant: remoteParticipantAdded) { (error) in
    if (error == nil) {
        print("Successfully removed participant")
    } else {
        print("Failed to remove participant")
    }
}

```

## <a name="render-remote-participant-video-streams"></a>원격 참가자 비디오 스트림 렌더링

원격 참가자는 전화 통화 중에 비디오 또는 화면 공유를 시작할 수 있습니다.

### <a name="handle-remote-participant-videoscreen-sharing-streams"></a>원격 참가자 처리 비디오/화면 공유 스트림

원격 참가자의 스트림을 나열 하려면 컬렉션을 검사 합니다 `videoStreams` .

```swift

var remoteParticipantVideoStream = call.remoteParticipants[0].videoStreams[0]

```

### <a name="remote-video-stream-properties"></a>원격 비디오 스트림 속성

```swift

var type: MediaStreamType = remoteParticipantVideoStream.type // 'ACSMediaStreamTypeVideo'

var isAvailable: Bool = remoteParticipantVideoStream.isAvailable // indicates if remote stream is available

var id: Int = remoteParticipantVideoStream.id // id of remoteParticipantStream

```

### <a name="render-remote-participant-stream"></a>원격 참가자 스트림 렌더링

원격 참가자 스트림 렌더링을 시작 하려면 다음을 수행 합니다.

```swift

let renderer: Renderer? = Renderer(remoteVideoStream: remoteParticipantVideoStream)
let targetRemoteParticipantView: RendererView? = renderer?.createView(with: RenderingOptions(scalingMode: ScalingMode.crop))
// To update the scaling mode later
targetRemoteParticipantView.update(scalingMode: ScalingMode.fit)

```

### <a name="remote-video-renderer-methods-and-properties"></a>원격 비디오 렌더러 메서드 및 속성

```swift
// [Bool] isRendering - indicating if stream is being rendered
remoteVideoRenderer.isRendering()
// [Synchronous] dispose() - dispose renderer and all `RendererView` associated with this renderer. To be called when you have removed all associated views from the UI.
remoteVideoRenderer.dispose()
```

## <a name="device-management"></a>디바이스 관리

`DeviceManager` 오디오/비디오 스트림 전송 호출에 사용할 수 있는 로컬 장치를 열거할 수 있습니다. 또한 사용자가 마이크/카메라에 액세스할 수 있는 권한을 요청할 수 있습니다. `deviceManager`개체에 액세스할 수 있습니다 `callClient` .

```swift

self.callClient!.getDeviceManager(
    completionHandler: { (deviceManager, error) in
        if (error == nil) {
            print("Got device manager instance")
            self.deviceManager = deviceManager
        } else {
            print("Failed to get device manager instance")
        }
    })
```

### <a name="enumerate-local-devices"></a>로컬 장치 열거

로컬 장치에 액세스 하려면 Device Manager 열거 메서드를 사용할 수 있습니다. 열거는 동기 작업입니다.

```swift
// enumerate local cameras
var localCameras = deviceManager.getCameraList() // [ACSVideoDeviceInfo, ACSVideoDeviceInfo...]
// enumerate local cameras
var localMicrophones = deviceManager.getMicrophoneList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
// enumerate local cameras
var localSpeakers = deviceManager.getSpeakerList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
``` 

### <a name="set-default-microphonespeaker"></a>기본 마이크/스피커 설정

장치 관리자를 사용 하 여 호출을 시작할 때 사용할 기본 장치를 설정할 수 있습니다. 스택 기본값을 설정 하지 않으면 통신 서비스는 OS 기본값으로 대체 됩니다.

```swift
// get first microphone
var firstMicrophone = self.deviceManager!.getMicrophoneList()![0]
// [Synchronous] set microphone
deviceManager.setMicrophone(microphoneDevice: firstMicrophone)
// get first speaker
var firstSpeaker = self.deviceManager!.getSpeakerList()![0]
// [Synchronous] set speaker
deviceManager.setSpeaker(speakerDevice: firstSpeaker)
```

### <a name="local-camera-preview"></a>로컬 카메라 미리 보기

`ACSRenderer`를 사용 하 여 로컬 카메라에서 스트림 렌더링을 시작할 수 있습니다. 이 스트림은 다른 참가자로 전송 되지 않습니다. 로컬 미리 보기 피드입니다. 비동기 작업입니다.

```swift

let camera: VideoDeviceInfo = self.deviceManager!.getCameraList()![0]
let localVideoStream: LocalVideoStream = LocalVideoStream(camera: camera)
let renderer: Renderer = Renderer(localVideoStream: localVideoStream)
self.view = renderer!.createView()

```

### <a name="local-camera-preview-properties"></a>로컬 카메라 미리 보기 속성

렌더러에는 렌더링을 제어 하는 데 사용할 수 있는 속성 및 메서드 집합이 있습니다.

```swift

// Constructor can take in ACSLocalVideoStream or ACSRemoteVideoStream
let localRenderer = Renderer(localVideoStream:localVideoStream)
let remoteRenderer = Renderer(remoteVideoStream:remoteVideoStream)

// [ACSStreamSize] size of the rendering view
localRenderer.size

// [ACSRendererDelegate] an object you provide to receive events from this ACSRenderer instance
localRenderer.delegate

// [Synchronous] create view
try! localRenderer.createView()

// [Synchronous] create view with rendering options
try! localRenderer.createView(with: RenderingOptions(scalingMode: ScalingMode.fit))

// [Synchronous] dispose rendering view
localRenderer.dispose()

```

## <a name="eventing-model"></a>이벤트 모델

값이 변경 될 때 알림을 받으려면 대부분의 속성 및 컬렉션을 구독할 수 있습니다.

### <a name="properties"></a>속성
이벤트를 구독 하려면 `property changed` :

```swift
call.delegate = self
// Get the property of the call state by doing get on the call's state member
public func onCallStateChanged(_ call: Call!,
                               args: PropertyChangedEventArgs!)
{
    print("Callback from SDK when the call state changes, current state: " + call.state.rawValue)
}

 // to unsubscribe
 self.call.delegate = nil

```

### <a name="collections"></a>컬렉션
이벤트를 구독 하려면 `collection updated` :

```swift
call.delegate = self
// Collection contains the streams that were added or removed only
public func onLocalVideoStreamsChanged(_ call: Call!,
                                       args: LocalVideoStreamsUpdatedEventArgs!)
{
    print(args.addedStreams.count)
    print(args.removedStreams.count)
}
// to unsubscribe
self.call.delegate = nil
```
