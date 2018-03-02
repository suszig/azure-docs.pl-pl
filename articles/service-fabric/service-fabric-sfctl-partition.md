---
title: Azure Service Fabric interfejsu wiersza polecenia sfctl partycji | Dokumentacja firmy Microsoft
description: "Zawiera opis poleceń interfejsu wiersza polecenia usługi sieć szkieletowa sfctl partycji."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 01dd1900fe765618e5da20bd289b9c3a021ea9a3
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-partition"></a>sfctl partition
Zapytanie partycji i zarządzania nimi dla żadnej usługi.

## <a name="commands"></a>Polecenia

|Polecenie|Opis|
| --- | --- |
|    utrata danych      | Ten interfejs API powoduje utratę danych dla określonej partycji.|
|    data-loss-status  | Pobiera postęp uruchamiania przy użyciu interfejsu API StartDataLoss operacji utraty danych partycji.|
|    kondycja         | Pobiera kondycji określonej partycji usługi sieć szkieletowa usług.|
|    Informacje o           | Pobiera informacje o partycji usługi sieć szkieletowa usług.|
|    lista           | Pobiera listę partycji usługi sieć szkieletowa usług.|
|    ładowanie           | Pobiera obciążenia określonej partycji usługi sieć szkieletowa usług.|
|    Resetowanie obciążenia     | Resetuje bieżącego obciążenia partycji usługi sieć szkieletowa usług.|
|    utrata kworum    | Powoduje utratę kworum dla danej usługi stanowej partycji.|
|    stanu w przypadku utraty kworum| Pobiera postępu operacji utraty kworum na partycji uruchomiony przy użyciu interfejsu API StartQuorumLoss.|
|    Odzyskiwanie        | Wskazuje, aby klaster sieci szkieletowej usług mają podejmować próbę odzyskać określonej partycji, który jest obecnie zablokowane w wyniku utraty kworum.|
|    Odzyskaj wszystkie    | Do klastra usługi sieć szkieletowa wskazuje, czy powinien on podejmowana próba odzyskania żadnych usług (takich jak usługi systemu), które są aktualnie zatrzymane w wyniku utraty kworum.|
|    report-health  | Wysyła raport o kondycji na partycji usługi sieć szkieletowa usług.|
|    Ponowne uruchomienie        | Ten interfejs API ponownego uruchomienia niektórych lub wszystkich replik lub wystąpienia określonej partycji.|
|    stan ponownego uruchomienia | Pobiera postępu operacji PartitionRestart uruchomiony przy użyciu StartPartitionRestart.|
|    Nazwa SVC       | Pobiera nazwę usługi Service Fabric dla partycji.|


## <a name="sfctl-partition-health"></a>sfctl partycji kondycji
Pobiera kondycji określonej partycji usługi sieć szkieletowa usług.

Pobiera informacje o kondycji określonej partycji. Filtr EventsHealthStateFilter umożliwia filtrowanie zbierania zdarzeń kondycji zgłoszonych w usłudze oparte na stanie kondycji.
Filtr ReplicasHealthStateFilter umożliwia filtrowanie kolekcji obiektów ReplicaHealthState na partycji. To polecenie cmdlet zwraca błąd, jeśli określono partycję, która nie istnieje w magazynie kondycji.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — Identyfikator partycji [wymagane]| Tożsamość partycji.|
| --events-health-state-filter  | Umożliwia filtrowanie kolekcji zwracanych obiektów HealthEvent oparte na stanie kondycji. Możliwe wartości tego parametru obejmują liczbę całkowitą jednego z następujących stanów kondycji.                Zwracane są tylko zdarzenia, które są zgodne z filtrem. Wszystkie zdarzenia są używane do oceny stanu kondycji zagregowanych. Jeśli nie zostanie określona, zwracane są wszystkie wpisy. Wartości stanu są oparte na flagi wyliczenie, może to być kombinacją te wartości uzyskanych przy użyciu bitowego operatora "Lub". Na przykład jeśli podana wartość jest 6 następnie wszystkie zdarzenia o wartości atrybutu HealthState OK (2) i ostrzeżenia (4) są zwracane. -Domyślnie — wartość domyślna. Dopasowuje wszystkie właściwości HealthState. Wartość wynosi zero. -None - filtr, który nie odpowiada żadnej wartości właściwości HealthState. Używany, aby nie zwracała żadnych wyników w danej kolekcji stanów. Wartość to 1. -Ok - filtru, że dopasowań danych wejściowych o wartości atrybutu HealthState Ok. Wartość jest równa 2. — Ostrzeżenie - filtru, że dane wejściowe zgodna z atrybutem HealthState wartość ostrzeżenie. Wartość to 4. -Błąd filtru pasującego do danych wejściowych o wartości atrybutu HealthState błędu. Wartość jest 8.                -All - filtru pasującego do danych wejściowych z dowolną wartością właściwości HealthState.                Wartość jest 65535.|
|--exclude-health-statistics   | Wskazuje, czy statystyki kondycji powinny być zwracane w ramach wyniku zapytania. Wartość false, domyślnie. Statystyki zawierają liczbę elementów podrzędnych obiektów w kondycja Ok, ostrzeżeń i błędów.|
| --replicas-health-state-filter| Umożliwia filtrowanie kolekcji obiektów ReplicaHealthState na partycji. Wartość można uzyskać z bitowe operacje na elementach członkowskich HealthStateFilter lub elementy członkowskie. Zwracane są tylko replik zgodne z filtrem. Wszystkie repliki są używane do oceny stanu kondycji zagregowanych. Jeśli nie zostanie określona, zwracane są wszystkie wpisy. Wartości stanu są oparte na flagi wyliczenie, może to być kombinacją te wartości uzyskanych przy użyciu bitowego operatora "Lub". Na przykład jeśli podana wartość jest 6 następnie wszystkie zdarzenia o wartości atrybutu HealthState OK (2) i ostrzeżenia (4) są zwracane. Możliwe wartości tego parametru obejmują liczbę całkowitą jednego z następujących stanów kondycji. -Domyślnie — wartość domyślna. Dopasowuje wszystkie właściwości HealthState. Wartość wynosi zero. -None - filtr, który nie odpowiada żadnej wartości właściwości HealthState. Używany, aby nie zwracała żadnych wyników w danej kolekcji stanów. Wartość to 1. -Ok - filtru, że dopasowań danych wejściowych o wartości atrybutu HealthState Ok. Wartość jest równa 2. — Ostrzeżenie - filtru, że dane wejściowe zgodna z atrybutem HealthState wartość ostrzeżenie. Wartość to 4. -Błąd filtru pasującego do danych wejściowych o wartości atrybutu HealthState błędu. Wartość jest 8. -All - filtru pasującego do danych wejściowych z dowolną wartością właściwości HealthState. Wartość jest 65535.|
| limit czasu — -t               | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug                    | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h                  | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o                | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.                Domyślne: json.|
| — zapytania                    | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/. |
| -verbose                  | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-partition-info"></a>informacje o partycji sfctl
Pobiera informacje o partycji usługi sieć szkieletowa usług.

Punkt końcowy partycje zwraca informacje na temat określonej partycji. Odpowiedź zawiera identyfikator partycji: informacji schemat partycjonowania, kluczy obsługiwanych przez partycji, stan kondycji i inne szczegółowe informacje o partycji.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — Identyfikator partycji [wymagane]| Tożsamość partycji.|
| limit czasu — -t          | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug               | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h             | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o           | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania               | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/.|
| -verbose             | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-partition-list"></a>listy partycji sfctl
Pobiera listę partycji usługi sieć szkieletowa usług.

Pobiera listę partycji usługi sieć szkieletowa usług. S Identyfikatora partycji, informacje schemat partycjonowania, kluczy obsługiwanych przez partycji, stan kondycji i inne szczegółowe informacje o partycji.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — Identyfikator usługi [wymagane]| Tożsamość usługi. Zazwyczaj jest to pełna nazwa tej usługi bez "fabric:" schemat identyfikatora URI. Począwszy od wersji 6.0, hierarchicznych nazwy są rozdzielane "~" znaków. Na przykład, jeśli nazwa usługi jest "fabric: / myapp/app1/svc1", będzie tożsamość usługi "moja_aplikacja ~ app1 ~ svc1" w wersji 6.0 + i "myapp/app1/svc1" w poprzednich wersjach.|
| --token kontynuacji| Parametr token kontynuacji służy do uzyskiwania następnej zestaw wyników.         Token kontynuacji z wartość pusta jest uwzględniana w odpowiedzi interfejsu API wyników z systemu nie mieszczą się w jednej odpowiedzi. Jeśli ta wartość jest przekazywany do następnego wywołania interfejsu API interfejsu API zwraca następny zestaw wyników. Jeśli nie są dalsze wyniki, token kontynuacji nie zawiera wartości. Wartość tego parametru nie powinny być zakodowane w adresie URL.|
| limit czasu — -t        | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug             | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h           | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o         | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania             | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/.|
| -verbose           | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-partition-load"></a>sfctl obciążenia partycji
Pobiera obciążenia określonej partycji usługi sieć szkieletowa usług.

Zwraca informacje o określonej partycji. Odpowiedź zawiera listę informacji obciążenia. Każdy informacje obejmują Nazwa metryki ładowania, wartość oraz czas ostatniego zgłoszone w formacie UTC.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — Identyfikator partycji [wymagane]| Tożsamość partycji.|
| limit czasu — -t          | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug               | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h             | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o           | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania               | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/.|
| -verbose             | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-partition-recover"></a>Odzyskaj sfctl partycji
Wskazuje, aby klaster sieci szkieletowej usług mają podejmować próbę odzyskać określonej partycji, który jest obecnie zablokowane w wyniku utraty kworum.

Wskazuje, aby klaster sieci szkieletowej usług mają podejmować próbę odzyskać określonej partycji, który jest obecnie zablokowane w wyniku utraty kworum. Ta operacja powinna być wykonana tylko, jeśli wiadomo, że nie można odzyskać replik, które nie działają. Nieprawidłowe użycie tego interfejsu API może spowodować ryzyko utraty danych.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — Identyfikator partycji [wymagane]| Tożsamość partycji.|
| limit czasu — -t          | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug               | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h             | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o           | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania               | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/.|
| -verbose             | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-partition-restart"></a>ponowne uruchomienie partycji sfctl
Ten interfejs API ponownego uruchomienia niektórych lub wszystkich replik lub wystąpienia określonej partycji.

Ten interfejs API jest przydatna przy testowaniu trybu failover. Jeśli używane pod kątem partycji usługi bezstanowej, tryb RestartPartitionMode musi być AllReplicasOrInstances. Wywołanie interfejsu API GetPartitionRestartProgress przy użyciu tego samego OperationId uzyskać postęp.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — Identyfikator operacji [wymagane]| Identyfikator GUID identyfikujący wywołanie tego interfejsu API.  To jest przekazywany do odpowiedniego interfejsu API GetProgress.|
| — Identyfikator partycji [wymagane]| Tożsamość partycji.|
| — ponowne uruchomienie partycji — trybu [wymagane]| Opis partycji do ponownego uruchomienia.|
| — Identyfikator usługi [wymagane]| Tożsamość usługi. Zazwyczaj jest to pełna nazwa tej usługi bez "fabric:" schemat identyfikatora URI. Począwszy od wersji 6.0, hierarchicznych nazwy są rozdzielane "~" znaków. Na przykład, jeśli nazwa usługi jest "sieci szkieletowej: / myapp/app1/svc1", jest tożsamość usługi "moja_aplikacja ~ app1 ~ svc1" w wersji 6.0 + i "myapp/app1/svc1" w poprzedniej kolejnych rsions.|
| limit czasu — -t                    | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug                         | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h                       | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o                     | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.                     Domyślne: json.|
| — zapytania                         | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/.|
| -verbose                       | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="next-steps"></a>Kolejne kroki
- [Instalator](service-fabric-cli.md) sieci szkieletowej usług interfejsu wiersza polecenia.
- Dowiedz się, jak używać przy użyciu interfejsu wiersza polecenia usługi sieć szkieletowa [przykładowe skrypty](/azure/service-fabric/scripts/sfctl-upgrade-application).
