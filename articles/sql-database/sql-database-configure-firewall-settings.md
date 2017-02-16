---
title: "Azure Portal: reguły zapory na poziomie serwera usługi Azure SQL Database | Microsoft Docs"
description: "Informacje o konfigurowaniu reguł zapory na poziomie serwera dla adresów IP uzyskujących dostęp do serwera Azure SQL przy użyciu witryny Azure Portal."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: c3b206b5-af6e-41af-8306-db12ecfc1b5d
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 11/28/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2a85b3dc1078bad9e5e2fc0ce0bec7e994b29150
ms.openlocfilehash: 36938e642853d587cd28405f5e518ac9ac0697ac


---
# <a name="create-and-manage-azure-sql-database-server-level-firewall-rules-using-the-azure-portal"></a>Tworzenie reguł zapory na poziomie serwera usługi Azure SQL Database i zarządzanie nimi za pomocą witryny Azure Portal
> [!div class="op_single_selector"]
> * [Omówienie](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [Interfejs API REST](sql-database-configure-firewall-settings-rest.md)
> 

Reguły zapory na poziomie serwera umożliwiają administratorom uzyskiwanie dostępu do serwera usługi SQL Database z określonego adresu IP lub zakresu adresów IP. Reguły zapory na poziomie serwera mogą być również używane dla użytkowników w sytuacji, gdy wiele baz danych ma takie same wymagania dotyczące dostępu i nie chcesz poświęcać czasu na oddzielne konfigurowanie każdej bazy danych. Firma Microsoft zaleca korzystanie z reguł zapory na poziomie bazy danych zawsze wtedy, gdy jest to możliwe, aby zwiększyć poziom bezpieczeństwa i możliwość przenoszenia bazy danych. Omówienie zapór usługi SQL Database można znaleźć na stronie [Omówienie reguł zapory usługi SQL Database](sql-database-firewall-configure.md).

> [!Note]
> Aby uzyskać informacje o przenośnych bazach danych w kontekście ciągłości działalności biznesowej, zobacz [Wymagania dotyczące uwierzytelniania dla odzyskiwania po awarii](sql-database-geo-replication-security-config.md).
>

[!INCLUDE [Create SQL Database firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Zarządzanie istniejącymi regułami zapory na poziomie serwera przy użyciu witryny Azure Portal
Powtórz kroki, aby zarządzać regułami zapory na poziomie serwera.

* Aby dodać bieżący komputer, kliknij pozycję Dodaj adres IP klienta.
* Aby dodać kolejne adresy IP, wpisz nazwę reguły, początkowy adres IP i końcowy adres IP.
* Aby zmodyfikować istniejącą regułę, kliknij dowolne pole w regule i wprowadź zmiany.
* Aby usunąć istniejącą regułę, umieść kursor na regule do czasu wyświetlenia znaku X na końcu wiersza. Kliknij przycisk X, aby usunąć regułę.

Kliknij przycisk **Zapisz**, aby zapisać zmiany.

## <a name="next-steps"></a>Następne kroki

- Aby zapoznać się z samouczkiem z wprowadzeniem, zobacz [Samouczek usługi SQL Database: tworzenie serwera, reguły zapory na poziomie serwera, przykładowej bazy danych i reguły zapory na poziomie bazy danych oraz nawiązywanie połączenia z programem SQL Server](sql-database-get-started.md).
- Aby zapoznać się z samouczkiem z wprowadzeniem do zabezpieczeń, zobacz [Wprowadzenie do zabezpieczeń](sql-database-control-access-sql-authentication-get-started.md)
- Aby uzyskać pomoc podczas łączenia się z bazą danych Azure SQL z aplikacji innych firm lub aplikacji typu open source, zobacz [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Przykłady kodu umożliwiające szybki start dla klienta usługi SQL Database).
- Aby dowiedzieć się, jak utworzyć dodatkowych użytkowników mogących łączyć się z bazami danych, zobacz [SQL Database Authentication and Authorization: Granting Access](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Uwierzytelnianie i autoryzacja w usłudze SQL Database: udzielanie dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Zabezpieczanie bazy danych](sql-database-security-overview.md)   
* [Usługa Security Center aparatu bazy danych programu SQL Server i bazy danych Azure SQL Database](https://msdn.microsoft.com/library/bb510589)   






<!--HONumber=Jan17_HO3-->


