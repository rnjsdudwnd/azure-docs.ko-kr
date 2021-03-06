---
title: 연결 문자열-Azure Database for MariaDB
description: 이 문서에는 ADO.NET(C#), JDBC, Node.js, ODBC, PHP, Python, Ruby 등 애플리케이션을 Azure Database for MariaDB와 연결하려는 경우 현재 지원되는 연결 문자열이 나와 있습니다.
author: savjani
ms.author: pariks
ms.service: jroth
ms.topic: how-to
ms.date: 3/18/2020
ms.custom: devx-track-python, devx-track-js, devx-track-csharp
ms.openlocfilehash: 309c8b55910c5223f377571edb7207e5b34bd219
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98662180"
---
# <a name="how-to-connect-applications-to-azure-database-for-mariadb"></a>Azure Database for MariaDB에 애플리케이션을 연결하는 방법
이 항목에는 템플릿 및 예제와 함께 Azure Database for MariaDB에서 지원되는 연결 문자열 형식이 나와 있습니다. 연결 문자열에 다른 매개 변수 및 설정을 사용할 수도 있습니다.

- 인증서를 가져오려면 [SSL 구성 방법](./howto-configure-ssl.md)을 참조하세요.
- {your_host} = [servername].mariadb.database.azure.com
- {your_user}@{servername} = 올바른 인증을 위한 사용자 ID 형식입니다.  사용자 ID만 사용하는 경우 인증이 실패합니다.

## <a name="adonet"></a>ADO.NET
```csharp
Server={your_host}; Port=3306; Database={your_database}; Uid={username@servername}; Pwd={your_password}; SslMode=Preferred;
```

이 예제에서 서버 이름은 `mydemoserver`이고, 데이터베이스 이름은 `wpdb`이고, 사용자 이름은 `WPAdmin`이고, 암호는 `mypassword!2`입니다. 결과적으로 연결 문자열은 다음과 같습니다.

```csharp
Server= "mydemoserver.mariadb.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```java
String url ="jdbc:mariadb://{your_host}:3306/{your_database}?useSSL=true&trustServerCertificate=true"; myDbConn = DriverManager.getConnection(url, "{username@servername}", {your_password});
```

## <a name="nodejs"></a>Node.js
```javascript
var conn = mysql.createConnection({host: "{your_host}", user: "{your_username}", password: {your_password}, database: {your_database}, port: 3306, ssl:{ca:fs.readFileSync({ca-cert filename})}});
```

## <a name="odbc"></a>ODBC
```cpp
DRIVER={MARIADB ODBC 3.0 Driver}; Server="{your_host}"; Port=3306; Database={your_database}; Uid="{username@servername}"; Pwd={your_password}; sslca={ca-cert filename}; sslverify=1;
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "{your_host}", "{username@servername}", {your_password}, {your_database}, 3306);
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user="{username@servername}", password={your_password}, host="{your_host}", port=3306, database={your_database}, ssl_ca={ca-cert filename}, ssl_verify_cert=true)
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: "{username@servername}", password: {your_password}, database: {your_database}, host: "{your_host}", port: 3306, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA')
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>Azure Portal에서 연결 문자열 세부 정보 가져오기
[Azure Portal](https://portal.azure.com)에서 Azure Database for MariaDB 서버로 이동한 다음 **연결 문자열** 을 클릭하여 사용자 인스턴스에 대한 문자열 목록을 가져옵니다. ![Azure Portal의 연결 문자열 창](./media/howto-connection-strings/connection-strings-on-portal.png)

문자열은 드라이버, 서버 및 다른 데이터베이스 연결 매개 변수와 같은 세부 정보를 제공합니다. 데이터베이스 이름, 암호 등과 같은 고유한 매개 변수를 사용하여 이러한 예제를 수정합니다. 그런 다음, 이 문자열을 사용하여 사용자 코드와 애플리케이션에서 서버에 연결할 수 있습니다.

<!-- 
## Next steps
- For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).
- -->
