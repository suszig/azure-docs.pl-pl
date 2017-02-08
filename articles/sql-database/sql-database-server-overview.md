---
title: "Omówienie serwera logicznego usługi Azure SQL Database | Microsoft Docs"
description: "Ta strona zawiera zagadnienia i wytyczne dotyczące pracy z serwerami logicznymi usługi Azure SQL."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: servers
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 17e2830dceeaa313dd0fd7d406bf68a75b6f900e


---
# <a name="azure-sql-database-logical-servers"></a>Serwery logiczne usługi Azure SQL Database

W tym temacie przedstawiono zagadnienia i wytyczne dotyczące pracy z serwerami logicznymi usługi Azure SQL. Aby uzyskać informacje o bazach danych SQL Azure, zobacz temat [Bazy danych SQL](sql-database-overview.md).

## <a name="what-is-an-azure-sql-database-logical-server"></a>Co to jest serwer logiczny usługi Azure SQL Database?
Serwer logiczny usługi Azure SQL Database działa jako centralny punkt administracyjny dla wielu baz danych. W usłudze SQL Database serwer jest konstrukcją logiczną, inną niż wystąpienie programu SQL Server, z którym można zapoznać się w środowisku lokalnym. W szczególności usługa SQL Database nie udziela żadnych gwarancji dotyczących lokalizacji baz danych w odniesieniu do ich serwerów logicznych i nie uwidacznia dostępu ani żadnych funkcji na poziomie wystąpienia. Aby uzyskać więcej informacji o serwerach logicznych Azure SQL, zobacz [Serwery logiczne](sql-database-server-overview.md). 

Serwer logiczny bazy danych Azure:

- Jest tworzony w ramach subskrypcji platformy Azure, ale można go przenieść wraz z zawartymi w nim zasobami do innej subskrypcji
- Jest zasobem nadrzędnym dla baz danych, pul elastycznych i magazynów danych
- Udostępnia przestrzeń nazw dla baz danych, pul elastycznych i magazynów danych
- Jest kontenerem logicznym z silną semantyką okresu istnienia — usunięcie serwera usuwa zawarte w nim bazy danych, pule elastyczne i magazyny danych
- Uczestniczy w kontroli dostępu opartej na rolach (RBAC) na platformie Azure — bazy danych i pule elastyczne na serwerze dziedziczą uprawnienia dostępu z serwera
- Jest elementem wyższego rzędu tożsamości baz danych i pul elastycznych dla celów zarządzania zasobami platformy Azure (zobacz schemat adresu URL dla baz danych i pul)
- Rozmieszcza zasoby w regionie
- Udostępnia punkt końcowy połączenia dla dostępu do baz danych (<serverName>.database.windows.net)
- Zapewnia dostęp do metadanych dotyczących zawartych zasobów przy użyciu dynamicznych widoków zarządzania, łącząc się z główną bazą danych 
- Określa zakres dla zasad zarządzania, które są stosowane do jego baz danych: logowania, zapory, inspekcje, wykrywanie zagrożeń itd. 
- Jest ograniczony przez limit przydziału w ramach subskrypcji nadrzędnej (sześć serwerów na subskrypcję — [zobacz tutaj limity subskrypcji](../azure-subscription-service-limits.md))
- Określa zakres dla limitu przydziału bazy danych i limitu przydziału jednostek DTU dla zasobów, które zawiera (na przykład 45000 DTU w wersji V12)
- Stanowi zakres przechowywania wersji dla możliwości włączonych w zawartych zasobach (najnowszą wersją jest wersja V12)
- Logowania główne na poziomie serwera mogą zarządzać wszystkimi bazami danych na serwerze
- Może zawierać logowania podobne do tych w lokalnych wystąpieniach programu SQL Server, którym udzielono dostępu do co najmniej jednej bazy danych na serwerze, i może otrzymać ograniczone prawa administracyjne. Aby uzyskać więcej informacji, zobacz temat [Logowania](sql-database-manage-logins.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database-logical-server"></a>Jak połączyć się z serwerem logicznym usługi Azure SQL Database i uwierzytelnić się na nim?

- **Uwierzytelnianie i autoryzacja**: usługa Azure SQL Database obsługuje uwierzytelnianie SQL i uwierzytelnianie za pomocą usługi Azure Active Directory (z pewnymi ograniczeniami — zobacz temat [Connect to SQL Database with Azure Active Directory Authentication](sql-database-aad-authentication.md) (Łączenie z usługą SQL Database przy użyciu uwierzytelniania usługi Azure Active Directory), aby uzyskać informacje o uwierzytelnianiu). Z bazami danych usługi Azure SQL Database można łączyć się i uwierzytelniać w nich za pośrednictwem głównej bazy danych serwera lub bezpośrednio na poziomie bazy danych użytkownika. Aby uzyskać więcej informacji, zobacz [Zarządzanie bazami danych i logowaniami w usłudze Azure SQL Database](sql-database-manage-logins.md). Uwierzytelnianie systemu Windows nie jest obsługiwane. 
- **TDS**: usługa Microsoft Azure SQL Database obsługuje wersję 7.3 lub nowszą klienta protokołu strumienia danych tabelarycznych (TDS).
- **TCP/IP**: dozwolone są tylko połączenia TCP/IP.
- **Zapora usługi SQL Database**: aby chronić dane, zapora usługi SQL Database uniemożliwia dostęp do serwera bazy danych lub jego baz danych do momentu określenia komputerów, które mają uprawnienia dostępu. Zobacz temat [Zapory](sql-database-firewall-configure.md).

## <a name="what-collations-are-supported"></a>Jakie sortowania są obsługiwane?

Domyślnym sortowaniem baz danych używanym przez usługę Microsoft Azure SQL Database (w tym główną bazę danych) jest sortowanie **SQL_LATIN1_GENERAL_CP1_CI_AS**, gdzie **LATIN1_GENERAL** oznacza język angielski (Stany Zjednoczone), **CP1** oznacza stronę kodową 1252, **CI** oznacza uwzględnianie wielkości liter, a **AS** oznacza uwzględnianie akcentów. Nie zaleca się zmiany sortowania dla baz danych w wersji V12 po utworzeniu. Aby uzyskać więcej informacji na temat sortowań, zobacz instrukcję [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Jakie są wymagania dotyczące nazewnictwa obiektów bazy danych?

Nazwy wszystkich nowych obiektów muszą być zgodne z regułami programu SQL Server dotyczącymi identyfikatorów. Aby uzyskać więcej informacji, zobacz temat [Identyfikatory](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported"></a>Jakie funkcje są obsługiwane?

Informacje dotyczące obsługiwanych funkcji można znaleźć w temacie [Funkcje](sql-database-features.md). Zobacz też [Różnice w języku Transact-SQL usługi Azure SQL Database](sql-database-transact-sql-information.md), aby uzyskać więcej informacji na temat przyczyn braku obsługi niektórych typów funkcji.

## <a name="how-do-i-manage-a-logical-server"></a>Jak zarządzać serwerem logicznym?

Serwerami logicznymi usługi Azure SQL Database można zarządzać przy użyciu kilku metod:
- [Azure Portal](sql-database-manage-portal.md)
- [Program PowerShell](sql-database-manage-powershell.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje na temat usługi Azure SQL Database, zobacz [Co to jest SQL Database?](sql-database-technical-overview.md)
- Informacje dotyczące obsługiwanych funkcji można znaleźć w temacie [Funkcje](sql-database-features.md).
- Omówienie baz danych usługi Azure SQL Database znajduje się w temacie [Omówienie usługi SQL Database](sql-database-overview.md)
- Informacje o obsłudze i różnicach języka Transact-SQL można znaleźć w temacie [Różnice w języku Transact-SQL usługi Azure SQL Database](sql-database-transact-sql-information.md).
- Informacje o konkretnych limitach przydziału i ograniczeniach zasobów na podstawie określonej **warstwy usługi**. Omówienie warstw usługi można znaleźć w temacie [Warstwy usługi w usłudze SQL Database](sql-database-service-tiers.md).
- Aby zapoznać się z przeglądem zabezpieczeń, zobacz [przegląd zabezpieczeń usługi Azure SQL Database](sql-database-security-overview.md).
- Aby uzyskać informacje o dostępności sterowników i obsłudze usługi SQL Database, zobacz [biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md).




<!--HONumber=Dec16_HO4-->


