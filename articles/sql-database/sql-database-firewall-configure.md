---
title: "Omówienie reguł zapory usługi SQL Database | Microsoft Docs"
description: "Informacje o sposobie konfigurowania zapory bazy danych SQL do zarządzania dostępem za pomocą reguł zapory na poziomie serwera i bazy danych."
keywords: zapora bazy danych
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/23/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: ae1cacf0ff003e69a16d6beac48abc36a7f18896


---
# <a name="overview-of-azure-sql-database-firewall-rules"></a>Omówienie reguł zapory usługi Azure SQL Database 
> [!div class="op_single_selector"]
> * [Omówienie](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [Interfejs API REST](sql-database-configure-firewall-settings-rest.md)
> 
> 

Usługa Microsoft Azure SQL Database udostępnia usługę relacyjnej bazy danych dla platformy Azure i innych aplikacji internetowych. Aby chronić dane, zapora uniemożliwia wszelki dostęp do serwera bazy danych do momentu określenia komputerów, które mają uprawnienia. Zapora udziela dostępu do bazy danych na podstawie źródłowego adresu IP każdego żądania.

Aby skonfigurować zaporę, należy utworzyć reguły zapory określające zakresy dopuszczalnych adresów IP. Można utworzyć reguły zapory na poziomach serwera i bazy danych.

* **Reguły zapory na poziomie serwera:** te reguły umożliwiają klientom dostęp do całego serwera Azure SQL, to znaczy do wszystkich baz danych na tym samym serwerze logicznym. Reguły te są przechowywane w **głównej** bazie danych. Reguły zapory na poziomie serwera można skonfigurować za pomocą portalu lub za pomocą instrukcji języka Transact-SQL. Aby utworzyć reguły zapory na poziomie serwera przy użyciu witryny Azure Portal lub programu PowerShell, musisz być właścicielem bądź współautorem subskrypcji. Aby utworzyć regułę zapory na poziomie serwera przy użyciu języka Transact-SQL, musisz połączyć się z wystąpieniem usługi SQL Database za pomocą identyfikatora logowania podmiotu zabezpieczeń na poziomie serwera lub jako administrator usługi Azure Active Directory (co oznacza, że reguła zapory na poziomie serwera musi zostać pierwotnie utworzona przez użytkownika mającego uprawnienia na poziomie platformy Azure).
* **Reguły zapory na poziomie bazy danych:** reguły te umożliwiają klientom dostęp do poszczególnych baz danych w ramach serwera usługi Azure SQL Database. Te reguły można utworzyć dla każdej bazy danych i są one przechowywane w poszczególnych bazach danych. (Można utworzyć reguły zapory na poziomie bazy danych dla **głównej** bazy danych). Reguły te mogą być pomocne w ograniczaniu dostępu do niektórych (bezpiecznych) baz danych na tym samym serwerze logicznym. Reguły zapory na poziomie bazy danych można skonfigurować tylko za pomocą instrukcji języka Transact-SQL.

**Zalecenie:** firma Microsoft zaleca korzystanie z reguł zapory na poziomie bazy danych zawsze wtedy, gdy jest to możliwe, aby zwiększyć poziom bezpieczeństwa i uczynić bazę danych bardziej przenośną. Reguły zapory na poziomie serwera powinny być używane dla administratorów i w przypadku, gdy wiele baz danych ma takie same wymagania dotyczące dostępu, a użytkownik nie chce poświęcać czasu na oddzielne konfigurowanie każdej bazy danych.

> [!Note]
> Aby uzyskać informacje o przenośnych bazach danych w kontekście ciągłości działalności biznesowej, zobacz [Wymagania dotyczące uwierzytelniania dla odzyskiwania po awarii](sql-database-geo-replication-security-config.md).
>

## <a name="firewall-overview"></a>Omówienie zapory
Początkowo cały dostęp języka Transact-SQL do serwera Azure SQL jest blokowany przez zaporę. Aby rozpocząć korzystanie z serwera Azure SQL, należy przejść do witryny Azure Portal i określić reguły zapory na poziomie serwera, które umożliwią dostęp do serwera Azure SQL. Użyj reguł zapory do określenia zakresu dozwolonych adresów IP pochodzących z Internetu oraz możliwości podejmowania przez aplikacje platformy Azure prób połączenia się z serwerem Azure SQL.

Aby selektywnie udzielić dostępu do tylko jednej z baz danych na serwerze Azure SQL, należy utworzyć regułę na poziomie bazy danych dla wymaganej bazy danych. Określ zakres adresów IP dla reguły zapory bazy danych, który wykracza poza zakres adresów IP określony w regule zapory na poziomie serwera, i upewnij się, że adres IP klienta znajduje się w zakresie określonym w regule na poziomie bazy danych.

Próby połączenia z Internetu i platformy Azure muszą najpierw przejść przez zaporę, zanim dotrą do serwera Azure SQL lub usługi SQL Database, jak pokazano na poniższym diagramie:

   ![Diagram opisujący konfigurację zapory.][1]

## <a name="connecting-from-the-internet"></a>Łączenie się z Internetu
Gdy komputer próbuje połączyć się w Internecie z serwerem bazy danych, zapora najpierw sprawdza źródłowy adres IP żądania i porównuje go z pełnym zestawem reguł zapory:

* Jeśli adres IP żądania należy do jednego z zakresów określonych w regułach zapory na poziomie serwera, zostanie ustanowione połączenie z serwerem usługi Azure SQL Database.
* Jeśli adres IP żądania nie należy do jednego z zakresów określonych w regułach zapory na poziomie serwera, sprawdzane są reguły zapory na poziomie bazy danych. Jeśli adres IP żądania należy do jednego z zakresów określonych w regułach zapory na poziomie bazy danych, zostanie ustanowione połączenie tylko z bazą danych, która ma pasujące reguły zapory.
* Jeśli adres IP żądania nie należy do żadnego z zakresów określonych w regułach zapory na poziomie serwera lub na poziomie bazy danych, żądanie połączenia zakończy się niepowodzeniem.

> [!NOTE]
> Aby uzyskać dostęp do usługi Azure SQL Database z komputera lokalnego, upewnij się, że zapora w sieci i na komputerze lokalnym zezwala na komunikację wychodzącą na porcie TCP 1433.
> 

## <a name="connecting-from-azure"></a>Łączenie z platformy Azure
Aby umożliwić aplikacjom z platformy Azure łączenie się z serwerem Azure SQL, należy włączyć połączenia platformy Azure. Gdy aplikacja platformy Azure próbuje połączyć się z serwerem bazy danych, zapora sprawdza, czy połączenia platformy Azure są dozwolone. Ustawienie zapory z początkowym i końcowym adresem równym 0.0.0.0 wskazuje, że te połączenia są dozwolone. Jeśli próba połączenia nie jest dozwolona, żądanie nie dociera do serwera usługi Azure SQL Database.

> [!IMPORTANT]
> Ta opcja konfiguruje zaporę w celu zezwalania na wszystkie połączenia z platformy Azure, w tym połączenia z subskrypcji innych klientów. W przypadku wybrania tej opcji upewnij się, że uprawnienia logowania i użytkownika zezwalają na dostęp tylko uprawnionym użytkownikom.
> 

> [!NOTE]
>  Aby uzyskać więcej informacji, zobacz sekcję **SQL Database: Outside vs inside** (SQL Database: na zewnątrz i wewnątrz) tematu [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md) (Porty inne niż 1433 dla ADO.NET 4.5 i usługi SQL Database)
>  

## <a name="creating-the-first-server-level-firewall-rule"></a>Tworzenie pierwszej reguły zapory na poziomie serwera
Pierwsze ustawienie zapory na poziomie serwera można utworzyć za pomocą [witryny Azure Portal](https://portal.azure.com/) lub programowo przy użyciu interfejsu API REST lub programu Azure PowerShell. Kolejne reguły zapory na poziomie serwera mogą być tworzone i zarządzane przy użyciu tych metod oraz za pomocą języka Transact-SQL. Aby poprawić wydajność, reguły zapory na poziomie serwera są tymczasowo przechowywane w pamięci podręcznej na poziomie bazy danych. Aby odświeżyć pamięć podręczną, zobacz [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Aby uzyskać więcej informacji dotyczących reguł zapory na poziomie serwera, zobacz [Instrukcje: konfigurowanie zapory serwera Azure SQL za pomocą witryny Azure Portal](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Tworzenie reguł zapory na poziomie bazy danych
Po skonfigurowaniu pierwszej zapory na poziomie serwera można ograniczyć dostęp do niektórych baz danych. Jeśli zakres adresów IP określony w regule zapory na poziomie bazy danych znajduje się poza zakresem określonym w regule zapory na poziomie serwera, dostęp do bazy danych mogą uzyskać tylko ci klienci, którzy mają adresy IP z zakresu ustalonego na poziomie bazy danych. Dla bazy danych można określić maksymalnie 128 reguł zapory na poziomie bazy danych. Reguły zapory na poziomie bazy danych dla głównej bazy danych i baz danych użytkownika można tworzyć i zarządzać nimi za pomocą języka Transact-SQL. Aby uzyskać więcej informacji na temat konfigurowania reguł zapory na poziomie bazy danych, zobacz [sp_set_database_firewall_rule (Azure SQL Database)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Programowe zarządzanie regułami zapory
Oprócz użycia witryny Azure Portal regułami zapory można zarządzać programowo przy użyciu języka Transact-SQL, interfejsu API REST i programu Azure PowerShell. W poniższych tabelach opisano zestaw poleceń dostępnych dla każdej metody.

### <a name="transact-sql"></a>Język Transact-SQL
| Widok wykazu lub procedura składowana | Poziom | Opis |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Serwer |Wyświetla bieżące reguły zapory na poziomie serwera |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Serwer |Tworzy lub aktualizuje reguły zapory na poziomie serwera |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Serwer |Usuwa reguły zapory na poziomie serwera |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database (Baza danych) |Wyświetla bieżące reguły zapory na poziomie bazy danych |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database (Baza danych) |Tworzy lub aktualizuje reguły zapory na poziomie bazy danych |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Bazy danych |Usuwa reguły zapory na poziomie bazy danych |

### <a name="rest-api"></a>Interfejs API REST
| Interfejs API | Poziom | Opis |
| --- | --- | --- |
| [List Firewall Rules](https://msdn.microsoft.com/library/azure/dn505715.aspx) (Lista reguł zapory) |Serwer |Wyświetla bieżące reguły zapory na poziomie serwera |
| [Create Firewall Rule](https://msdn.microsoft.com/library/azure/dn505712.aspx) (Tworzenie reguły zapory) |Serwer |Tworzy lub aktualizuje reguły zapory na poziomie serwera |
| [Set Firewall Rule](https://msdn.microsoft.com/library/azure/dn505707.aspx) (Ustawianie reguły zapory) |Serwer |Aktualizuje właściwości istniejącej reguły zapory na poziomie serwera |
| [Delete Firewall Rule](https://msdn.microsoft.com/library/azure/dn505706.aspx) (Usuwanie reguły zapory) |Serwer |Usuwa reguły zapory na poziomie serwera |

### <a name="azure-powershell"></a>Azure PowerShell
| Polecenie cmdlet | Poziom | Opis |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Serwer |Zwraca bieżące reguły zapory na poziomie serwera |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Serwer |Tworzy nową regułę zapory na poziomie serwera |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Serwer |Aktualizuje właściwości istniejącej reguły zapory na poziomie serwera |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Serwer |Usuwa reguły zapory na poziomie serwera |

> [!NOTE]
> Może wystąpić do pięciu minut opóźnienia, zanim zmiany w ustawieniach zapory zaczną obowiązywać.
> 
> 

## <a name="troubleshooting-the-database-firewall"></a>Rozwiązywanie problemów z zaporą bazy danych
Jeśli dostęp do usługi Microsoft Azure SQL Database nie działa zgodnie z oczekiwaniami, należy rozważyć następujące kwestie:

* **Konfiguracja lokalnej zapory:** aby komputer mógł uzyskać dostęp do usługi Azure SQL Database, może być konieczne utworzenie wyjątku zapory na komputerze dla portu TCP 1433. W przypadku nawiązywania połączeń wewnątrz granic chmury Azure może być konieczne otwarcie dodatkowych portów. Aby uzyskać więcej informacji, zobacz sekcję **V12 of SQL Database: Outside vs inside** (SQL Database V12: na zewnątrz i wewnątrz) tematu [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Porty inne niż 1433 dla ADO.NET 4.5 i usługi SQL Database V12).
* **Translator adresów sieciowych (NAT):** z powodu translatora adresów sieciowych (NAT), adres IP używany przez komputer do połączenia z usługą Azure SQL Database może być inny niż adres IP wyświetlany w ustawieniach konfiguracji adresu IP komputera. Aby wyświetlić adres IP używany przez komputer do łączenia się z platformą Azure, zaloguj się do portalu i przejdź na kartę **Konfigurowanie** na serwerze, który hostuje bazę danych. W sekcji **Dozwolone adresy IP** wyświetlany jest **bieżący adres IP klienta**. Kliknij przycisk **Dodaj** do listy **Dozwolone adresy IP**, aby zezwolić temu komputerowi na dostęp do serwera.
* **Zmiany na liście dozwolonych jeszcze nie zaczęły obowiązywać:** może wystąpić do pięciu minut opóźnienia, zanim zmiany konfiguracji zapory usługi Azure SQL Database zostaną zastosowane.
* **Logowanie nie ma autoryzacji lub użyto nieprawidłowego hasła:** jeśli logowanie nie ma uprawnień na serwerze usługi Azure SQL Database lub użyte hasło jest nieprawidłowe, nastąpi odmowa połączenia z serwerem usługi Azure SQL Database. Utworzenie ustawień zapory zapewnia klientom jedynie możliwość próby nawiązania połączenia z serwerem, ale każdy klient musi podać niezbędne poświadczenia zabezpieczeń. Aby uzyskać więcej informacji na temat przygotowywania logowań, zobacz sekcję Zarządzanie bazami danych, logowaniami i użytkownikami w usłudze Azure SQL Database.
* **Dynamiczny adres IP:** jeśli używane jest połączenie internetowe za pomocą dynamicznego adresowania IP i występują problemy z przejściem przez zaporę, można wypróbować jedno z poniższych rozwiązań:
  
  * Poproś usługodawcę internetowego (ISP) o zakres adresów IP przypisany do komputerów klienckich uzyskujących dostęp do serwera usługi Azure SQL Database, a następnie dodaj ten zakres adresów IP jako regułę zapory.
  * Pobierz statyczne adresy IP dla komputerów klienckich, a następnie dodaj te adresy IP jako reguły zapory.

## <a name="next-steps"></a>Następne kroki
Aby przejrzeć artykuły zawierające wskazówki na temat tworzenia reguł zapory na poziomie serwera i na poziomie bazy danych, zobacz:

* [Configure Azure SQL Database server-level firewall rules using the Azure portal](sql-database-configure-firewall-settings.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database za pomocą witryny Azure Portal)
* [Configure Azure SQL Database server-level and database-level firewall rules using T-SQL](sql-database-configure-firewall-settings-tsql.md) (Konfigurowanie reguł zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database z użyciem języka T-SQL)
* [Configure Azure SQL Database server-level firewall rules using PowerShell](sql-database-configure-firewall-settings-powershell.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu programu PowerShell)
* [Configure Azure SQL Database server-level firewall rules using the REST API](sql-database-configure-firewall-settings-rest.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu interfejsu API REST)

Aby uzyskać samouczek tworzenia bazy danych, zobacz [Create a SQL database in minutes using the Azure portal](sql-database-get-started.md) (Tworzenie bazy danych SQL w ciągu kilku minut za pomocą witryny Azure Portal).
Aby uzyskać pomoc podczas łączenia się z bazą danych Azure SQL z aplikacji innych firm lub aplikacji typu open source, zobacz [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Przykłady kodu umożliwiające szybki start dla klienta usługi SQL Database).
Aby dowiedzieć się, jak przechodzić do baz danych, zobacz artykuł [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Zarządzanie zabezpieczeniami dostępu i logowania do bazy danych).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Zabezpieczanie bazy danych](sql-database-security-overview.md)
* [Usługa Security Center aparatu bazy danych programu SQL Server i bazy danych Azure SQL Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png



<!--HONumber=Dec16_HO4-->


