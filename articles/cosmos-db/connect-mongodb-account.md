---
title: Azure Cosmos DB에 MongoDB 애플리케이션 연결
description: 에서 연결 문자열을 가져와 Azure Cosmos DB에 MongoDB 앱을 연결 하는 방법에 대해 알아봅니다 Azure Portal
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/19/2020
ms.reviewer: sngun
adobe-target: true
adobe-target-activity: DocsExp-A/B-384740-MongoDB-2.8.2021
adobe-target-experience: Experience B
adobe-target-content: connect-mongodb-account-experimental.md
ms.openlocfilehash: d3e619559f1fb2500904aa7349aa131fd9b5fc00
ms.sourcegitcommit: 706e7d3eaa27f242312d3d8e3ff072d2ae685956
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99980799"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Azure Cosmos DB에 MongoDB 애플리케이션 연결
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

MongoDB 연결 문자열을 사용하여 MongoDB 앱을 Azure Cosmos DB에 연결하는 방법에 대해 알아봅니다. 그런 다음 Azure Cosmos 데이터베이스를 MongoDB 앱의 데이터 저장소로 사용할 수 있습니다.

이 자습서에서는 연결 문자열 정보를 검색하는 다음 두 가지 방법을 제공합니다.

- [빠른 시작 방법](#get-the-mongodb-connection-string-by-using-the-quick-start) - .NET, Node.js, MongoDB Shell, Java 및 Python 드라이버와 함께 사용
- 다른 드라이버에 사용할 [사용자 지정 연결 문자열 방법](#get-the-mongodb-connection-string-to-customize)

## <a name="prerequisites"></a>필수 구성 요소

- Azure 계정. Azure 계정이 없으면 지금 [무료 Azure 계정](https://azure.microsoft.com/free/)을 만듭니다.
- Cosmos 계정입니다. 지침은 [Azure Cosmos DB의 API for MongoDB 및 .NET SDK를 사용하여 웹앱 빌드](create-mongodb-dotnet.md)를 참조하세요.

## <a name="get-the-mongodb-connection-string-by-using-the-quick-start"></a>빠른 시작을 사용하여 MongoDB 연결 문자열 가져오기

1. 인터넷 브라우저에서 [Azure Portal](https://portal.azure.com)에 로그인 합니다.
2. **Azure Cosmos DB** 블레이드에서 API를 선택합니다.
3. 계정 블레이드의 왼쪽 창에서 **빠른 시작** 을 클릭합니다.
4. 플랫폼(**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**)을 선택합니다. 드라이버나 도구가 목록에 없더라도 계속해서 더 많은 연결 코드 조각을 문서화하므로 걱정하지 마세요. 아래에 보고 싶은 항목에 대한 의견을 보내주세요. 사용자 고유의 연결을 만드는 방법을 알아보려면 [계정 연결 문자열 정보 가져오기](#get-the-mongodb-connection-string-to-customize)를 참조하세요.
5. 코드 조각을 복사하여 MongoDB 앱에 붙여넣습니다.

    :::image type="content" source="./media/connect-mongodb-account/QuickStartBlade.png" alt-text="빠른 시작 블레이드":::

## <a name="get-the-mongodb-connection-string-to-customize"></a> 사용자 지정할 MongoDB 연결 문자열 가져오기

1. 인터넷 브라우저에서 [Azure Portal](https://portal.azure.com)에 로그인 합니다.
2. **Azure Cosmos DB** 블레이드에서 API를 선택합니다.
3. 계정 블레이드의 왼쪽 창에서 **연결 문자열** 을 클릭합니다.
4. **연결 문자열** 블레이드가 열립니다. 여기에는 미리 구성된 연결 문자열을 비롯해 MongoDB용 드라이버를 사용하여 계정에 연결하는 데 필요한 모든 정보가 있습니다.

   :::image type="content" source="./media/connect-mongodb-account/ConnectionStringBlade.png" alt-text="연결 문자열 블레이드" lightbox= "./media/connect-mongodb-account/ConnectionStringBlade.png" :::

## <a name="connection-string-requirements"></a>연결 문자열 요구 사항

> [!Important]
> Azure Cosmos DB에는 엄격한 보안 요구 사항과 표준이 있습니다. Azure Cosmos DB 계정은 *TLS* 를 통한 인증 및 보안 통신이 필요 합니다.

Azure Cosmos DB는 표준 MongoDB 연결 문자열 URI 형식을 지원 합니다. Azure Cosmos DB 계정에는 TLS를 통한 인증 및 보안 통신이 필요 합니다. 따라서 연결 문자열 형식은 다음과 같습니다.

`mongodb://username:password@host:port/[database]?ssl=true`

이 문자열의 값은 위에 표시된 **연결 문자열** 블레이드에서 사용할 수 있습니다.

* Username (필수): Cosmos account name.
* 암호 (필수): Cosmos 계정 암호입니다.
* Host (필수): Cosmos 계정의 FQDN입니다.
* Port(필수): 10255
* Database(선택): 연결에서 사용하는 데이터베이스. 제공된 데이터베이스가 없는 경우 기본 데이터베이스는 "test"입니다.
* ssl=true(필수)

예를 들어 **연결 문자열** 블레이드에 표시된 계정을 사용하는 것이 좋습니다. 유효한 연결 문자열은 다음과 같습니다.

`mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true`

## <a name="next-steps"></a>다음 단계

- Azure Cosmos DB의 API for MongoDB와 함께 [Studio 3T를 사용](mongodb-mongochef.md)하는 방법을 알아봅니다.
- Azure Cosmos DB의 API for MongoDB와 함께 [Robo 3T를 사용](mongodb-robomongo.md)하는 방법을 알아봅니다.
- Azure Cosmos DB의 API for MongoDB를 사용하여 MongoDB [샘플](mongodb-samples.md)을 살펴봅니다.
