---
title: "Projektowanie wzorce dla wielodostępnych aplikacji SaaS i usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat wymagań i wspólne dane wzorce architektura wielodostępnej oprogramowania jako usługa (SaaS) aplikacje baz danych, które są uruchamiane w środowisku chmury."
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 02/01/2017
ms.author: srinia
ms.openlocfilehash: eef48cfcbc7d6c241b5ece863df0be6ecad78ca7
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="design-patterns-for-multi-tenant-saas-applications-and-azure-sql-database"></a>Projektowanie wzorce dla wielodostępnych aplikacji SaaS i bazy danych SQL Azure
W tym artykule można zapoznać się wymagania i wspólne dane wzorce architektura wielodostępnej oprogramowania jako usługa (SaaS) aplikacje baz danych, które są uruchamiane w środowisku chmury. Wyjaśniono również czynników, które należy wziąć pod uwagę i kompromis wzorce projektowania. Pule elastyczne i narzędzi elastycznej bazy danych SQL Azure może pomóc własnych wymagań bez naruszania innych celów.

Deweloperzy czasami podejmować decyzje, które działają przed ich zainteresowaniami najlepsze długoterminowej podczas projektowania modeli dzierżawy dla warstwy danych aplikacje wielodostępne. Początkowo co najmniej deweloper może szybko wychwycić łatwość programowania i obniżyć koszty dostawcy usług chmury jako większe znaczenie niż izolacji dzierżawców i skalowalność aplikacji. Ten wybór może prowadzić do problemów zadowolenia klienta i kosztowne korekty kursu później.

Aplikacja wielodostępne jest aplikacji hostowanej w środowisku chmury i który zawiera ten sam zestaw usług setek lub tysięcy dzierżawców, którzy nie udostępniać lub jego dane są widoczne. Przykładem jest aplikacja SaaS, która udostępnia usługi do dzierżawców w środowisku hostowanych w chmurze.

## <a name="multi-tenant-applications"></a>Aplikacje wielodostępne
W aplikacjach wielodostępne danych i obciążenia mogą być łatwo partycjonowane. Można podzielić danych oraz obciążeniu, na przykład na bloki dzierżawy, ponieważ większość żądań są wykonywane w obrębie dzierżawcy. Ta właściwość jest związane z danych i obciążenia, a jego objawach wzorce aplikacji omówione w tym artykule.

Deweloperzy używać tego typu aplikacji w aplikacji opartej na chmurze, w tym w pełnym zakresie:

* Partner bazy danych aplikacji, które są trwa są przenoszone do chmury co aplikacje SaaS
* Zbudowany dla chmury od podstaw w aplikacji SaaS
* Bezpośrednie, uwzględniającym klienta aplikacji
* Pracownik aplikacjach enterprise

Aplikacji SaaS, które są przeznaczone do chmury oraz tych, których certyfikaty główne partnera aplikacje baz danych zazwyczaj są aplikacje wielodostępne. Te aplikacje SaaS dostarczanie specjalistycznych aplikacji jako usługa dzierżawcom. Dzierżawcy można uzyskać dostęp do usługi aplikacji i mają pełne prawa własności skojarzone dane przechowywane w ramach aplikacji. Jednak aby skorzystać z zalet SaaS, dzierżaw musi przekazanie pewną kontrolę nad ich danymi. Ufają dostawcę usługi SaaS, aby zapewnić danych bezpieczne i odizolowane od pozostałych dzierżawców danych. Przykładem tego rodzaju wielodostępnych aplikacji SaaS są MYOB, SnelStart i Salesforce.com. Każda z tych aplikacji mogą być partycjonowane wzdłuż granic dzierżawy i pomocy technicznej, które omówiono w tym artykule projektowania aplikacji.

Aplikacje, które udostępniają usługi bezpośredniego klientom lub pracowników w ramach organizacji (nazywanej często użytkowników zamiast dzierżawcy) są inną kategorię na spektrum wielodostępnych aplikacji. Klienci subskrybować usługę i nie ma dostawcy usług zbiera i przechowuje dane. Dostawcy usług mają mniej rygorystyczne wymagania dotyczące przechowywania danych ich klientom odizolowane od siebie poza obowiązkowe dla instytucji rządowych zasad zachowania poufności. Przykładem tego rodzaju aplikacji wielodostępnych uwzględniającym klienta są nośnika dostawców zawartości, takich jak Netflix, serwis Spotify i Xbox LIVE. Inne przykłady aplikacji łatwo partitionable są skierowane do klienta, aplikacji w skali Internet lub aplikacji Internetu rzeczy (IoT), w którym każdy klienta lub urządzenia można służyć jako partycji. Granice partycji można oddzielić użytkowników i urządzeń.

Nie wszystkie aplikacje partycji łatwo wzdłuż jednej właściwości, takie jak dzierżawy, odbiorcy, użytkownika lub urządzenia. Zasób przedsiębiorstwa złożonych planowania aplikacji (ERP), na przykład ma produktów, zamówień i klientów. Zwykle ma on złożonych schematu z tysiącami wysokiej połączonych tabel.

Żadna strategia jednej partycji można zastosować do wszystkich tabel i działa między obciążenia aplikacji. Ten artykuł skupia się na aplikacje wielodostępne, które łatwo partitionable danych i obciążeń.

## <a name="multi-tenant-application-design-trade-offs"></a>Kompromis projektowania aplikacji z wieloma dzierżawcami
Wzorzec projektowania, który Deweloper aplikacji wielodostępnych zazwyczaj wybiera opiera się na jest brany pod uwagę następujące czynniki:

* **Odizolowania dzierżawców**. Deweloper musi upewnij się, że żaden z dzierżawców ma niechcianego dostępu do danych innych dzierżawców. Wymaganie izolacji rozszerza do innych właściwości, takie jak zapewnienie ochrony z sąsiadów zakłócenia, możliwość przywrócenia danych dzierżawcy i wdrażanie dostosowań specyficznego dla dzierżawy.
* **Koszt zasobów w chmurze**. Aplikacja SaaS musi być konkurencyjnych kosztów. Deweloper aplikacji wielodostępnych wybrać Optymalizuj dla niższe koszty w przypadku użycia zasobów w chmurze, takie jak magazyn i obliczeń kosztów.
* **Łatwość DevOps**. Deweloper aplikacji wielodostępnych musi dołączyć do nich ochrony izolacji, obsługi, monitorowania kondycji aplikacji i schemat bazy danych i rozwiązywania problemów dzierżawy. Złożoność w opracowywaniu aplikacji oraz operacji tłumaczy bezpośrednio do zwiększenia kosztów i będzie wymagał z zadowolenia dzierżawy.
* **Skalowalność**. Możliwość stopniowego dodawania więcej dzierżaw i pojemności dla dzierżawców, którzy go potrzebują jest powodzenie operacji SaaS.

Każdy z nich ma możliwości w porównaniu do innego. Chmura najniższy koszt oferty nie może oferować najodpowiedniejszym środowisko programistyczne. Jest ważne dla deweloperów do podejmowanie świadomych wyborów o te opcje i możliwości ich podczas projektowania aplikacji.

Wzorzec popularnych programowanie jest pakietu wielu dzierżawców do jednej lub kilku baz danych. Zalety tego podejścia są niższe koszty, ponieważ płacisz za kilka baz danych i względną prostotę Praca z ograniczoną liczbę baz danych. Jednak wraz z upływem czasu dewelopera wielodostępnych aplikacji SaaS będzie Pamiętaj, że ten wybór ma istotne downsides izolacji dzierżawców i skalowalność. W przypadku izolacji dzierżawców ważne, dodatkowego nakładu pracy jest wymagana do ochrony danych dzierżawy w magazynie udostępnionym przed nieautoryzowanym dostępem lub zakłócenia sąsiadów. Tego dodatkowego nakładu pracy może znacznie zwiększyć programistycznych i kosztów obsługi izolacji. Podobnie jeśli wymagana jest dodanie dzierżawcy, tym wzorcu projektowym wymaga zwykle od doświadczenia, aby ponownie rozesłać danych dzierżawy dla baz danych prawidłowo skalowania warstwy danych aplikacji.  

Często izolacji dzierżawy jest podstawowe wymagania w aplikacjach wielodostępne SaaS, które automatycznie dostosowują się do firmy i organizacje. Deweloper może skłonny przez postrzegana zalet prostotę i koszt w porównaniu z izolacji dzierżawców i skalowalność. Takie rozwiązanie może okazać się złożone i kosztowne miarę rozwoju usługi i wymagania dotyczące izolacji dzierżawy staną się bardziej ważne i zarządzanych w warstwie aplikacji. Jednak w wielodostępnych aplikacji, które klientom usługi bezpośredniego, dla użytkownika, izolacji dzierżawców może być niższy priorytet niż optymalizacji dla koszt zasobów chmury.

## <a name="multi-tenant-data-models"></a>Modeli danych z wieloma dzierżawcami
Typowe rozwiązania projekt umieszczenia danych dzierżawy należy wykonać trzy różne modele, pokazany na rysunku 1.

![modeli danych aplikacji z wieloma dzierżawcami](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

Rysunek 1: Typowe projektowania rozwiązania modeli danych z wieloma dzierżawcami

* **Bazy danych dla dzierżawy**. Każdy dzierżawca ma własną bazę danych. Wszystkie dane specyficzne dla dzierżawy jest ograniczone do bazy danych dzierżawcy i odizolowane od pozostałych dzierżawców i ich danych.
* **Udostępnione bazy danych podzielonej**. Wiele dzierżaw współużytkować jeden z wielu baz danych. Odrębnego zestawu dzierżawcy jest przypisany do każdej bazy danych przy użyciu strategii partycjonowania, takich jak wyznaczania wartości skrótu, zakres lub listy partycjonowania. Ta strategia dystrybucji danych często nazywa się dzielenia na fragmenty.
* **Udostępnione pojedynczej bazy danych**. Pojedynczy, czasami duży bazy danych zawiera dane dla wszystkich dzierżawców, które są rozróżniane w kolumnie Identyfikator dzierżawy.

> [!NOTE]
> Deweloper aplikacji może wybrać umieszczenie różnych dzierżaw w schematach innej bazy danych, a następnie użyj nazwy schematu do odróżniania różnym dzierżawcom. Nie zaleca się tą metodą, ponieważ zwykle wymaga użycia dynamicznych SQL i nie może być aktywne buforowanie planu. W dalszej części tego artykułu możemy skupić się na podejście tabeli udostępnionego dla tej kategorii wielodostępnych aplikacji.
> 
> 

## <a name="popular-multi-tenant-data-models"></a>Modele popularnych danych z wieloma dzierżawcami
Należy ocenić różnych typów modeli danych wielodostępne pod względem aplikacji projektowania kompromisy które już określiliśmy. Czynniki te pomagają scharakteryzowania trzy modele danych wielodostępne najczęściej opisanych wcześniej i ich użycia bazy danych, jak pokazano na rysunku 2.

* **Izolacja**. Stopień izolacji między dzierżawcami może być środek izolacji dzierżawców, ile osiąga modelu danych.
* **Koszt zasobów w chmurze**. Ilość udostępniania zasobów między dzierżawcami zoptymalizować koszt zasobów chmury. Zasób można zdefiniować jako koszt zasobów obliczeniowych i magazynu.
* **Koszt DevOps**. Łatwość tworzenia aplikacji, wdrażania i zarządzania zmniejsza całkowity koszt operacji SaaS.  

Na rysunku 2 osi Y wskazuje poziom izolacji dzierżawców. Na osi X zawiera poziom udostępniania zasobów. Szary, ukośnej strzałki w środku wskazuje kierunek tendencję do zwiększenia lub zmniejszenia kosztów DevOps.

![Wzorce projektowe popularnych aplikacji wieloma dzierżawcami](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

Rysunek 2: Modele popularnych danych wielodostępne

Wiązania kwadrantu prawym dolnym na rysunku 2 przedstawiono wzorzec aplikacji, który używa potencjalnie dużą, udostępnionych pojedynczej bazy danych i tabeli udostępnionych (lub osobnych schematu) podejście. To ułatwia udostępnianie ponieważ wszystkich dzierżawców używać tych samych zasobów bazy danych (Procesora, pamięci, we/wy) w jednej bazie danych zasobów. Ale izolacji dzierżawców, jest ograniczona. Należy wykonać dodatkowe czynności, aby chronić dzierżaw od siebie na warstwie aplikacji. Te dodatkowe kroki mogą znacznie zwiększyć DevOps koszt tworzenia i zarządzania aplikacji. Skalowalność jest ograniczona przez skali sprzętu, który jest hostem bazy danych.

Wiązania kwadrantu lewym dolnym na rysunku 2 przedstawiono wielu dzierżawców podzielonej między wieloma bazami danych (zwykle innego sprzętu jednostek skalowania). Każda baza danych obsługuje podzbiór dzierżawcy, którego dotyczy problem dotyczący skalowalność z innymi wzorami. Jeśli większej pojemności jest wymagana w przypadku dzierżaw więcej, można łatwo umieścić dzierżawcy na nowych baz danych przydzielone do jednostki skalowania usługi nowego sprzętu. Jednak ilość udostępniania zasobów zostanie zmniejszona. Tylko dzierżaw umieszczone na tej samej jednostki skalowania współużytkowanie zasobów. Metoda ta umożliwia małego poprawy odizolowania dzierżawców, ponieważ wiele dzierżaw nadal są zawsze umieszczane bez automatycznie chronione od siebie nawzajem akcje. Złożoność aplikacji pozostaje wysoki.

Wiązania kwadrantu lewej górnej na rysunku 2 jest trzeci rozwiązanie. Dane każdego dzierżawcy umieszcza w własną bazę danych. Takie podejście ma właściwości dobrej izolacji dzierżawców, ale ogranicza udostępniania zasobów, gdy każda baza danych ma własną zasobów dedykowanych. Ta metoda jest zalecana, jeśli wszystkie dzierżawy mają przewidywalna obciążeń. Jeśli obciążenia dzierżaw są mniej przewidywalne, dostawca nie można zoptymalizować udostępniania zasobów. Nieprzewidywalność jest typowe dla aplikacji SaaS. Dostawca musi być albo nadmiernego udostępniania do spełnienia wymagań lub niższy zasobów. Każda z tych operacji powoduje wyższe koszty lub niższy zadowolenia dzierżawy. Wyższy stopień zasobów, udostępnianie dzierżaw staje się pożądane, aby bardziej ekonomiczne rozwiązanie. Zwiększenie liczby baz danych zwiększa także koszt DevOps, wdrażaniu i obsłudze aplikacji. Pomimo tych problemów ta metoda zapewnia izolację najlepszym i najłatwiejszy dzierżaw.

Te czynniki wpływają również wzorzec projektowania, który klient wybierze:

* **Własność danych dzierżawy**. Aplikacja, w którym dzierżaw zachować własność własne dane objawach wzorzec pojedynczej bazy danych dla każdego dzierżawcy.
* **Skalowalność**. Aplikacja, która dotyczy setki tysięcy lub miliony dzierżaw objawach udostępniania podejścia, takie jak dzielenia na fragmenty bazy danych. Wymagania dotyczące izolacji nadal może stanowić wyzwanie.
* **Wartość i business modelu**. Jeśli aplikacja na dzierżawcy przychodu relacjami małych (mniej niż dolara), wymagania dotyczące izolacji stają się mniej istotny i udostępnionej bazy danych. W przypadku kilku kwoty lub więcej przychód na dzierżawy modelu bazy danych na dzierżawy jest coraz. Może pomóc, zmniejszyć koszty rozwoju.

Biorąc pod uwagę projekt kompromisy pokazany na rysunku 2, idealne rozwiązanie w modelu wielodostępnym musi właściwości izolacji dzierżawy dobrej zastosować z zasobem optymalne udostępnianie między dzierżawcami. Ten model mieści się w kategorii opisanego w wiązania kwadrantu prawym górnym rysunku 2.

## <a name="multi-tenancy-support-in-azure-sql-database"></a>Obsługa wielu dzierżawców w bazie danych SQL Azure
Baza danych SQL Azure obsługuje wszystkie wzorce wielodostępnych aplikacji wyróżnione na rysunku 2. Pule elastyczne obsługuje ona również wzorzec aplikacji, który łączy udostępniania zasobów dobrej i korzyści izolacji bazy danych na poszczególnych dzierżawcy podejścia (zobacz wiązania kwadrantu prawym górnym na rysunku 3). Narzędzi elastycznej bazy danych i możliwości w bazie danych SQL ograniczyć koszty umożliwia tworzenie i obsługę aplikacji, która ma wiele baz danych (znajdujące się na zacienionym obszarze na rysunku 3). Te narzędzia ułatwiają tworzenie i zarządzanie aplikacjami, które korzystają z dowolnych wzorce obsługi wielu baz danych.

![Wzorce w bazie danych Azure SQL](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

Rysunek 3: Wzorce wielodostępnych aplikacji w bazie danych SQL Azure

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Model bazy danych dla dzierżawy o elastycznych pul i narzędzia
Pule elastyczne w bazie danych SQL łączyć izolacji dzierżawcy z zasobów, udostępnianie między baz danych dzierżawy w celu skuteczniejszej obsługi podejście bazy danych dla dzierżawy. Baza danych SQL jest rozwiązanie SaaS dostawców, którzy tworzą aplikacje wielodostępne warstwy danych. Obciążenie zasobów, udostępnianie między dzierżawcami przewiduje z warstwy aplikacji w warstwie usług bazy danych. Złożoność zarządzania i wykonywanie zapytań na dużą skalę w baz danych zostało uproszczone dzięki zadania elastyczne, elastyczne zapytania transakcji elastycznej i biblioteki klienta elastycznej bazy danych.

| Wymagania dotyczące aplikacji | Funkcje bazy danych SQL |
| --- | --- |
| Izolacji dzierżawców i udostępnianie zasobów |[Pule elastyczne](sql-database-elastic-pool.md): Przydziel pulę zasobów bazy danych SQL i udostępnianie zasobów między różnymi bazami danych. Ponadto pojedynczych baz danych można narysować tyle samo zasobów z puli w celu uwzględnienia pojemności nagłego żądanie z powodu zmian w obciążeń dzierżawców. Sam puli elastycznej można skalować w górę lub w dół zgodnie z potrzebami. Pule elastyczne udostępniają łatwość zarządzania i monitorowania i rozwiązywania problemów na poziomie puli. |
| Łatwość DevOps dla baz danych |[Pule elastyczne](sql-database-elastic-pool.md): jak wspomniano wcześniej. |
| | [Zapytanie elastycznej](sql-database-elastic-query-horizontal-partitioning.md): zapytania dla baz danych na potrzeby raportowania lub cross dzierżawy analizy. |
| | [Zadania elastyczne](sql-database-elastic-jobs-overview.md): pakiet i niezawodny sposób wdrażania operacji konserwacji bazy danych lub zmiany schematu bazy danych do wielu baz danych. |
| | [Transakcje elastycznej](sql-database-elastic-transactions-overview.md): proces zmiany szereg baz danych w sposób atomic i izolowanym. Elastyczne transakcje są wymagane, gdy aplikacje muszą "wszystkie lub żadne" gwarancje przez kilka operacji w bazie danych. |
| | [Biblioteki klienta elastycznej bazy danych](sql-database-elastic-database-client-library.md): Zarządzanie dystrybucje danych i zamapuj dzierżawcy do baz danych. |

## <a name="shared-models"></a>Udostępnione modele
Jak opisano wcześniej, dostawców większości SaaS, podejście modelu udostępnionego może stanowić problemy z problemami dotyczącymi izolacji dzierżawy i złożoności przy opracowywaniu aplikacji oraz konserwacji. Jednak dla wielodostępnych aplikacji, które dostarcza usługi bezpośrednio do użytkowników, wymagania dotyczące izolacji dzierżawy nie może być wysoki priorytet jako minimalizowania kosztów. Mogą one możliwość pakietu dzierżaw w jedną lub więcej baz danych o wysokiej gęstości zmniejszyć koszty. Modele udostępnione bazy danych przy użyciu pojedynczej bazy danych lub wielu baz danych podzielonej może spowodować dodatkowych korzyści w koszt udostępniania i ogólnej zasobu. Baza danych SQL Azure oferuje funkcje, które ułatwiają klientom tworzenie izolację lepsze bezpieczeństwo i zarządzania na dużą skalę w warstwie danych.

| Wymagania dotyczące aplikacji | Funkcje bazy danych SQL |
| --- | --- |
| Funkcje zabezpieczeń izolacji |[Zabezpieczenia na poziomie wiersza](https://msdn.microsoft.com/library/dn765131.aspx) |
| [Schemat bazy danych](https://msdn.microsoft.com/library/dd207005.aspx) | |
| Łatwość DevOps dla baz danych |[Elastyczne zapytania](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Zadania elastyczne](sql-database-elastic-jobs-overview.md) |
| | [Elastyczne transakcji](sql-database-elastic-transactions-overview.md) |
| | [Biblioteka kliencka Elastic Database](sql-database-elastic-database-client-library.md) |
| | [Podziel elastycznej bazy danych i scalania](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Podsumowanie
Wymagania dotyczące izolacji dzierżawy są ważne w przypadku większości aplikacji wielodostępnych SaaS. Najlepszą opcją zapewnienie izolacji silnie leans kierunku podejście bazy danych dla dzierżawy. Dwa podejścia wymaga inwestycji w warstwy aplikacji złożonych, które wymagają programowanie wykwalifikowanych pracowników zapewnienie izolacji, co znacznie zwiększa koszt i ryzyko. Jeśli wymagania dotyczące izolacji nie uwzględnia się na początku programowanie usługi, modernizacji je można jeszcze bardziej kosztowne w pierwszych dwóch modeli. Główne wady, skojarzonego z tym modelem bazy danych dla dzierżawy odnoszą się do zwiększenia chmury kosztów zasobów z powodu zmniejszenie udostępniania i utrzymywania oraz zarządzania wielu baz danych. Deweloperzy aplikacji SaaS napotykają po wysłaniu tych kompromisy.

Chociaż kompromisy może być istotne bariery z większość dostawców usług bazy danych w chmurze, bazę danych SQL Azure eliminuje bariery z puli elastycznej i możliwości elastycznej bazy danych. Deweloperzy SaaS można łączyć właściwości izolacji modelu bazy danych na dzierżawy i zoptymalizować współużytkowanie zasobów i usprawnienia zarządzania wielu baz danych za pomocą elastycznych pul i powiązane z nim narzędzia.

Dostawcy wielodostępnych aplikacji, które nie mają żadnych wymagań izolacji dzierżawy i który pakietu dzierżaw w bazie danych o wysokiej gęstości może się okazać, że udostępnionych danych modeli wynik w dodatkowych efektywna współużytkowanie zasobów i zmniejszyć całkowity koszt. Narzędzi elastycznej bazy danych w usłudze Azure SQL Database, bibliotek dzielenia na fragmenty i funkcje zabezpieczeń ułatwiają tworzenie i zarządzanie nimi aplikacje wielodostępne dostawców w modelu SaaS.

## <a name="next-steps"></a>Następne kroki
[Wprowadzenie do narzędzi elastycznej bazy danych](sql-database-elastic-scale-get-started.md) z przykładową aplikację prezentującą biblioteki klienta.

Utwórz [puli elastycznej niestandardowy pulpit nawigacyjny rozwiązania saas](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) z przykładowej aplikacji, który korzysta z puli elastycznej dla rozwiązania ekonomicznych, skalowalnych bazy danych.

Użyj narzędzia bazy danych SQL Azure, aby [migracji istniejących baz danych do skalowania w poziomie](sql-database-elastic-convert-to-use-elastic-tools.md).

Aby utworzyć puli elastycznej za pomocą portalu Azure, zobacz [tworzenia puli elastycznej](sql-database-elastic-pool-manage-portal.md).  

Dowiedz się, jak [monitorować i zarządzać nimi puli elastycznej](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wdrażanie i Eksploruj aplikacji wielodostępnych, która używa bazy danych SQL Azure - Wingtip SaaS](sql-database-saas-tutorial.md)
* [Co to jest puli elastycznej platformy Azure?](sql-database-elastic-pool.md)
* [Scaling out with Azure SQL Database (Skalowanie w poziomie za pomocą usługi Azure SQL Database)](sql-database-elastic-scale-introduction.md)
* [aplikacje wielodostępne z narzędzi elastycznej bazy danych i zabezpieczenia na poziomie wiersza](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Uwierzytelnianie w aplikacjach wielu dzierżawców przy użyciu usługi Azure Active Directory i OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Aplikacja Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Pytania i żądania funkcji

Odpowiedzi na pytania, Znajdź NAS w [forum bazy danych SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Dodaj żądanie funkcji [forum opinii bazy danych SQL](https://feedback.azure.com/forums/217321-sql-database/).

