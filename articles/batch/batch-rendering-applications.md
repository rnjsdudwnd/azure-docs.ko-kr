---
title: 렌더링 애플리케이션
description: Azure Batch를 모든 렌더링 애플리케이션에 사용할 수 있습니다. 그러나 Azure Marketplace VM 이미지는 미리 설치된 일반적인 애플리케이션에 사용할 수 있습니다.
ms.date: 02/01/2021
ms.topic: how-to
ms.openlocfilehash: 4eeb87c866858f2f1733b81e3e2154f4521e94bc
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99430031"
---
# <a name="pre-installed-applications-on-batch-rendering-vm-images"></a>일괄 처리 렌더링 VM 이미지에 미리 설치 된 응용 프로그램

Azure Batch를 모든 렌더링 애플리케이션에 사용할 수 있습니다. 그러나 Azure Marketplace VM 이미지는 미리 설치된 일반적인 애플리케이션에 사용할 수 있습니다.

해당하는 경우 미리 설치된 렌더링 애플리케이션에 종량제 라이선스를 사용할 수 있습니다. Batch 풀을 만들 때 필요한 애플리케이션을 지정할 수 있고 VM 및 애플리케이션 비용이 분 단위로 청구됩니다. 애플리케이션 가격은 [Azure Batch 가격 책정 페이지](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering)에 나열되어 있습니다.

일부 애플리케이션은 Windows만 지원하나 대부분 Windows와 Linux를 모두 지원합니다.

## <a name="applications-on-centos-7-rendering-image"></a>CentOS 7 렌더링 이미지의 응용 프로그램

다음 목록은 CentOS 7.6, 버전 1.1.6 렌더링 이미지에 적용됩니다.

* Autodesk Maya I/O 2017 업데이트 5(201708032230 잘라내기)
* Autodesk Maya I/O 2018 업데이트 2(201711281015 컷)
* Autodesk Maya I/O 2019 업데이트 1
* Autodesk Arnold for Maya 2017(Arnold 버전 5.3.1.1) MtoA-3.2.1.1-2017
* Autodesk Arnold for Maya 2018(Arnold 버전 5.3.1.1) MtoA-3.2.1.1-2018
* Autodesk Arnold for Maya 2019(Arnold 버전 5.3.1.1) MtoA-3.2.1.1-2019
* Maya 2017용 Chaos Group V-Ray(버전 3.60.04)
* Maya 2018용 Chaos Group V-Ray(버전 3.60.04)
* Blender(2.68)
* Blender(2.8)

## <a name="applications-on-latest-windows-server-rendering-image"></a>최신 Windows Server 렌더링 이미지의 응용 프로그램

다음 목록은 Windows Server 렌더링 이미지 버전 1.5.0에 적용 됩니다.

* Autodesk Maya i/o 2020 업데이트 4.4
* Autodesk 3ds Max i/o 2021 업데이트 3
* Maya 2020에 대 한 Autodesk Arnold (Arnold version 6.1.0.1) MtoA-4.1.1.1-2020
* 3ds Max 2021 (Arnold version 6.1.0.1)에 대 한 Autodesk Arnold MAXtoA-4.2.2.20-2021
* Maya 2020 (버전 5.00.21)의 비정상 그룹 V-광선
* 3ds Max 2021 (version 5.00.05)의 비정상 그룹 V-광선
* Blender(2.79)
* Blender(2.80)
* AZ 10

> [!IMPORTANT]
> [Azure Batch 확장 템플릿](https://github.com/Azure/batch-extension-templates) 외부에서 V-Ray를 Maya와 함께 실행하려면 렌더링을 실행하기 전에 `vrayses.exe`를 시작합니다. 템플릿 외부에서 vrayses.exe를 시작하려면 `%MAYA_2020%\vray\bin\vrayses.exe"`라는 명령을 사용하면 됩니다.
>
> 예를 들어 GitHub에서 [Maya 및 V-Ray 템플릿](https://github.com/Azure/batch-extension-templates/blob/master/templates/maya/render-vray-windows/pool.template.json)의 시작 작업을 참조하세요.

## <a name="applications-on-previous-windows-server-rendering-images"></a>이전 Windows Server의 응용 프로그램 렌더링 이미지

다음 목록은 Windows Server 2016, 버전 1.3.8 렌더링 이미지에 적용됩니다.

* Autodesk Maya I/O 2017 업데이트 5(버전 17.4.5459)
* Autodesk Maya I/O 2018 업데이트 6(버전 18.4.0.7622)
* Autodesk Maya I/O 2019
* Autodesk 3ds Max I/O 2018 업데이트 4(버전 20.4.0.4254)
* Autodesk 3ds Max I/O 2019 업데이트 1(버전 21.2.0.2219)
* Autodesk 3ds Max I/O 2020 업데이트 2
* Autodesk Arnold for Maya 2017(Arnold 버전 5.3.0.2) MtoA-3.2.0.2-2017
* Autodesk Arnold for Maya 2018(Arnold 버전 5.3.0.2) MtoA-3.2.0.2-2018
* Autodesk Arnold for Maya 2019(Arnold 버전 5.3.0.2) MtoA-3.2.0.2-2019
* Autodesk Arnold for 3ds Max 2018(Arnold 버전 5.3.0.2)(버전 1.2.926)
* Autodesk Arnold for 3ds Max 2019(Arnold 버전 5.3.0.2)(버전 1.2.926)
* Autodesk Arnold for 3ds Max 2020(Arnold 버전 5.3.0.2)(버전 1.2.926)
* Chaos Group V-Ray for Maya 2017(버전 4.12.01)
* Chaos Group V-Ray for Maya 2018(버전 4.12.01)
* Chaos Group V-Ray for Maya 2019(버전 4.04.03)
* Chaos Group V-Ray for 3ds Max 2018(버전 4.20.01)
* Chaos Group V-Ray for 3ds Max 2019(버전 4.20.01)
* Chaos Group V-Ray for 3ds Max 2020(버전 4.20.01)
* Blender(2.79)
* Blender(2.80)
* AZ 10

다음 목록은 Windows Server 2016, 버전 1.3.7 렌더링 이미지에 적용됩니다.

* Autodesk Maya I/O 2017 업데이트 5(버전 17.4.5459)
* Autodesk Maya I/O 2018 업데이트 4(버전 18.4.0.7622)
* Autodesk 3ds Max I/O 2019 업데이트 1(버전 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 업데이트 4(버전 20.4.0.4254)
* Autodesk Arnold for Maya 2017(Arnold 버전 5.2.0.1) MtoA-3.1.0.1-2017
* Autodesk Arnold for Maya 2018(Arnold 버전 5.2.0.1) MtoA-3.1.0.1-2018
* Autodesk Arnold for 3ds Max 2018(Arnold 버전 5.0.2.4)(버전 1.2.926)
* Autodesk Arnold for 3ds Max 2019(Arnold 버전 5.0.2.4)(버전 1.2.926)
* Chaos Group V-Ray for Maya 2018(버전 3.52.03)
* Chaos Group V-Ray for 3ds Max 2018(버전 3.60.02)
* Chaos Group V-Ray for Maya 2019(버전 3.52.03)
* Chaos Group V-Ray for 3ds Max 2019(버전 4.10.01)
* Blender(2.79)

> [!NOTE]
> Chaos Group V-Ray for 3ds Max 2019(버전 4.10.01)에는 V-ray에 대한 호환성이 손상되는 변경이 도입되었습니다. 이전 버전(버전 3.60.02)을 사용하려면 Windows Server 2016, 버전 1.3.2 렌더링 노드를 사용하세요.

## <a name="next-steps"></a>다음 단계

렌더링 VM 이미지를 사용하려면 풀을 만들 때 풀 구성에서 지정해야 합니다. [렌더링을 위한 Batch 풀 기능](./batch-rendering-functionality.md)을 참조하세요.
