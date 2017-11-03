---
title: "Planowanie wydajności dla usługi Azure Search | Dokumentacja firmy Microsoft"
description: "Dostosuj partycji i replik zasobów komputera w usłudze Azure Search, gdzie w jednostkach rozliczeniowy wyszukiwania kosztuje każdego zasobu."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 1dc16afe-56f9-439d-8874-1733ae1a2b74
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 02/08/2017
ms.author: heidist
ms.openlocfilehash: 26f5e71f3d00161a92de702209e224008ec8a5ae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>Poziomy zasobów skali kwerendy i indeksowania obciążeń w usłudze Azure Search
Po [wybierz warstwę cenową](search-sku-tier.md) i [alokowanie usługi wyszukiwania](search-create-service-portal.md), następnym krokiem jest opcjonalnie zwiększenie liczby replikami lub partycje używane przez usługę. Każda warstwa oferuje ustalona liczba jednostek rozliczeń. W tym artykule opisano sposób przydzielania tych jednostek, aby osiągnąć optymalną konfigurację, który równoważy wymagania dotyczące wykonywania zapytania, indeksowanie i magazynu.

Konfiguracja zasobu jest dostępna podczas konfigurowania usługi po [warstwy Basic](http://aka.ms/azuresearchbasic) lub jeden z [warstwy standardowa](search-limits-quotas-capacity.md). W przypadku rozliczeniowy usług w tych warstwach pojemności jest zakupić według przyrostów *jednostek wyszukiwania* (SUs) gdzie każdej partycji i replik liczone jako jeden SU. 

Użycie mniejszej liczby powoduje SUs rachunek proporcjonalnie niższa. Karta jest włączona dla tak długo, jak usługa jest skonfigurowana. Jeśli nie tymczasowo używasz usługi, jedynym sposobem uniknięcia rozliczeń jest usunięcie usługi, a następnie ponownie go utworzyć w razie konieczności.

> [!Note]
> Usunięcie usługi spowoduje usunięcie wszystkich na nim. Nie istnieje żadne funkcje w ramach usługi Azure Search do wykonywania kopii zapasowych i przywracanie utrwalone wyszukiwania danych. Aby ponownie wdrożyć istniejący indeks na nową usługę, należy uruchomić program używany do tworzenia i załaduj go pierwotnie. 

## <a name="terminology-partitions-and-replicas"></a>Terminologią: partycji i replik
Partycji i replik są głównej zasoby obsługujące usługi wyszukiwania.

| Zasób | Definicja |
|----------|------------|
|*Partycje* | Udostępnia magazynu indeksu i we/wy dla operacji odczytu/zapisu (na przykład podczas odbudowywania lub odświeżanie indeks).|
|*Repliki* | Wystąpienia usługi wyszukiwania używane głównie w celu operacje kwerend równoważenia obciążenia. Każda replika zawsze obsługuje jedną kopię indeksu. Jeśli masz 12 replik, masz 12 kopie każdy indeks załadowany na usługę.|

> [!NOTE]
> Nie istnieje sposób bezpośrednio manipulować ani zarządzanie nimi indeksów, które jest uruchamiane na replikę. Jedna kopia każdy indeks dla każdej repliki jest częścią architektury usługi.
>

## <a name="how-to-allocate-partitions-and-replicas"></a>Sposób przydzielania partycji i replik
Usługi w usłudze Azure Search początkowo jest przydzielany minimalny poziom zasobów składające się z jednej partycji i replik jednej. Dla warstwy, które obsługują tę przyrostowo można dostosować zasoby obliczeniowe, zwiększając partycji, jeśli wymagają więcej pamięci masowej i we/wy, lub Dodaj więcej replik większych woluminów zapytania lub lepszą wydajność. Usługa rejestracji musi mieć wystarczające zasoby do obsługi wszystkich obciążeń (indeksowanie i kwerendy). Nie można podzielić obciążenia między wieloma usługami.

Aby zwiększyć lub zmienić alokację replik i partycji, zaleca się korzystanie z portalu Azure. Portalu wymusza limity dopuszczalny kombinacje, które pozostają poniżej maksymalny limit:

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) i wybierz usługę wyszukiwania.
2. W **ustawienia**, otwórz **skali** bloku i zwiększyć lub zmniejszyć liczbę partycji i replik za pomocą suwaków.

Jeśli potrzebujesz opartych na skryptach lub oparte na kodzie metody inicjowania obsługi administracyjnej, [interfejsu API REST zarządzania](https://msdn.microsoft.com/library/azure/dn832687.aspx) stanowi alternatywę do portalu.

Ogólnie rzecz biorąc wyszukiwanie aplikacji muszą replik więcej niż partycje, szczególnie w przypadku, gdy operacje usług są ukierunkowane kierunku obciążeń zapytania. Sekcję [wysokiej dostępności](#HA) zawiera informacje o przyczynie.

> [!NOTE]
> Po zainicjowaniu obsługi usługi, nie można uaktualnić do nowszej wersji produktu. Konieczne będzie Utwórz usługę wyszukiwania na nową warstwę i ponownie załaduj z indeksów. Zobacz [Tworzenie usługi Azure Search w portalu](search-create-service-portal.md) Aby uzyskać pomoc dotyczącą usługi udostępniania.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Wysoka dostępność
Ponieważ jest stosunkowo szybkie skalowanie w górę, ogólnie zalecamy czy uruchomić jedną partycję oraz jedną lub dwie repliki, a następnie skalowania w górę jako woluminy zapytania kompilacji. Dla wielu usług w warstwach Basic lub S1 jedną partycję miejsce wystarczające magazynu i operacji We/Wy (co 15 milionów dokumentów na partycję).

Uruchom zapytanie obciążeń głównie w replikach. Jeśli potrzebujesz więcej przepustowości lub wysoką dostępność, prawdopodobnie będzie wymagać dodatkowych replik.

Ogólne zalecenia dotyczące wysokiej dostępności są:

* Dwie repliki wysokiej dostępności obciążeń tylko do odczytu (queries)
* Co najmniej trzech replik wysokiej dostępności obciążeń odczytu/zapisu (kwerend oraz indeksowania podczas dodawania, aktualizacji lub usuwania pojedyncze dokumenty)

Umowy dotyczące poziomu usług (SLA) dla usługi wyszukiwanie Azure są przeznaczone dla operacji zapytań i indeksu aktualizacje, które składają się z Dodawanie, aktualizowanie lub usuwanie dokumentów.

### <a name="index-availability-during-a-rebuild"></a>Indeks dostępności podczas kompilowania

Wysoka dostępność dla usługi Azure Search odnosi się do zapytań i indeksu aktualizacji, które nie obejmują odbudowanie indeksu. Usuwanie pola, Zmień typ danych lub zmień nazwę pola, należy odbudować indeksu. Odbudowanie indeksu, należy usunąć indeksu, ponownie utwórz indeks i załaduj ponownie dane.

> [!NOTE]
> Możesz dodać nowe pola do indeksu usługi Azure Search, bez odbudowanie indeksu. Wartość nowego pola będzie równy null dla wszystkich dokumentów już w indeksie.

Do obsługi indeksu dostępności podczas kompilowania, musi mieć kopię indeksu z inną nazwą w tej samej usługi lub kopię indeks o takiej samej nazwie w innej usługi, a następnie podaj przekierowania lub logiki pracy awaryjnej w kodzie.

## <a name="disaster-recovery"></a>Odzyskiwanie po awarii
Obecnie nie istnieje wbudowany mechanizm odzyskiwania po awarii. Dodawanie repliki lub partycje będą w niewłaściwej strategii realizacji celów odzyskiwania po awarii. Najbardziej typowym podejściem jest dodawanie nadmiarowości na poziomie usługi poprzez ustawienie drugi Usługa wyszukiwania w innym regionie. Podobnie jak w przypadku dostępności podczas odbudowywania indeksu, przekierowywanie lub logiki trybu failover muszą pochodzić z kodu.

## <a name="increase-query-performance-with-replicas"></a>Zwiększ wydajność zapytań z replikami
Opóźnienie kwerendy jest wskaźnik potrzebne są dodatkowe repliki. Ogólnie rzecz biorąc pierwszym krokiem procesu poprawę wydajności zapytań jest dodanie więcej zasobów. Podczas dodawania replik dodatkowych kopii indeksu są przełączony w tryb online do obsługi większych obciążeń zapytania i załadować równoważenie żądań przez wiele replik.

Nie możemy przekazać twardych szacuje na zapytania na sekundę (QPS): zapytanie wydajność zależy od złożoności zapytania i konkurencyjnych obciążeń. Średnio repliki na podstawowym lub jednostki SKU S1 może obsłużyć QPS około 15, ale przepustowość sieci jest wyższe lub niższe w zależności od złożoności kwerendy (aspektowej zapytania są bardziej złożonych) i opóźnienie sieci. Ponadto ważne jest, aby rozpoznać, że chociaż dodanie replik ostatecznie spowoduje dodanie skalowalność i wydajność, wynik nie jest ściśle liniowa: dodanie trzy repliki nie gwarantuje Potrójna przepływności.

Aby dowiedzieć się więcej o QPS podejścia do oszacowania QPS dla obciążeń, w tym temacie [Zarządzanie usługą wyszukiwania](search-manage.md).

## <a name="increase-indexing-performance-with-partitions"></a>Zwiększ wydajność indeksowania partycji
Aplikacji wyszukiwania, które wymagają niemal odświeżanie danych w czasie rzeczywistym, należy proporcjonalnie więcej partycji niż replik. Dodawanie partycji rozprzestrzenia operacji odczytu/zapisu w większej liczby zasobów obliczeniowych. Również daje więcej miejsca na dysku do przechowywania dodatkowych indeksów oraz dokumentów.

Indeksy większe wydłużyć czas zapytania. Tak może się okazać, że każdy dodatkowe zwiększenie partycji wymaga mniejsze, ale proporcjonalny wzrost replik. Złożoność zapytania i zapytania woluminu zostanie uwzględnić szybkość wykonywania zapytania jest włączona.

## <a name="basic-tier-partition-and-replica-combinations"></a>Warstwa podstawowa: kombinacji partycji i replik
Podstawowe usługi można mieć dokładnie jedną partycję i maksymalnie trzech replik, maksymalnie ograniczyć trzech serwerów SUS. Tylko regulowany zasób jest replik. Należy co najmniej dwóch replik wysokiej dostępności na kwerendach.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standardowa warstw: kombinacji partycji i replik
W poniższej tabeli przedstawiono SUs wymagane do obsługi kombinacje repliki i partycji, z uwzględnieniem limitu 36 SU dla wszystkie warstwy standardowa.

|   | **1 partycji** | **partycje 2** | **3 partycji** | **partycje 4** | **partycje 6** | **partycje 12** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 repliki** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 replik** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **repliki 3** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 replik** |4 SU |8 SU |12 SU |16 SU |24 SU |Nie dotyczy |
| **5 repliki** |5 SU |10 SU |15 SU |20 SU |30 SU |Nie dotyczy |
| **6 repliki** |6 SU |12 SU |18 SU |24 SU |36 SU |Nie dotyczy |
| **12 repliki** |12 SU |24 SU |36 SU |Nie dotyczy |Nie dotyczy |Nie dotyczy |

SUs, ceny i pojemność są szczegółowo opisane w witrynie sieci Web platformy Azure. Aby uzyskać więcej informacji, zobacz [szczegóły cennika](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Liczba replik partycji dzieli bez reszty do 12 (w szczególności 1, 2, 3, 4, 6, 12). Jest to spowodowane usługi Azure Search wstępnie dzieli każdy indeks na odłamków 12, dzięki czemu mogą być rozkładu w równe części wszystkich partycji. Na przykład jeśli usługa ma trzy partycje, Utwórz indeks każda partycja zawiera cztery odłamków indeksu. Jak odłamków usługi Azure Search jako indeks jest szczegółów implementacji, która może ulec zmianie w przyszłych wersjach. Mimo że numer 12 dzisiaj, nie należy spodziewać się liczby zawsze można w przyszłości 12.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>Formuła rozliczeń dla repliki i partycji zasobów
Obliczanie liczby SUs są używane dla określonej kombinacji jest iloczyn repliki i partycji, lub (R X P = SU). Na przykład trzech replik pomnożona przez trzy partycje jest on rozliczany jako dziewięć SUs.

Koszt SU jest określany przez warstwę, o mniejszej szybkości rozliczeń jednostkę Basic niż dla standardowej. Ocenia dla każdej warstwy można znaleźć na [szczegóły cennika](https://azure.microsoft.com/pricing/details/search/).
