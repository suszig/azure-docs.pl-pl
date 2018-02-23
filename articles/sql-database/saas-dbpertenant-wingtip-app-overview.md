---
title: "Przykład wielodostępnych aplikacji bazy danych SQL Azure — Wingtip SaaS | Dokumentacja firmy Microsoft"
description: "Dowiedz się, korzystając z przykładowej aplikacji wielodostępnych, która używa usługi Azure SQL Database w przykładzie Wingtip SaaS"
keywords: "samouczek usługi sql database"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: 2871d2b1208013808958e8a5b0c62fce31af86ec
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="introduction-to-a-multi-tenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Wprowadzenie do aplikacji SaaS wielodostępne korzystającej z bazy danych na wzorzec dzierżawcy z bazy danych SQL

*Wingtip SaaS* jest aplikacja przykładowa aplikacja wielodostępnej. Aplikacja korzysta z bazy danych dla dzierżawy, wzorzec aplikacji SaaS, aby obsłużyć wielu dzierżawców. Funkcje bazy danych SQL Azure, które umożliwiają scenariuszy SaaS, przy użyciu kilku wzorce projektowania i zarządzania SaaS aplikacji, których są wyświetlane. Aby szybko rozpocząć pracę, aplikacja Wingtip SaaS wdraża w mniej niż pięć minut!

Skrypty zarządzania i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repozytorium GitHub. Przed rozpoczęciem zapoznaj się [ogólne wskazówki](saas-tenancy-wingtip-app-guidance-tips.md) dla czynności, aby pobrać i odblokować skrypty zarządzania Wingtip biletów.

## <a name="application-architecture"></a>Architektura aplikacji

Aplikacja Wingtip SaaS korzysta z modelu bazy danych na dzierżawy i korzysta z puli elastycznej SQL Aby zmaksymalizować wydajność. Dla dzierżawcy mapowania do swoich danych i udostępniania bazy danych katalogu jest używany. Podstawowe aplikacji Wingtip SaaS używa puli z trzech dzierżawcami próbki oraz baza danych katalogu. Kończenie wiele SaaS Wingtip samouczki spowodować dodatki do momentu pierwszego wdrożenia, wprowadzając baz danych analitycznych, między bazami danych Zarządzanie schematami itp.


![Architektura SaaS Wingtip](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Podczas przechodzenia przez samouczków i Praca z aplikacją, należy skoncentrować się na wzorce SaaS w odniesieniu do warstwy danych. Innymi słowy, należy zwrócić szczególną uwagę na warstwę danych i nie skupiać się nadmiernie na samej aplikacji. Opis wykonania tych SaaS wzorców jest kluczem do wykonania tych wzorców w aplikacjach, rozważając wszelkie potrzebne modyfikacje dla konkretnych potrzeb biznesowych.

## <a name="sql-database-wingtip-saas-tutorials"></a>Samouczki SaaS Wingtip bazy danych SQL

Po wdrożeniu aplikacji, Poznaj następujące samouczki, które zależą od momentu pierwszego wdrożenia. Te samouczki Poznaj typowe wzorce SaaS, które korzystają z wbudowanych funkcji bazy danych SQL, SQL Data Warehouse i innymi usługami Azure. Samouczki zawierają skryptów programu PowerShell z szczegółowe wyjaśnienia, które znacznie upraszcza opis i wdrażanie tego samego wzorce zarządzania SaaS w aplikacji.


| Samouczek | Opis |
|:--|:--|
| [Wskazówki i porady dotyczące przykład aplikacji SaaS wielodostępne bazy danych SQL Azure](saas-tenancy-wingtip-app-guidance-tips.md) | **ZACZNIJ TUTAJ!** Pobierz i uruchom skrypty programu PowerShell, aby przygotować części aplikacji. |
|[Wdrażanie i Eksploruj aplikacji Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)|  Wdrażanie i Eksploruj aplikacji Wingtip SaaS do subskrypcji platformy Azure. |
|[Dostarczanie i katalogu dzierżawcy](saas-dbpertenant-provision-and-catalog.md)| Dowiedz się, jak aplikacja łączy się dzierżawcy przy użyciu bazy danych katalogu oraz jak katalogu mapuje dzierżawcy do swoich danych. |
|[Monitorowanie i zarządzanie nimi wydajności](saas-dbpertenant-performance-monitoring.md)| Dowiedz się, jak używać funkcji monitorowania bazy danych SQL i sposobu ustawiania alertów po przekroczeniu progów wydajności. |
|[Monitorowanie za pomocą analizy dzienników (OMS)](saas-dbpertenant-log-analytics.md) | Informacje o używaniu [analizy dzienników](../log-analytics/log-analytics-overview.md) do monitorowania dużej ilości zasobów, między wiele pul. |
|[Przywracanie pojedynczej dzierżawy](saas-dbpertenant-restore-single-tenant.md)| Dowiedz się, jak przywracanie dzierżawy bazy danych do wcześniejszego punktu w czasie. Włącza się również kroki, aby przywrócić równoległych bazy danych w trybie online, pozostawiając istniejącej bazy danych dzierżawy. |
|[Zarządzanie dzierżawy schematu bazy danych](saas-tenancy-schema-management.md)| Dowiedz się, jak zaktualizować schemat i aktualizacji danych referencyjnych dla wszystkich baz danych dzierżawy. |
|[Uruchamianie zapytań rozproszonych między dzierżawy](saas-tenancy-cross-tenant-reporting.md) | Tworzenie bazy danych analizy ad hoc i uruchamianie zapytań rozproszonych w czasie rzeczywistym we wszystkich dzierżawców.  |
|[Uruchom analizy danych wyodrębnionych dzierżawy](saas-tenancy-tenant-analytics.md) | Wyodrębnianie danych dzierżawy do analityka bazy danych lub dane magazynu dla zapytania analityczne w trybie offline. |


## <a name="next-steps"></a>Kolejne kroki

- [Ogólne wskazówki i porady dotyczące wdrażania i przy użyciu przykład aplikacji SaaS biletów Wingtip](saas-tenancy-wingtip-app-guidance-tips.md)

- [Wdrażanie aplikacji Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)
