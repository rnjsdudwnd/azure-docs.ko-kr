---
title: 계산 인스턴스에 모델 배포
titleSuffix: Azure Machine Learning
description: 계산 인스턴스를 사용 하 여 Azure Machine Learning 모델을 웹 서비스로 배포 하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, deploy
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 03/05/2020
ms.openlocfilehash: d8b1c2a5384e479e39d169d368554f16c300a33e
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98954549"
---
# <a name="deploy-a-model-to-azure-machine-learning-compute-instances"></a>모델을 배포 하 여 계산 인스턴스 Azure Machine Learning



Azure Machine Learning를 사용 하 여 Azure Machine Learning 계산 인스턴스에서 모델을 웹 서비스로 배포 하는 방법을 알아봅니다. 다음 조건 중 하나에 해당 하는 경우 compute 인스턴스를 사용 합니다.

- 모델을 빠르게 배포하고 유효성을 검사해야 합니다.
- 개발 중인 모델을 테스트합니다.

> [!TIP]
> 계산 인스턴스의 Jupyter Notebook에서 동일한 VM의 웹 서비스로 모델을 배포 하는 것은 _로컬 배포_ 입니다. 이 경우 ' 로컬 ' 컴퓨터는 계산 인스턴스입니다. 배포에 대 한 자세한 내용은 [Azure Machine Learning를 사용 하 여 모델 배포](how-to-deploy-and-where.md)를 참조 하세요.

## <a name="prerequisites"></a>사전 요구 사항

- 계산 인스턴스가 실행 되는 Azure Machine Learning 작업 영역입니다. 자세한 내용은 [환경 및 작업 영역 설정](tutorial-1st-experiment-sdk-setup.md)을 참조 하세요.

## <a name="deploy-to-the-compute-instances"></a>계산 인스턴스에 배포

로컬 배포를 보여 주는 예제 노트북은 계산 인스턴스에 포함 됩니다. 다음 단계를 사용 하 여 노트북을 로드 하 고 모델을 VM에 웹 서비스로 배포 합니다.

1. [Azure Machine Learning studio](https://ml.azure.com)에서 "노트북"을 선택한 다음 "샘플 노트북"의 how-to-use-azureml/deployment/deploy-to-local/register-model-deploy-local를 선택 합니다. 이 노트북을 사용자 폴더에 복제 합니다.

1. 1 단계에서 복제 된 노트북을 찾고, 노트북을 실행할 계산 인스턴스를 선택 하거나 만듭니다.

    ![노트북에서 실행 중인 로컬 서비스의 스크린샷](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service.png)


1. 노트북에는 서비스가 실행 되는 URL 및 포트가 표시 됩니다. 예들 들어 `https://localhost:6789`입니다. 가 포함 된 셀을 실행 하 여 포트를 표시할 수도 있습니다 `print('Local service port: {}'.format(local_service.port))` .

    ![실행 중인 로컬 서비스 포트의 스크린샷](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service-port.png)

1. 계산 인스턴스에서 서비스를 테스트 하려면 URL을 사용 `https://localhost:<local_service.port>` 합니다. 원격 클라이언트에서 테스트 하려면 계산 인스턴스에서 실행 되는 서비스의 공용 URL을 가져옵니다. 공용 URL은 다음 수식을 사용 하 여 확인할 수 있습니다. 
    * 노트북 `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.notebooks.azureml.net/score` VM: 
    * 계산 인스턴스: `https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.instances.azureml.net/score` . 

    예를 들면 다음과 같습니다. 
    * 노트북 VM: `https://vm-name-6789.northcentralus.notebooks.azureml.net/score` 
    * 계산 인스턴스: `https://vm-name-6789.northcentralus.instances.azureml.net/score`

## <a name="test-the-service"></a>서비스 테스트

실행 중인 서비스에 샘플 데이터를 제출 하려면 다음 코드를 사용 합니다. 의 값을 `service_url` 이전 단계의 URL로 바꿉니다.

> [!NOTE]
> 계산 인스턴스에서 배포를 인증 하는 경우 Azure Active Directory를 사용 하 여 인증을 수행 합니다. `interactive_auth.get_authentication_header()`예제 코드에서를 호출 하면 AAD를 사용 하 여 인증 하 고, 계산 인스턴스에서 서비스를 인증 하는 데 사용할 수 있는 헤더를 반환 합니다. 자세한 내용은 [Azure Machine Learning 리소스 및 워크플로에 대한 인증 설정](how-to-setup-authentication.md#interactive-authentication)을 참조하세요.
>
> Azure Kubernetes Service 또는 Azure Container Instances에서 배포를 인증 하는 경우 다른 인증 방법이 사용 됩니다. 에 대 한 자세한 내용은 [웹 서비스로 배포 된 Azure 컴퓨터 모델에 대 한 인증 구성](how-to-authenticate-web-service.md)을 참조 하세요.

```python
import requests
import json
from azureml.core.authentication import InteractiveLoginAuthentication

# Get a token to authenticate to the compute instance from remote
interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()

# Create and submit a request using the auth header
headers = auth_header
# Add content type header
headers.update({'Content-Type':'application/json'})

# Sample data to send to the service
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

# Replace with the URL for your compute instance, as determined from the previous section
service_url = "https://vm-name-6789.northcentralus.notebooks.azureml.net/score"
# for a compute instance, the url would be https://vm-name-6789.northcentralus.instances.azureml.net/score
resp = requests.post(service_url, test_sample, headers=headers)
print("prediction:", resp.text)
```

## <a name="next-steps"></a>다음 단계

* [사용자 지정 Docker 이미지를 사용 하 여 모델을 배포 하는 방법](how-to-deploy-custom-docker-image.md)
* [배포 문제 해결](how-to-troubleshoot-deployment.md)
* [TLS를 사용하여 Azure Machine Learning을 통해 웹 서비스 보호](how-to-secure-web-service.md)
* [웹 서비스로 배포된 ML 모델 사용](how-to-consume-web-service.md)
* [Application Insights를 사용하여 Azure Machine Learning 모델 모니터링](how-to-enable-app-insights.md)
* [프로덕션 환경에서 모델용 데이터 수집](how-to-enable-data-collection.md)
