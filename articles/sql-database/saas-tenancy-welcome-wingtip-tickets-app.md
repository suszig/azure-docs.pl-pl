---
title: "Witamy w aplikacji Wingtips — baza danych Azure SQL | Dokumentacja firmy Microsoft"
description: "Informacje o modelach dzierżawy bazy danych i o przykładowej aplikacji Wingtips SaaS bazy danych SQL Azure w środowisku chmury."
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: billgib
manager: craigg
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: billgib
ms.openlocfilehash: 2a36df0e45af5bcce5338d04b7e1ba44221ae964
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="the-wingtip-tickets-saas-application"></a>Aplikacja SaaS biletów Wingtip

Taki sam *biletów Wingtip* aplikacja jest wdrażana w każdej z trzech próbek. Aplikacja jest zdarzenie proste wyświetlania i tworzenia biletów aplikacji SaaS przeznaczonych dla małych miejsc - theaters klubów, itp. Każda właściwość jest dzierżawy aplikacji i ma własne dane: miejscową szczegóły listę zdarzeń, klientów, zamówienia biletu itp.  Aplikacja, wraz z samouczki i skrypty zarządzania ilustrację scenariusza SaaS end-to-end. W tym dzierżaw inicjowania obsługi administracyjnej, monitorowanie i zarządzanie wydajności, zarządzania schematu i dzierżawy między raportowania i analiz.

## <a name="three-saas-application-patterns"></a>Trzy wzorce aplikacji SaaS

Trzy wersje aplikacji są dostępne; Każdy Eksploruje wzorzec dzierżawy innej bazy danych w bazie danych SQL Azure.  Pierwszy używa aplikacji pojedynczej dzierżawy przy użyciu izolowanego bazy danych jednego dzierżawcy. W drugiej zastosowano aplikację o wielu dzierżawców z bazy danych dla każdego dzierżawcy. Trzeci przykładzie użyto aplikacji wielodostępnych podzielonej wielodostępnych baz danych.

![Trzy wzorce dzierżawy][image-three-tenancy-patterns]

 Każda próbka zawiera wzorce zarządzania, którego można użyć w swojej aplikacji i skrypty zarządzania i samouczki przedstawiających zakres projektu.  Każda próbka wdraża w czasie krótszym tego pięć minut.  Wszystkie trzy może być wdrożone side-by-side, więc można porównać w projektowania i zarządzania.

## <a name="standalone-application-pattern"></a>Wzorzec aplikacji autonomicznych

Wzorzec aplikacji autonomicznej używa aplikacji pojedynczej dzierżawy z pojedynczej dzierżawy bazy danych dla każdego dzierżawcy. Poszczególni dzierżawcy aplikacja jest wdrażana w grupie oddzielnych zasobów platformy Azure. To może być w subskrypcji przez dostawcę usług lub subskrypcji dzierżawcy i zarządzane przez dostawcę w imieniu dzierżawcy. Ten wzorzec zapewnia największą izolacji dzierżawców, ale jest zazwyczaj najbardziej kosztowne, ponieważ nie ma możliwość udostępniania zasobów między wieloma dzierżawcami.

Zapoznaj się z [samouczki] [ docs-tutorials-for-wingtip-sa] i kodu w witrynie GitHub [.../Microsoft/WingtipTicketsSaaS-StandaloneApp][github-code-for-wingtip-sa].

## <a name="database-per-tenant-pattern"></a>Bazy danych dla każdej dzierżawy wzorca

Bazy danych na wzorzec dzierżawy obowiązuje dla dostawców usług, które są związane z izolacji dzierżawców i chcesz uruchomić scentralizowanej usługi, która umożliwia oszczędne korzystanie z zasobów udostępnionych. Baza danych została utworzona dla każdego miejsca lub dzierżawy, a wszystkie bazy danych są zarządzane centralnie. Pule elastyczne, aby zapewnić ekonomiczne i zarządzanie łatwe wydajności, która wykorzystuje wzorców obciążenia nieprzewidywalne dzierżawcy mogą być hostowane bazy danych. Bazy danych katalogu posiada mapowanie między dzierżawców i ich baz danych. To mapowanie odbywa się przy użyciu niezależnego fragmentu mapy funkcji do zarządzania [elastycznej bazy danych klienta biblioteki](sql-database-elastic-database-client-library.md), który umożliwia zarządzanie wydajność połączenia do aplikacji.

Zapoznaj się z [samouczki] [ docs-tutorials-for-wingtip-dpt] i kodu w witrynie GitHub [.../Microsoft/WingtipTicketsSaaS-DbPerTenant][github-code-for-wingtip-dpt].

## <a name="sharded-multi-tenant-database-pattern"></a>Wzorzec podzielonej wielodostępne bazy danych.

Wielodostępne baz danych są uwzględniane dla usługodawców wyszukiwanie niższy koszt dzierżawy i OK izolacji mniejsze dzierżawy. Ten wzorzec umożliwia pakowania dużej liczby dzierżawców do pojedynczej bazy danych, obniżenie kosztów na dla dzierżawy. NEAR nieskończone skala jest możliwe przez podanie dzierżawcy między wieloma bazy danych.  Katalog bazy danych ponownie mapuje dzierżawcy do baz danych.  

Ten wzorzec umożliwia także modelu hybrydowych, w którym można zoptymalizować kosztu z wieloma dzierżawcami w bazie danych lub Optymalizuj dla izolacji z pojedynczej dzierżawy w ich własnych bazy danych. Wybór będzie możliwe na podstawie dzierżawy przez dzierżawy, albo gdy dzierżawa jest udostępnione lub nowszej, bez wpływu na aplikację.

Zapoznaj się z [samouczki] [ docs-tutorials-for-wingtip-mt] i kodu w witrynie GitHub [.../Microsoft/WingtipTicketsSaaS-MultiTenantDb][github-code-for-wingtip-mt].

## <a name="next-steps"></a>Następne kroki

#### <a name="conceptual-descriptions"></a>Opisy pojęć

- Bardziej szczegółowy opis wzorce dzierżawy aplikacji znajduje się w temacie [SaaS wielodostępne bazy danych wzorce dzierżawy][saas-tenancy-app-design-patterns-md]

#### <a name="tutorials-and-code"></a>Samouczki i kod

- Autonomiczny aplikacji:
    - [Samouczki dla aplikacji autonomicznych][docs-tutorials-for-wingtip-sa].
    - [Kod dla autonomicznego w serwisie GitHub][github-code-for-wingtip-sa].

- Bazy danych dla poszczególnych dzierżawców:
    - [Samouczki dotyczące bazy danych dla poszczególnych dzierżawców][docs-tutorials-for-wingtip-dpt].
    - [Kod dla bazy danych dla każdego dzierżawcy w serwisie GitHub][github-code-for-wingtip-dpt].

- Podzielonej wielodostępne:
    - [Samouczki dotyczące podzielonej wielodostępne][docs-tutorials-for-wingtip-mt].
    - [Kod dla podzielonej wielodostępne w serwisie GitHub][github-code-for-wingtip-mt].



<!-- Image references. -->

[image-three-tenancy-patterns]: media/saas-tenancy-welcome-wingtip-tickets-app/three-tenancy-patterns.png "Trzy wzorce dzierżawy."

<!-- Docs.ms.com references. -->

[saas-tenancy-app-design-patterns-md]: saas-tenancy-app-design-patterns.md

<!-- WWWeb http references. -->

[docs-tutorials-for-wingtip-sa]: https://aka.ms/wingtipticketssaas-sa
[github-code-for-wingtip-sa]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp

[docs-tutorials-for-wingtip-dpt]: https://aka.ms/wingtipticketssaas-dpt
[github-code-for-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant

[docs-tutorials-for-wingtip-mt]: https://aka.ms/wingtipticketssaas-mt
[github-code-for-wingtip-mt]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb

