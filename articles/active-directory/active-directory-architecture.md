---
title: "Informacje na temat architektury usługi Azure Active Directory | Microsoft Docs"
description: "Wyjaśnienia dotyczące pojęcia „dzierżawa usługi Azure AD” i zarządzania nią za pomocą usługi Azure Active Directory"
services: active-directory
documentationcenter: 
author: markvi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 5c60fa737c0133482af8b653f795bf9086c39969
ms.lasthandoff: 03/28/2017


---
# <a name="understand-azure-active-directory-architecture"></a>Informacje na temat architektury usługi Azure Active Directory
Usługa Azure Active Directory (Azure AD) umożliwia bezpieczne zarządzanie dostępem do usług i zasobów platformy Azure dla użytkowników. W ramach usługi Azure AD można skorzystać z pełnego zestawu możliwości zarządzania tożsamościami. Aby uzyskać więcej informacji na temat funkcji usługi Azure AD, zobacz [Co to jest usługa Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis)

Za pomocą usługi Azure AD możesz tworzyć użytkowników i grupy oraz zarządzać nimi, a także zezwalać na dostęp do zasobów organizacji i odmawiać go przy użyciu uprawnień. Aby uzyskać informacje dotyczące zarządzania tożsamościami, zobacz [The fundamentals of Azure identity management](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity) (Podstawy zarządzania tożsamościami platformy Azure).

## <a name="azure-ad-architecture"></a>Architektura usługi Azure AD
Rozproszona geograficznie architektura usługi Azure AD umożliwia rozbudowane monitorowanie, automatyczne przekierowywanie, korzystanie z trybu failover i odzyskiwanie — pozwala to dostarczyć klientom dostępność i wydajność na poziomie korporacyjnym.

W tym artykule omówione są następujące elementy architektury:
 *    Projekt architektury usługi
 *    Skalowalność 
 *    Ciągła dostępność
 *    Centra danych

### <a name="service-architecture-design"></a>Projekt architektury usługi
Najczęstszym sposobem budowania skalowalnego systemu o wysokiej dostępności z dużą ilością danych jest użycie niezależnych bloków konstrukcyjnych lub jednostek skalowania warstwy danych usługi Azure AD. Jednostki skalowania są nazywane *partycjami*. 

Warstwa danych ma kilka usług frontonu, które zapewniają możliwość odczytu i zapisu. Na poniższym diagramie pokazano sposób, w jaki składniki partycji pojedynczego katalogu są dystrybuowane w obrębie rozproszonych geograficznie centrów danych. 

  ![Partycje pojedynczego katalogu](./media/active-directory-architecture/active-directory-architecture.png)

Składniki architektury usługi Azure AD obejmują repliki podstawowe i pomocnicze.

**Replika podstawowa**

*Replika podstawowa* odbiera wszystkie operacje *zapisu* dla partycji, do której należy. Wszystkie operacje zapisu są natychmiast replikowane do repliki pomocniczej znajdującej się w innym centrum danych przed zwróceniem do obiektu wywołującego informacji o powodzeniu, co pozwala na zapewnienie nadmiarowej geograficznie trwałości zapisów.

**Repliki pomocnicze**

Wszystkie operacje *odczytu* z katalogu są obsługiwane przez *repliki pomocnicze* w centrach danych, które fizycznie znajdują się w różnych lokalizacjach geograficznych. Istnieje wiele replik pomocniczych, ponieważ dane są replikowane w sposób asynchroniczny. Operacje odczytu z katalogu, takie jak żądania uwierzytelniania, są obsługiwane przez centra danych znajdujące się blisko klientów. Repliki pomocnicze są odpowiedzialne za skalowalność odczytu.

### <a name="scalability"></a>Skalowalność

Skalowalność to możliwość rozszerzania działania usługi w związku z rosnącymi wymaganiami dotyczącymi wydajności. Skalowalność zapisu jest osiągana przez partycjonowanie danych. Skalowalność odczytu uzyskuje się przez replikację danych z jednej partycji do wielu replik pomocniczych rozproszonych na całym świecie.

Żądania z katalogu aplikacji są zazwyczaj kierowane do centrum danych, które jest im fizycznie najbliższe. Zapisy są w sposób niewidoczny dla użytkownika przekierowywane do repliki podstawowej w celu zapewnienia spójności odczytu i zapisu. Repliki pomocnicze znacznie rozszerzają skalę partycji, ponieważ katalogi zazwyczaj przez większość czasu obsługują operacje odczytu.

Katalogi aplikacji nawiązują połączenie z najbliższymi centrami danych. Powoduje to zwiększenie wydajności, co umożliwia przeprowadzenie skalowania w poziomie. Ponieważ partycje katalogów mogą mieć wiele replik pomocniczych, repliki pomocnicze można umieścić bliżej klientów katalogu. Tylko wewnętrzne składniki usługi katalogu przeprowadzające wiele operacji odczytu bezpośrednio komunikują się z aktywną repliką podstawową.

### <a name="continuous-availability"></a>Ciągła dostępność

Dostępność (lub czas dostępności) definiuje możliwość wykonywania przez system nieprzerwanej pracy. Kluczem do wysokiej dostępności usługi Azure AD jest możliwość szybkiego przełączania ruchu między wiele rozproszonych geograficznie centrów danych. Każde centrum danych jest niezależne, co umożliwia wystąpienie nieskorelowanych trybów awarii.

Projekt partycji usługi Azure AD jest uproszczony w porównaniu do firmowego projektu usługi AD, który ma kluczowe znaczenie dla skalowania systemu w górę. Zaadoptowaliśmy projekt pojedynczego elementu głównego, który obejmuje starannie zorganizowany i deterministyczny proces pracy w trybie failover repliki podstawowej.

**Odporność na uszkodzenia**

System jest bardziej dostępny, jeśli jest odporny na błędy sprzętu, sieci i oprogramowania. Dla każdej partycji w katalogu istnieje replika główna o wysokiej dostępności — replika podstawowa. W ramach tej repliki są wykonywane tylko operacje zapisu. Ta replika jest stale i ściśle monitorowana, a operacje zapisu w przypadku wykrycia błędu mogą zostać natychmiast przełączone do innej repliki, która staje się wtedy nową repliką podstawową. Podczas pracy w trybie failover może to być związane z utratą dostępności do zapisu trwającą zwykle od 1 do 2 minut. Taka sytuacja nie ma wpływu na dostępność do odczytu.

Operacje odczytu (których liczba jest wiele rzędów wielkości większa od operacji zapisu) odbywają się tylko za pośrednictwem replik pomocniczych. Ponieważ repliki pomocnicze są idempotentne, utrata dowolnej repliki w danej partycji jest łatwo kompensowana przez przełączenie operacji odczytu do innej repliki, która zwykle znajduje się w tym samym centrum danych.

**Trwałość danych**

Operacja zapisu jest trwale zatwierdzana w co najmniej dwóch centrach danych przed potwierdzeniem. Najpierw ma miejsce zatwierdzanie operacji zapisu w replice podstawowej, a następnie operacja zapisu jest natychmiast replikowana do co najmniej jednego innego centrum danych. Pozwala to zapewnić, że potencjalna krytyczna utrata centrum danych hostującego replikę podstawową nie spowoduje utraty danych.

Usługa Azure AD obsługuje zerowy [cel czasu odzyskiwania (RTO, Recovery Time Objective)](https://en.wikipedia.org/wiki/Recovery_time_objective) dla operacji wystawiania tokenów i odczytu z katalogu w celu zapewnienia minutowego (około 5 minut) celu czasu odzyskiwania dla operacji zapisu w katalogu. Obsługujemy również zerowy [cel punktu odzyskiwania (RPO, Recovery Point Objective)](https://en.wikipedia.org/wiki/Recovery_point_objective) i zapewniamy, że dane nie zostaną utracone podczas przechodzenia w tryb failover.

### <a name="data-centers"></a>Centra danych

Repliki usługi Azure AD są przechowywane w centrach danych znajdujących się na całym świecie. Aby uzyskać więcej informacji, zobacz [Centra danych platformy Azure](https://azure.microsoft.com/en-us/overview/datacenters).

Działania usługi Azure AD względem centrów danych mają następujące cechy:

 * Uwierzytelnianie, usługa Graph i inne usługi AD znajdują się za usługą bramy. Brama zarządza równoważeniem obciążenia tych usług. Automatycznie przejdzie ona w tryb failover, jeśli za pomocą transakcyjnych sond kondycji wykryte zostaną serwery w złej kondycji. Na podstawie danych z tych sond kondycji brama dynamicznie kieruje ruchem do centrów danych będących w dobrej kondycji.
 * Na potrzeby operacji *odczytu* katalog posiada repliki pomocnicze i odpowiednie usługi frontonu w ramach konfiguracji aktywne-aktywne działające w wielu centrach danych. W razie awarii całego centrum danych ruch zostanie automatycznie skierowany do innego centrum danych.
 *    Na potrzeby operacji *zapisu* katalog spowoduje przejście w tryb failover repliki podstawowej (głównej) w centrach danych za pośrednictwem planowanych (nowa replika podstawowa jest synchronizowana ze starą repliką podstawową) lub awaryjnych procedur przejścia w tryb failover. Trwałość danych jest zapewniana przez replikowanie każdego zatwierdzenia do co najmniej dwóch centrów danych.

**Spójność danych**

Model katalogu to jeden z modeli ze spójnością ostateczną. Jednym z typowych problemów z rozproszonymi systemami replikowanymi asynchronicznie jest potencjalna nieaktualność danych zwracanych z „konkretnej” repliki. 

Usługa Azure AD zapewnia spójność odczytu i zapisu dla aplikacji działających w ramach repliki pomocniczej, kierując jej operacje zapisu do repliki podstawowej i synchronicznie pobierając operacje zapisu do repliki pomocniczej.

Operacje zapisu aplikacji wykonywane za pomocą interfejsu API programu Graph usługi Azure AD są odseparowane od operacji zapewniania koligacji z repliką katalogu w celu zapewnienia spójności odczytu i zapisu. Usługa Azure AD Graph obsługuje sesję logiczną będącą w koligacji z repliką pomocniczą używaną do operacji odczytu; koligacja jest przechwytywana w ramach „tokenu repliki” buforowanego przez usługę Graph za pomocą rozproszonej pamięci podręcznej. Token ten jest następnie używany na potrzeby kolejnych operacji w ramach tej samej sesji logicznej. 

 >[!NOTE]
 >Operacje zapisu są natychmiast replikowane do repliki pomocniczej, dla której zostały utworzone operacje odczytu sesji logicznej.
 >

**Ochrona kopii zapasowych**

Katalog implementuje usuwanie elastyczne zamiast usuwania całkowitego w celu umożliwienia użytkownikom i dzierżawcom łatwego odzyskiwania w razie wykonania przypadkowego usunięcia przez klienta. Jeśli administrator dzierżawy przypadkowo usunie użytkowników, może on łatwo cofnąć tę operację i przywrócić usuniętych użytkowników. 

Usługa Azure AD implementuje codzienne tworzenie kopii zapasowych wszystkich danych i w związku z tym umożliwia skuteczne przywrócenie danych w przypadku wykonania jakichkolwiek logicznych operacji usuwania lub uszkodzenia danych. Nasze warstwy danych korzystają z kodu służącego do korygowanie kodów, dzięki czemu możliwe jest wykonanie sprawdzenia w poszukiwaniu błędów i automatyczne naprawienie określonych rodzajów błędów na dysku.

**Metryki i monitory**

Uruchamianie usługi o wysokiej dostępności wymaga światowej klasy metryk i możliwości monitorowania. Usługa Azure AD stale analizuje i raportuje kluczowe metryki kondycji usługi i kryteria powodzenia dla każdej ze swoich usług. Nieustannie opracowujemy i dostrajamy metryki, monitorowanie i alerty dla poszczególnych scenariuszy w ramach każdej z usług Azure AD i wszystkich innych usług.

Jeśli jakakolwiek z usług Azure AD nie działa zgodnie z oczekiwaniami, natychmiast podejmujemy działania w celu jak najszybszego przywrócenia jej funkcjonalności. Najważniejszą metryką śledzoną przez usługę Azure AD jest metryka określająca szybkość, z jaką możliwe jest wykrycie problemów klienta lub aktywnej witryny i usunięcie ich. Ciężko pracujemy nad funkcjami monitorowania i alertów w celu zminimalizowania czasu wykrywania (docelowy czas wykrywania: poniżej 5 minut) oraz nad gotowością operacyjną w celu zminimalizowania czasu usuwania problemu (docelowy czas usuwania problemu: poniżej 30 minut).

**Zabezpieczanie operacji**

Stosujemy kontrole operacyjne, takie jak uwierzytelnianie wieloskładnikowe (MFA) dla każdej operacji, a także inspekcję wszystkich operacji. Ponadto nieprzerwanie używamy systemu podnoszenia uprawnień w miarę potrzeb w celu udzielenia niezbędnego tymczasowego dostępu dla dowolnych operacyjnych zadań na żądanie. Aby uzyskać więcej informacji, zobacz [Zaufana chmura](https://azure.microsoft.com/en-us/support/trust-center).

## <a name="next-steps"></a>Następne kroki
[Przewodnik dewelopera usługi Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)


