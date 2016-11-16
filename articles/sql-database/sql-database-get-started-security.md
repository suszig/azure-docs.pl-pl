---
title: "Samouczek usługi SQL Database: wprowadzenie do zabezpieczeń"
description: "Dowiedz się, jak tworzyć konta służące do uzyskiwania dostępu do bazy danych i zarządzania nią."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/17/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a3bd8ac5466ae67df0a1865509c8fb897d1374ec


---
# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Samouczek usługi SQL Database: tworzenie kont użytkowników bazy danych SQL służących do uzyskiwania dostępu do bazy danych i zarządzania nią
> [!div class="op_single_selector"]
> * [Samouczek z wprowadzeniem](sql-database-get-started-security.md)
> * [Udzielanie dostępu](sql-database-manage-logins.md)
> 
> 

Z tego samouczka dowiesz się, jak przy użyciu programu SQL Server Management Studio (SSMS) wykonać następujące czynności:

* Logowanie się do usługi SQL Database za pomocą identyfikatora logowania podmiotu zabezpieczeń na poziomie serwera.
* Tworzenie konta użytkownika usługi SQL Database.
* Przyznawanie [uprawnień db_owner](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0) użytkownikowi usługi SQL Database.
* Nawiązywanie połączenia z bazą danych SQL przy użyciu konta użytkownika, które nie jest podmiotem zabezpieczeń na poziomie serwera.

[!INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

[!INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]

## <a name="next-steps"></a>Następne kroki
Po ukończeniu tego samouczka usługi SQL Database, utworzeniu konta użytkownika i przyznaniu mu uprawnień właściciela bazy danych możesz pogłębić wiedzę o [zabezpieczeniach usługi SQL Database](sql-database-manage-logins.md).




<!--HONumber=Nov16_HO2-->


