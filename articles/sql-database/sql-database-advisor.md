---
title: "Zalecenia dotyczące wydajności — baza danych SQL Azure | Dokumentacja firmy Microsoft"
description: "Baza danych SQL Azure zawiera zalecenia dotyczące bazy danych SQL, które może poprawić wydajność kwerend bieżącej."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 09/20/2017
ms.author: sstein
ms.openlocfilehash: 84706837aeb416d13dab617f51a33d62a934c016
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="performance-recommendations"></a>Zalecenia dotyczące wydajności

Baza danych SQL Azure uzyskuje informacje o dostosowuje się z aplikacją i zawiera dostosowany zalecenia co pozwala zmaksymalizować wydajność bazy danych SQL. Wydajność jest ciągle oceniane analizując Twojej historii użycia bazy danych SQL. Zalecenia, które są dostarczane są oparte na wzorzec obciążenia unikatowy bazy danych i zwiększyć jej wydajność.

> [!TIP]
> [Automatycznego dostrajania](sql-database-automatic-tuning.md) jest zalecanym sposobem dostrajania wydajności. [Inteligentnego Insights](sql-database-intelligent-insights.md) jest zalecanym sposobem monitorowania wydajności. 
>

## <a name="create-index-recommendations"></a>Utwórz zalecenia dotyczące indeksu
Baza danych SQL Azure stale monitoruje zapytań, które było wykonywane i identyfikuje indeksów, które może poprawić wydajność. Po utworzeniu za mało pewność, że indeks niektórych brakuje, nowy **Utwórz indeks** zalecenie zostanie utworzony. Baza danych SQL Azure tworzy zaufania szacowaną bardziej wydajne, który indeks doprowadzi do czasu. W zależności od szacunkowego wzrostu wydajności zalecenia są sklasyfikowane jako wysoki, średni lub niski. 

Utworzone za pomocą zalecenia dotyczące indeksów są zawsze oznaczonej jako auto_created indeksów. Można sprawdzić, które indeksy są auto_created analizując sys.indexes widoku. Automatycznie utworzone indeksy nie blokować polecenia ALTER/Zmień nazwę. Jeśli spróbujesz porzucić tę kolumnę, który został automatycznie utworzony indeks nad nim przekazuje polecenia i automatycznie utworzony indeks zostało porzucone z poleceniem również. Zwykłych indeksów uniemożliwiają polecenia ALTER/Zmień nazwę kolumny, które są indeksowane.

Po zastosowaniu zalecenie dotyczące indeksu tworzenie bazy danych SQL Azure zostanie porównany wydajność kwerend linię bazową wydajności. Jeśli nowy indeks, sprowadzonych ulepszenia wydajności, zalecenie zostanie oznaczone jako pomyślnie i wpływ raport będzie dostępny. W przypadku, gdy indeks nie Przełącz korzyści, zostanie automatycznie przywrócony. Dzięki temu bazy danych SQL Azure zapewnia, że za pomocą zalecenia tylko zwiększy wydajność bazy danych.

Wszelkie **Utwórz indeks** zalecenie ma wycofywania zasad, które nie pozwalają na stosowanie zalecenie, jeśli użycie jednostek dtu w warstwie bazy danych lub puli nie przekracza 80% w ostatnich 20 minut lub jeśli magazyn jest ponad 90% użycia. W takim przypadku zostanie przełożone zalecenia.

## <a name="drop-index-recommendations"></a>Zalecenia dotyczące usuwania indeksów
Oprócz wykrywania brakuje indeksu, bazy danych SQL Azure stale analizuje wydajność istniejące indeksy. Jeśli indeks nie jest używany, baza danych SQL Azure zaleci usunięcie go. W obu przypadkach zaleca się porzucenie indeksu:
* Indeks jest duplikat nazwy innego indeksu (sama indeksowane i zawiera kolumny, schemat partycji i filtrów)
* Indeks nie jest używana przez dłuższy okres (93 dni)

Zalecenia dotyczące usuwania indeksów także przejść do weryfikacji po implementacji. Jeśli wydajność jest zwiększona raport wpływ będą dostępne. W przypadku wykrycia obniżenia wydajności zalecenie zostaną cofnięte.


## <a name="parameterize-queries-recommendations"></a>Parametryzacja zapytań zalecenia
**Parametryzacja zapytań** zalecenia są wyświetlane, jeśli masz co najmniej jednego zapytania, które są stale jest ponownie kompilowana ale zakończenia się z tego samego planu wykonania zapytania. Ten warunek otwiera możliwość zastosowania parametryzacja wymuszone, co pozwoli plany zapytań w pamięci podręcznej i użyć ponownie w przyszłości poprawy wydajności i zmniejsza zużycie zasobów. 

Każdej kwerendy początkowo wystawiony na podstawie programu SQL Server musi być skompilowany do wygenerowania planu wykonywania. Każdy plan wygenerowany jest dodawana do pamięci podręcznej planu, a podczas kolejnych wykonań kodu tego samego zapytania można ponownie użyć tego planu z pamięci podręcznej, eliminując konieczność dodatkowe kompilacji. 

Aplikacje wysyłające zapytania, które obejmują wartości bez parametrów, może spowodować zmniejszenie wydajności, którym dla każdego takie zapytanie z innym parametrem wartości planu wykonania jest skompilowane ponownie. W wielu przypadkach tego samego zapytania z innym parametrem wartości Generowanie tego samego planu wykonywania, ale te plany nadal oddzielnie są dodawane do pamięci podręcznej planu. Planów wykonania ponownej kompilacji korzystanie z zasobów bazy danych, zwiększyć czas trwania zapytania i przepełnienie pamięci podręcznej planu powoduje planuje można wykluczyć z pamięci podręcznej. Może się zmienić to zachowanie programu SQL Server przez ustawienie opcji parametryzacja wymuszone w bazie danych. 

Ułatwić oszacowanie wpływu tego zalecenia, można uzyskać z porównanie rzeczywistego użycia procesora CPU i projektowane użycie Procesora (tak, jakby zalecenie została zastosowana). Oprócz zużycie procesora CPU czasu trwania kwerendy zmniejsza czas spędzony w kompilacji. Również będzie znacznie mniejszy narzut na pamięci podręcznej planu, dzięki czemu większość planów pozostanie w pamięci podręcznej i można użyć ponownie. Można zastosować zalecenie to szybkie i łatwe, klikając **Zastosuj** polecenia. 

Gdy zastosujesz zalecenie spowoduje włączenie parametryzacja wymuszone w ciągu minut bazy danych i rozpoczyna proces monitorowania, które trwa około 24 godzin. Po upływie tego czasu można wyświetlić raportu weryfikacji, który pokazuje użycie procesora CPU bazy danych, 24 godzin przed i po zastosowaniu zalecenia. Doradca bazy danych programu SQL ma mechanizm bezpieczeństwa, który automatycznie przywrócona zalecenie zastosowane w przypadku, gdy wykryto regresji wydajności.

## <a name="fix-schema-issues-recommendations-preview"></a>Usuń zalecenia dotyczące problemów schematu (wersja zapoznawcza)

> [!IMPORTANT]
> Microsoft trwa wycofano "Napraw problem schematu" zalecenia. Należy zacząć korzystać [Insights inteligentnego](sql-database-intelligent-insights.md) automatyczne monitorowania sieci problemy z wydajnością bazy danych, które obejmują schemat problemy, które poprzednio objęte zalecenia "Napraw problem schematu".
> 

**Rozwiązywanie problemów schematu** zalecenia są wyświetlane po usługi SQL Database powiadomienia anomalii w liczba błędów SQL dotyczące schematu wykonywanych na bazie danych SQL Azure. To zalecenie jest zwykle wyświetlany, gdy bazy danych wystąpi wiele błędów związanych z schematu (Nieprawidłowa nazwa kolumny, nieprawidłowa nazwa obiektu itp.) w ciągu godziny.

"Problemy z schematu" jest klasą błędy składniowe w programie SQL Server, które się zdarzyć, gdy nie są wyrównane definicji zapytania SQL, jak i definicja schematu bazy danych. Na przykład jedna z kolumn oczekiwany przez zapytanie może brakować w tabeli docelowej lub na odwrót. 

"Napraw problem schematu" zalecenie jest wyświetlany, gdy usługi baza danych SQL Azure powiadomienia anomalii w liczba błędów SQL dotyczące schematu wykonywanych na bazie danych SQL Azure. W poniższej tabeli przedstawiono błędów, które dotyczą problemów schematu:

| Kod błędu SQL | Komunikat |
| --- | --- |
| 201 |Procedura lub funkcja "*"oczekuje parametru"*", który nie został dostarczony. |
| 207 |Nieprawidłowa nazwa kolumny ' *'. |
| 208 |Nieprawidłowa nazwa obiektu "*". |
| 213 |Nazwa kolumny lub liczba podanych wartości nie odpowiada definicji tabeli. |
| 2812 |Nie można odnaleźć procedury składowanej "*". |
| 8144 |Procedura lub funkcja * ma określono zbyt wiele argumentów. |

## <a name="next-steps"></a>Następne kroki
Monitorowanie zalecenia i zastosować je do ograniczania wydajności w dalszym ciągu. Obciążeń bazy danych są dynamiczne i zmienianie w sposób ciągły. Doradca bazy danych SQL kontynuuje monitorowanie i podano zalecenia, które może potencjalnie podnieść wydajność bazy danych. 

* Zobacz [automatycznego dostrajania bazy danych SQL Azure](sql-database-automatic-tuning.md) automatycznego dostrajania indeksy bazy danych i planów wykonywania zapytania.
* Zobacz [Insights inteligentnego SQL Azure](sql-database-intelligent-insights.md) automatycznie monitorowania wydajności bazy danych przy użyciu automatycznego diagnostyki i głównego przyczynę problemu, problemy z wydajnością.
* Zobacz [zalecenia dotyczące wydajności w portalu Azure](sql-database-advisor-portal.md) instrukcje dotyczące sposobu używania zalecenia dotyczące wydajności w portalu Azure.
* Zobacz [szczegółowe informacje o wydajności zapytań](sql-database-query-performance.md) poznać i wyświetlić jego wpływ na wydajność kwerend top.


