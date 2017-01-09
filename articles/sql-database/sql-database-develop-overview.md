---
title: "Omówienie tworzenia aplikacji korzystających z usługi SQL Database | Microsoft Docs"
description: "Informacje o dostępnych bibliotekach łączności i najlepsze praktyki dotyczące aplikacji łączących się z usługą SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 18dc3cce7451d90b6b65b990b80c05e7f6decb56


---
# <a name="sql-database-application-development-overview"></a>Omówienie tworzenia aplikacji korzystających z usługi SQL Database
W tym artykule przedstawiono podstawowe zagadnienia, jakie powinien mieć na uwadze programista podczas pisania kodu nawiązującego połączenie z usługą Azure SQL Database.

> [!TIP]
> Aby uzyskać samouczek omawiający tworzenie serwera i opartej na serwerze zapory, wyświetlanie właściwości serwera, nawiązywanie połączenia przy użyciu programu SQL Server Management Studio, odpytywanie bazy danych master, tworzenie przykładowej i pustej bazy danych, tworzenie zapytań o właściwości bazy danych, nawiązywanie połączenia przy użyciu programu SQL Server Management Studio, a także odpytywanie przykładowej bazy danych, zobacz [Samouczek z wprowadzeniem](sql-database-get-started.md).
>

## <a name="language-and-platform"></a>Język i platforma
Dostępne są przykłady kodu dla różnych języków programowania i platform programistycznych. Linki do przykładów kodu można znaleźć tutaj: 

* Więcej informacji: [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md) (Biblioteki połączeń dla usługi SQL Database i programu SQL Server)

## <a name="resource-limitations"></a>Ograniczenia zasobów
Usługa Azure SQL Database zarządza zasobami dostępnymi dla bazy danych przy użyciu dwóch różnych mechanizmów: nadzoru nad zasobami i wymuszania limitów.

* Więcej informacji: [Azure SQL Database resource limits](sql-database-resource-limits.md) (Limity zasobów usługi Azure SQL Database)

## <a name="security"></a>Bezpieczeństwo
Usługa Azure SQL Database udostępnia zasoby na potrzeby ograniczania dostępu, ochrony danych i monitorowania działań w bazie danych SQL Database.

* Więcej informacji: [Securing your SQL Database](sql-database-security-overview.md) (Zabezpieczanie bazy danych SQL Database)

## <a name="authentication"></a>Authentication
* Usługa Azure SQL Database obsługuje użytkowników i logowania korzystające zarówno z uwierzytelniania programu SQL Server, jak i z [uwierzytelniania za pomocą usługi Azure Active Directory](sql-database-aad-authentication.md).
* Należy określić konkretną bazy danych, zamiast używać domyślnej bazy danych *master*.
* W usłudze SQL Database nie można używać instrukcji **USE mojaNazwaBazyDanych;** języka Transact-SQL w celu przełączenia się na inną bazę danych.
* Więcej informacji: [SQL Database security: Manage database access and login security](sql-database-manage-logins.md) (Zabezpieczenia usługi SQL Database: zarządzanie zabezpieczeniami dostępu i logowania do bazy danych)

## <a name="resiliency"></a>Odporność
W przypadku wystąpienia przejściowego błędu podczas łączenia się z usługą SQL Database kod powinien ponowić wywołanie.  Zalecamy, aby logika ponawiania korzystała z logiki wycofywania, co pozwoli uniknąć przeciążenia usługi SQL Database z powodu jednoczesnych ponownych prób ze strony wielu klientów.

* Przykłady kodu: aby uzyskać przykłady kodu ilustrujące logikę ponawiania, zapoznaj się z przykładami dla wybranego języka w temacie [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md) (Biblioteki połączeń dla usługi SQL Database i programu SQL Server)
* Więcej informacji: [Komunikaty o błędach dotyczących programów klienckich usługi SQL Database](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Zarządzanie połączeniami
* W logice połączenia klienta zastąp domyślny limit czasu wartością 30 sekund.  Domyślna wartość 15 sekund jest zbyt mała w przypadku połączeń zależnych od Internetu.
* Jeśli korzystasz z [puli połączeń](http://msdn.microsoft.com/library/8xx3tyca.aspx), pamiętaj o zamknięciu połączenia, gdy tylko Twój program nie korzysta z niego aktywnie i nie przygotowuje się do jego ponownego użycia.

## <a name="network-considerations"></a>Zagadnienia dotyczące sieci
* Upewnij się, że zapora na komputerze hostującym program kliencki zezwala na wychodzącą komunikację TCP na porcie 1433.  Więcej informacji: [Konfigurowanie zapory usługi Azure SQL Database](sql-database-configure-firewall-settings.md)
* Jeśli podczas łączenia się z usługą SQL Database V12 program kliencki jest uruchomiony na maszynie wirtualnej platformy Azure, musisz otworzyć określone zakresy portów na tej maszynie. Więcej informacji: [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Porty inne niż 1433 dla platformy ADO.NET 4.5 i usługi SQL Database 12)
* Połączenia klienta z usługą Azure SQL Database V12 czasami pomijają serwer proxy i wchodzą w interakcję bezpośrednio z bazą danych. Porty inne niż 1433 nabierają znaczenia. Więcej informacji: [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Porty inne niż 1433 dla platformy ADO.NET 4.5 i usługi SQL Database 12)

## <a name="data-sharding-with-elastic-scale"></a>Dzielenie danych na fragmenty za pomocą elastycznego skalowania
Elastyczne skalowanie upraszcza proces skalowania do wewnątrz (i na zewnątrz). 

* [Wzorce projektowe dla wielodostępnych aplikacji SaaS wykorzystujących usługę Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Routing zależny od danych](sql-database-elastic-scale-data-dependent-routing.md)
* [Wprowadzenie do funkcji Elastyczne skalowanie w wersji zapoznawczej dla usługi Azure SQL Database](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Następne kroki
Poznaj wszystkie [możliwości usługi SQL Database](https://azure.microsoft.com/services/sql-database/).




<!--HONumber=Dec16_HO4-->


