---
title: "Konfigurowanie reguły zapory na poziomie serwera w usłudze SQL Database | Microsoft Docs"
description: "Informacje o konfigurowaniu zapory dla adresów IP uzyskujących dostęp do serwera Azure SQL."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: c3b206b5-af6e-41af-8306-db12ecfc1b5d
ms.service: sql-database
ms.custom: auth and access
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 11/28/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 867f06c1fae3715ab03ae4a3ff4ec381603e32f7
ms.openlocfilehash: 1479595fb7de2e0a37520c7d323624142e624af1


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
- Aby zapoznać się z samouczkiem z wprowadzeniem do zabezpieczeń, zobacz [Wprowadzenie do zabezpieczeń](sql-database-get-started-security.md)
- Aby uzyskać pomoc podczas łączenia się z bazą danych Azure SQL z aplikacji innych firm lub aplikacji typu open source, zobacz [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Przykłady kodu umożliwiające szybki start dla klienta usługi SQL Database).
- Aby dowiedzieć się, jak utworzyć dodatkowych użytkowników mogących łączyć się z bazami danych, zobacz [SQL Database Authentication and Authorization: Granting Access](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Uwierzytelnianie i autoryzacja w usłudze SQL Database: udzielanie dostępu).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Zabezpieczanie bazy danych](sql-database-security.md)   
* [Usługa Security Center aparatu bazy danych programu SQL Server i bazy danych Azure SQL Database](https://msdn.microsoft.com/library/bb510589)   






<!--HONumber=Dec16_HO1-->


