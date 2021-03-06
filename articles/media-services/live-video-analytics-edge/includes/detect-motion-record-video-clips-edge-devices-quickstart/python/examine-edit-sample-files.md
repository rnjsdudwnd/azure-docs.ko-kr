---
ms.openlocfilehash: 6ea9be8e7e67a8e52412e7011cfb1d33c9929191
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97486696"
---
이 빠른 시작의 필수 구성 요소로 샘플 코드를 폴더에 다운로드했습니다. 다음 단계에 따라 샘플 코드를 검사하고 편집합니다.

1. Visual Studio Code에서 *src/edge* 로 이동합니다. *.env* 파일 및 몇 가지 배포 템플릿 파일이 표시됩니다.

    배포 템플릿은 변수가 일부 속성에 사용된 에지 디바이스에 대한 배포 매니페스트를 나타냅니다. *.env* 파일에는 이러한 변수에 대한 값이 포함되어 있습니다.
1. *src/cloud-to-device-console-app* 폴더로 이동합니다. 여기에는 *appsettings.json* 파일 및 몇 가지 다른 파일이 표시됩니다.
    * ***operations.json** _ - 프로그램에서 실행하려는 작업 목록입니다.
    _ ***main.py** _ - 샘플 프로그램 코드입니다. 이 코드에서는 다음을 수행합니다.

        _ 앱 설정을 로드합니다.
        * Live Video Analytics on IoT Edge 모듈에서 공개하는 직접 메서드를 호출합니다. 이 모듈을 통해 [직접 메서드](../../../direct-methods.md)를 호출하여 라이브 비디오 스트림을 분석할 수 있습니다. 
        * **터미널** 창에서 프로그램의 출력을 검사하고 **출력** 창에서 모듈이 생성한 이벤트를 검사할 수 있도록 일시 중지합니다.
        * 리소스를 정리하는 직접 메서드를 호출합니다.

1. *operations.json* 파일을 편집합니다.
    * 그래프 토폴로지의 링크를 변경합니다.

        `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-motion-files/2.0/topology.json"`
    * `GraphInstanceSet` 아래에서 이전 링크의 값과 일치하도록 그래프 토폴로지의 이름을 편집합니다.
    
      `"topologyName" : "EVRToFilesOnMotionDetection"`
    * 다음과 같이 비디오 파일을 가리키도록 RTSP URL을 편집합니다.

        `"value": "rtsp://rtspsim:554/media/lots_015.mkv"`
    * 다음과 같이 `GraphTopologyDelete` 아래에서 이름을 편집합니다.

        `"name": "EVRToFilesOnMotionDetection"`
