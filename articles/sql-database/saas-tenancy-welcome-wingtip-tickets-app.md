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
ms.date: 11/14/2017
ms.author: billgib;genemi
ms.openlocfilehash: 96e031835905057a9ab2b3ee4023b08de092dd8e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="welcome-to-the-wingtip-tickets-sample-saas-azure-sql-database-tenancy-app"></a>Bilety Wingtip przykładowej bazy danych SQL Azure SaaS dzierżawy aplikacji — Zapraszamy!

Witamy w aplikacji dzierżawy biletów Wingtip przykładowej bazy danych SQL Azure SaaS i jego samouczki. Dzierżawy bazy danych odwołuje się do trybu izolacji danych, w którym aplikacja zapewnia klientom, którzy płacą ma być obsługiwana w aplikacji. Za pośrednictwem uprościć w tej chwili, albo każdy klient ma całej bazy danych do samego siebie lub współużytkuje bazy danych z innego klienta.

## <a name="wingtip-tickets-app"></a>Wingtip biletów aplikacji

Przykładowa aplikacja biletów Wingtip ilustruje skutków modeli dzierżawy innej bazy danych w projekcie i zarządzania wielodostępnych aplikacji SaaS. Samouczki towarzyszący bezpośrednio opisano te same skutki. Bilety Wingtip jest oparty na bazie danych SQL Azure.

Bilety Wingtip przeznaczony do obsługi różnych scenariuszy projektowania i zarządzania, które są używane przez klientów SaaS rzeczywistego. Wzorce użycia, która wynika z nich są uwzględnione w Wingtip biletów.

Można zainstalować aplikację biletów Wingtip w subskrypcji platformy Azure w ciągu pięciu minut. Instalacja obejmuje Wstawianie dane przykładowe dla wielu dzierżawców. Aplikacji i skrypty zarządzania dla wszystkich modeli można zainstalować bezpiecznie ponieważ instalacje interakcji lub nie koliduje ze sobą.

#### <a name="code-in-github"></a>Kod w serwisie Github

Kod aplikacji i skrypty zarządzania są wszystkie dostępne w witrynie GitHub:

- **Aplikacją autonomiczną** modelu: [WingtipTicketsSaaS StandaloneApp repozytorium](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
- **Bazy danych dla poszczególnych dzierżawców** modelu: [repozytorium WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
- **Wielodostępne podzielonej** modelu: [repozytorium WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB).

Ten sam kod jednej podstawowej aplikacji biletów Wingtip zostanie ponownie użyty dla wszystkich powyższych modeli wymienionych. Do uruchomienia projektów SaaS, można użyć kodu z usługi Github.



## <a name="major-database-tenancy-models"></a>Modele dzierżawy głównej bazy danych

Bilety Wingtip jest zdarzeniem wyświetlania i tworzenia biletów aplikacji SaaS. Wingtip udostępnia usługi, które są wymagane przez miejsc. Poniższe elementy dotyczą każdego miejsce:

- Płaci użytkownik ma być obsługiwana w aplikacji.
- Jest *dzierżawy* w Wingtip.
- Zdarzenia hostów. Obejmuje następujące zdarzenia:
    - Ceny biletu.
    - Sprzedaży biletów.
    - Klienci, którzy kupić biletów.

Aplikacja, wraz z samouczki i skrypty zarządzania ilustrację całego scenariusza SaaS. Scenariusz obejmuje wykonywanie następujących czynności:

- Inicjowanie obsługi dzierżawców.
- Monitorowanie i zarządzanie wydajnością.
- Zarządzanie schematu.
- Dzierżawcy między raportowania i analiz.

Te działania są udostępniane w dowolnej skali jest wymagana.



## <a name="code-samples-for-each-tenancy-model"></a>Przykłady kodu dla każdego modelu dzierżawy

Wyróżniony są zestaw modeli aplikacji. Jednak inne implementacje można mieszać elementy co najmniej dwa modele.

#### <a name="standalone-app-model"></a>Model aplikacji autonomicznych

![Model aplikacji autonomicznych][standalone-app-model-62s]

Ten model korzysta z aplikacji pojedynczej dzierżawy. Dlatego ten model wymaga tylko jedną bazę danych i przechowuje dane tylko jednego dzierżawcy. Dzierżawca korzysta z pełnej izolacji od innych dzierżaw w bazie danych.

Można użyć tego modelu sprzedaży wystąpień aplikacji dla wielu różnych klientów, dla każdego klienta działać samodzielnie. Klient jest następnie tylko dzierżawy. Gdy baza danych programu przechowuje dane dla tylko jednego klienta, bazy danych przechowuje dane dla wielu klientów klienta.

#### <a name="database-per-tenant"></a>Bazy danych dla każdego dzierżawcy

![Bazę danych na model dzierżawy][database-per-tenant-model-35d]

Ten model ma wiele dzierżaw w wystąpienie aplikacji. Jeszcze dla każdego nowego dzierżawcy innej bazy danych jest przydzielona do użycia tylko przez nowym dzierżawcą.

Ten model zapewnia izolację pełnej bazy danych dla każdego dzierżawcy. Usługa bazy danych SQL Azure ma wiedzy dokonanie wiarygodne tego modelu.

- [Wprowadzenie do przykład aplikacji SaaS wielodostępne połączenia bazy danych SQL] [ saas-dbpertenant-wingtip-app-overview-15d] -ma więcej informacji na temat tego modelu.

#### <a name="sharded-multi-tenant-databases-the-hybrid"></a>Podzielonej wielodostępnych baz danych, hybrydowego

![Podzielonej wielodostępne bazy danych modelu, hybrydowego][sharded-multitenantdb-model-hybrid-79m]

Ten model ma wiele dzierżaw w wystąpienie aplikacji. Ten model ma wiele dzierżaw w niektórych lub wszystkich jej baz danych. Ten model jest prawidłowa dla oferty różnych warstwach usług, dzięki czemu klienci może zwrócić więcej jeśli wartość pełnej izolacji bazy danych.

Schemat każdej bazy danych zawiera identyfikator dzierżawy. Identyfikator dzierżawy jest nawet w tych baz danych, które przechowywane tylko jednego dzierżawcy.

- [Wprowadzenie do przykład aplikacji SaaS wielodostępne połączenia bazy danych SQL][saas-multitenantdb-get-started-deploy-89i]



## <a name="tutorials-for-each-tenancy-model"></a>Samouczki dotyczące każdego modelu dzierżawy

Każdy model dzierżawy jest udokumentowany przez następujące czynności:

- Zestaw samouczek artykułów.
- Kod źródłowy jest przechowywane w repozytorium Github, mający na celu modelu:
    - Kod aplikacji Wingtip biletów.
    - Kod skryptu scenariuszy zarządzania.

#### <a name="tutorials-for-management-scenarios"></a>Samouczki dotyczące scenariuszy zarządzania

Samouczek artykułów dla każdego modelu obejmuje następujące scenariusze zarządzania:

- Dzierżawca inicjowania obsługi administracyjnej.
- Monitorowanie wydajności i zarządzania.
- Zarządzanie schematu.
- Dzierżawcy między raportowania i analiz.
- Przywracanie jednego dzierżawcy do wcześniejszego punktu w czasie.
- Odzyskiwanie sprawności systemu po awarii.



## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do przykład aplikacji SaaS wielodostępne połączenia bazy danych SQL] [ saas-dbpertenant-wingtip-app-overview-15d] -ma więcej informacji na temat tego modelu.

- [Wielodostępne SaaS bazy danych dzierżawców wzorców][multi-tenant-saas-database-tenancy-patterns-60p]



<!-- Image references. -->

[standalone-app-model-62s]: media/saas-tenancy-welcome-wingtip-tickets-app/model-standalone-app.png "Model aplikacji autonomicznych"

[database-per-tenant-model-35d]: media/saas-tenancy-welcome-wingtip-tickets-app/model-database-per-tenant.png "Bazę danych na model dzierżawy"

[sharded-multitenantdb-model-hybrid-79m]: media/saas-tenancy-welcome-wingtip-tickets-app/model-sharded-multitenantdb-hybrid.png "Podzielonej wielodostępne bazy danych modelu, hybrydowego"



<!-- Article references. -->

[saas-dbpertenant-wingtip-app-overview-15d]: saas-dbpertenant-wingtip-app-overview.md

[multi-tenant-saas-database-tenancy-patterns-60p]: saas-tenancy-app-design-patterns.md

[saas-multitenantdb-get-started-deploy-89i]: saas-multitenantdb-get-started-deploy.md


