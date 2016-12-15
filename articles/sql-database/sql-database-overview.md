---
title: "Omówienie usługi Azure SQL Database | Microsoft Docs"
description: "Ta strona zawiera omówienie baz danych usługi Azure SQL Database."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: d772dabf84fc3b6c061c0f4607c989aabf9dd272
ms.openlocfilehash: 50200648bcd2aed4cbc0c720d140f2ecb2c17934


---
# <a name="azure-sql-database-overview"></a>Omówienie usługi Azure SQL Database
Ten temat zawiera omówienie baz danych usługi Azure SQL Database. Aby uzyskać więcej informacji o serwerach logicznych Azure SQL, zobacz temat [Serwery logiczne](sql-database-server-overview.md).

## <a name="what-is-azure-sql-database"></a>Co to jest baza danych usługi Azure SQL Database?
Każda baza danych w usłudze Azure SQL Database jest skojarzona z serwerem logicznym. Baza danych może być:

- pojedynczą bazą danych z [własnym zestawem zasobów](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (jednostki DTU).
- częścią [puli baz danych](sql-database-elastic-pool.md), która [współużytkuje zestaw zasobów](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (jednostki eDTU).
- częścią [skalowanego zestawu baz danych podzielonych na fragmenty](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), które mogą być pojedynczymi bazami danych lub pulami baz danych.
- częścią zestawu baz danych uczestniczącego we [wzorcu projektowym wielodostępnych aplikacji SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md), którego bazy danych mogą być pojedynczymi bazami danych i/lub pulami baz danych. 

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database"></a>Jak połączyć się z bazą danych usługi Azure SQL Database i uwierzytelnić się w niej?

- **Uwierzytelnianie i autoryzacja**: usługa Azure SQL Database obsługuje uwierzytelnianie SQL i uwierzytelnianie za pomocą usługi Azure Active Directory (z pewnymi ograniczeniami — zobacz temat [Connect to SQL Database with Azure Active Directory Authentication](sql-database-aad-authentication.md) (Łączenie z usługą SQL Database przy użyciu uwierzytelniania usługi Azure Active Directory), aby uzyskać informacje o uwierzytelnianiu). Z bazami danych usługi Azure SQL Database można łączyć się i uwierzytelniać w nich za pośrednictwem głównej bazy danych serwera lub bezpośrednio na poziomie bazy danych użytkownika. Aby uzyskać więcej informacji, zobacz [Zarządzanie bazami danych i logowaniami w usłudze Azure SQL Database](sql-database-manage-logins.md). Uwierzytelnianie systemu Windows nie jest obsługiwane. 
- **TDS**: usługa Microsoft Azure SQL Database obsługuje wersję 7.3 lub nowszą klienta protokołu strumienia danych tabelarycznych (TDS).
- **TCP/IP**: dozwolone są tylko połączenia TCP/IP.
- **Zapora usługi SQL Database**: aby chronić dane, zapora usługi SQL Database uniemożliwia dostęp do serwera bazy danych lub jego baz danych do momentu określenia komputerów, które mają uprawnienia dostępu. Zobacz temat [Zapory](sql-database-firewall-configure.md).

## <a name="what-collations-are-supported"></a>Jakie sortowania są obsługiwane?
Domyślnym sortowaniem baz danych używanym przez usługę Microsoft Azure SQL Database jest sortowanie **SQL_LATIN1_GENERAL_CP1_CI_AS**, gdzie **LATIN1_GENERAL** oznacza język angielski (Stany Zjednoczone), **CP1** oznacza stronę kodową 1252, **CI** oznacza uwzględnianie wielkości liter, a **AS** oznacza uwzględnianie akcentów. Nie można zmienić sortowania dla baz danych w wersji 12. Aby uzyskać więcej informacji na temat sposobu ustawiania sortowania, zobacz instrukcję [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Jakie są wymagania dotyczące nazewnictwa obiektów bazy danych?

Nazwy wszystkich nowych obiektów muszą być zgodne z regułami programu SQL Server dotyczącymi identyfikatorów. Aby uzyskać więcej informacji, zobacz temat [Identyfikatory](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported-by-azure-sql-databases"></a>Jakie funkcje są obsługiwane przez bazy danych usługi Azure SQL Database?

Informacje dotyczące obsługiwanych funkcji można znaleźć w temacie [Funkcje](sql-database-features.md). Zobacz też [Różnice w języku Transact-SQL usługi Azure SQL Database](sql-database-transact-sql-information.md), aby uzyskać więcej informacji na temat przyczyn braku obsługi niektórych typów funkcji.

## <a name="how-do-i-manage-an-azure-sql-database"></a>Jak zarządzać usługą Azure SQL Database?

Serwerami logicznymi usługi Azure SQL Database można zarządzać przy użyciu kilku metod:
- [Azure Portal](sql-database-manage-portal.md)
- [Program PowerShell](sql-database-manage-powershell.md)
- [Język Transact-SQL](sql-database-manage-azure-ssms.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje na temat usługi Azure SQL Database, zobacz [Co to jest SQL Database?](sql-database-technical-overview.md)
- Informacje dotyczące obsługiwanych funkcji można znaleźć w temacie [Funkcje](sql-database-features.md).
- Omówienie serwerów logicznych Azure SQL można znaleźć w temacie [Omówienie serwerów logicznych usługi SQL Database](sql-database-server-overview.md).
- Informacje o obsłudze i różnicach języka Transact-SQL można znaleźć w temacie [Różnice w języku Transact-SQL usługi Azure SQL Database](sql-database-transact-sql-information.md).
- Informacje o konkretnych limitach przydziału i ograniczeniach zasobów na podstawie określonej **warstwy usługi**. Omówienie warstw usługi można znaleźć w temacie [Warstwy usługi w usłudze SQL Database](sql-database-service-tiers.md).
- Aby uzyskać wytyczne dotyczące zabezpieczeń, zobacz [Wytyczne z zakresu bezpieczeństwa i ograniczenia dotyczące usługi Azure SQL Database](sql-database-security-guidelines.md).
- Aby uzyskać informacje o dostępności sterowników i obsłudze usługi SQL Database, zobacz [biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md).




<!--HONumber=Dec16_HO1-->


