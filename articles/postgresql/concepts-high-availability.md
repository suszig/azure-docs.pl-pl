---
title: "Pojęcia dotyczące wysokiej dostępności w bazie danych Azure PostgreSQL | Dokumentacja firmy Microsoft"
description: "Ten temat zawiera informacje o wysokiej dostępności, korzystając z bazy danych Azure PostgreSQL"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 315eb32a293eec89b274ccd017dba506241533e6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/21/2017
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>Pojęcia dotyczące wysokiej dostępności w bazie danych PostgreSQL Azure
Baza danych Azure usługi PostgreSQL zapewnia gwarantowane wysoki poziom dostępności. Finansowo kopii umowy dotyczącej poziomu usług (SLA) jest 99,99% po ogólnej dostępności. Nie ma praktycznie żadnej aplikacji czas przestoju podczas korzystania z tej usługi.

## <a name="high-availability"></a>Wysoka dostępność
Model o wysokiej dostępności (HA) opiera się na wbudowane mechanizmy awaryjnej po wystąpieniu przerwy poziomu węzła. Przerwanie węzeł na poziomie może wystąpić z powodu awarii sprzętu lub w odpowiedzi na wdrożenie usługi.

Przez cały czas zmiany wprowadzone w PostgreSQL występuje w kontekście transakcji. Zmiany są rejestrowane w synchronicznie w magazynie Azure, gdy transakcja została przekazana. W przypadku przerwania węzeł na poziomie serwera bazy danych automatycznie tworzy nowy węzeł i dołącza magazynu danych do nowego węzła. Wszystkie aktywne połączenia są usuwane i wszystkich transakcji porządkowych nie są przekazywane.

## <a name="application-retry-logic-is-essential"></a>Ważne jest Logika ponawiania aplikacji
Jest PostgreSQL aplikacje baz danych są przeznaczone do wykrywania i spróbuj ponownie usunąć połączenia i nie powiodło się transakcji. Podczas prób aplikacji, połączenie aplikacji nastąpi niewidocznie przekierowanie do nowo utworzyć wystąpienia, które ma dla tego wystąpienia nie powiodło się.

Na platformie Azure, brama jest używana wewnętrznie przekierowywanie połączeń do nowego wystąpienia. Po przerwaniu cały proces awaryjnej ma zazwyczaj dziesiątki sekund. Parametry połączenia zewnętrzne jest taka sama dla aplikacji klienckich.

## <a name="scaling-up-or-down"></a>Skalowanie w górę lub w dół
Podobnie jak w modelu wysokiej dostępności bazy danych Azure programu PostgreSQL skalowania w górę lub w dół, nowe wystąpienie serwera o określonym rozmiarze jest tworzony. Istniejący magazyn danych jest odłączona od oryginalnego wystąpienia i dołączona do nowego wystąpienia.

Podczas operacji skalowania odbywa się zakłócenia połączenia bazy danych. Aplikacje klienckie są odłączone i otwórz niezatwierdzone transakcje są anulowane. Gdy aplikacja kliencka ponowi próbę połączenia lub tworzy nowe połączenie, brama kieruje połączenia z wystąpieniem o nowym rozmiarze. 

## <a name="next-steps"></a>Następne kroki
- Aby zapoznać się z omówieniem usługi, zobacz [Azure bazy danych PostgreSQL — omówienie](overview.md)
