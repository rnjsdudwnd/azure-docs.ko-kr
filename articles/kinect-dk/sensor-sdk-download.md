---
title: Azure Kinect 센서 SDK 다운로드
description: Windows 및 Linux에서 Azure Kinect 센서 SDK를 다운로드하고 설치하는 방법에 대해 알아봅니다.
author: Brent-A
ms.author: brenta
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: azure, kinect, sdk, 업데이트 다운로드, 최신, 사용 가능, 설치
ms.openlocfilehash: 2fd14781c42192c713d826729f8fab6c698d6321
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505480"
---
# <a name="azure-kinect-sensor-sdk-download"></a>Azure Kinect 센서 SDK 다운로드

이 페이지에는 각 Azure Kinect 센서 SDK 버전에 대한 다운로드 링크가 있습니다. 설치 관리자는 Azure Kinect에 대해 개발하는 데 필요한 모든 파일을 제공합니다.

## <a name="azure-kinect-sensor-sdk-contents"></a>Azure Kinect 센서 SDK 콘텐츠

- Azure Kinect DK를 사용하는 애플리케이션을 빌드하기 위한 헤더 및 라이브러리
- Azure Kinect DK를 사용하는 애플리케이션에 필요한 재배포 가능 DLL
- [Azure Kinect 뷰어](azure-kinect-viewer.md)
- [Azure Kinect 레코더](azure-kinect-recorder.md)
- [Azure Kinect 펌웨어 도구](azure-kinect-firmware-tool.md)

## <a name="windows-installation-instructions"></a>Windows 설치 지침

Azure Kinect 센서 SDK 및 펌웨어의 최신 버전 및 이전 버전에 대 한 설치 세부 정보는 [여기](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md)에서 확인할 수 있습니다.

소스 코드는 [여기](https://github.com/microsoft/Azure-Kinect-Sensor-SDK)서 찾을 수 있습니다.

> [!NOTE]
> SDK를 설치할 때 설치 경로를 기억하세요. 예를 들어"C:\Program Files\Azure Kinect SDK 1.2"입니다. 문서에서 참조하는 도구를 이 경로에서 찾을 수 있습니다.

## <a name="linux-installation-instructions"></a>Linux 설치 지침

현재 유일하게 지원되는 배포는 Ubuntu 18.04입니다. 다른 배포에 대한 지원을 요청하려면 [이 페이지](https://aka.ms/azurekinectfeedback)를 참조하세요.

먼저 [여기](/windows-server/administration/linux-package-repository-for-microsoft-software) 지침에 따라 [Microsoft의 패키지 리포지토리](https://packages.microsoft.com/)를 구성해야 합니다.

이제 필요한 패키지를 설치할 수 있습니다. `k4a-tools` 패키지에는 [Azure Kinect 뷰어](azure-kinect-viewer.md), [Azure Kinect 레코더](record-sensor-streams-file.md) 및 [Azure Kinect 펌웨어 도구](azure-kinect-firmware-tool.md)가 포함되어 있습니다. 설치하려면 다음을 실행합니다.

 `sudo apt install k4a-tools`

 `libk4a<major>.<minor>-dev` 패키지에는 `libk4a`에 대해 빌드할 헤더와 CMake 파일이 포함되어 있습니다.
`libk4a<major>.<minor>` 패키지에는 `libk4a`에 종속된 실행 파일을 실행하는 데 필요한 공유 개체가 포함되어 있습니다.

 기본 자습서에는 `libk4a<major>.<minor>-dev` 패키지가 필요합니다. 설치하려면 다음을 실행합니다.

 `sudo apt install libk4a1.1-dev`

명령이 성공하면 SDK를 사용할 준비가 된 것입니다.

## <a name="change-log-and-older-versions"></a>변경 로그 및 이전 버전

Azure Kinect 센서 SDK에 대한 변경 로그는 [여기](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/CHANGELOG.md)에서 찾을 수 있습니다.

이전 버전의 Azure Kinect 센서 SDK가 필요한 경우 [여기](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md)에서 찾습니다.

## <a name="next-steps"></a>다음 단계

[Azure Kinect DK 설정](set-up-azure-kinect-dk.md)