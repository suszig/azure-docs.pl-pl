---
title: Azure Service Fabric interfejsu wiersza polecenia - sfctl choas | Dokumentacja firmy Microsoft
description: "Informacje dotyczące poleceń chaos sfctl interfejsu wiersza polecenia usługi sieci szkieletowej."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: 336e74d8f69cb04e6bd0e85fc68ba38b218fabae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-chaos"></a>sfctl chaos
Uruchom, Zatrzymaj, a raport dotyczący usługi testu chaos.

## <a name="commands"></a>Polecenia

|Polecenie|Opis|
| --- | --- |
|    raport| Pobiera następny segment Chaos raportu na podstawie token kontynuacji przekazany lub przekazany w czasie zakresie.|
|    rozpoczynanie | Jeśli Chaos nie jest już uruchomiona w klastrze, zaczyna działać Chaos z określonym w parametrach Chaos.|
|    Zatrzymaj  | Zatrzymuje Chaos w klastrze, jeśli jest już uruchomiona, w przeciwnym razie go nie działa.|


## <a name="sfctl-chaos-report"></a>sfctl chaos raportu
Pobiera następny segment Chaos raportu na podstawie token kontynuacji przekazany lub przekazany w czasie zakresie.

Można określić ContinuationToken uzyskać następny segment raportu Chaos lub można określić zakres czasu za pośrednictwem StartTimeUtc i EndTimeUtc, ale nie można określić zarówno ContinuationToken, jak i zakres czasu, w tym samym wywołaniu. W przypadku więcej niż 100 zdarzeń Chaos raportu Chaos jest zwracany w segmentów gdy segment zawiera nie więcej niż 100 Chaos zdarzeń. 

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| --token kontynuacji| Parametr token kontynuacji służy do uzyskiwania następnej zestaw wyników. Token kontynuacji z wartość pusta jest uwzględniana w odpowiedzi interfejsu API wyników z systemu nie mieszczą się w jednej odpowiedzi. Jeśli ta wartość jest przekazywany do następnego wywołania interfejsu API interfejsu API zwraca następny zestaw wyników. Jeśli nie są dalsze wyniki token kontynuacji nie zawiera wartości. Wartość tego parametru nie powinny być zakodowane w adresie URL.|
| --Godzina zakończenia utc   | Liczba reprezentującą godzinę zakończenia przedziału czasu, dla którego ma być generowany raport Chaos znaczniki osi. Przejrzyj [właściwości DateTime.Ticks](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) szczegółowe informacje na temat znaczników.|
| --utc-start-time | Liczba reprezentującą godzinę rozpoczęcia przedziału czasu, dla którego ma być generowany raport Chaos znaczniki osi. Przejrzyj [właściwości DateTime.Ticks](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) szczegółowe informacje na temat znaczników.|
| limit czasu — -t     | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug          | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h        | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o      | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania          | Ciąg zapytania JMESPath. Zobacz http://jmespath.org/ dodatkowe informacje i przykłady.|
| -verbose        | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-chaos-start"></a>sfctl chaos start
Jeśli Chaos nie jest już uruchomiona w klastrze, zaczyna działać Chaos z określonym w parametrach Chaos.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| --aplikacji typu kondycji zasad map  | Zakodowane JSON listy o złej kondycji aplikacji maksymalny procent dla typów określonej aplikacji. Każdy wpis określa jako klucz, nazwa typu aplikacji i jako wartość całkowitą reprezentującą procent MaxPercentUnhealthyApplications używane do analizowania aplikacji typu określonej aplikacji.|
| --Wyłącz move repliki — błędy | Wyłącza podstawowy przenoszenia i Przenieś dodatkowej błędów.|
| -max klastra stabilizacji| Maksymalna ilość czasu oczekiwania dla wszystkich klastrów jednostki do stają się stabilne i działa prawidłowo.  Domyślnie: 60.|
| -max równoczesnych — błędy    | Maksymalna liczba jednoczesnych błędów powstaniu na iterację.           Domyślne: 1.|
| -max procent — zła — aplikacje  | Podczas oceny kondycji klastra podczas Chaos, maksymalna dozwolona wartość procentowa w złej kondycji aplikacji przed zgłoszeniem błędu.|
| -max procent —-węzłów w złej kondycji | Podczas oceny kondycji klastra podczas Chaos, maksymalna dozwolona wartość procentowa węzłów w złej kondycji przed zgłoszeniem błędu.|
| — czas do uruchomienia              | Łączny czas (w sekundach), dla którego Chaos zostanie uruchomione przed zatrzymaniem automatycznie. Maksymalna dozwolona wartość to 4 294 967 295 (System.UInt32.MaxValue).  Domyślne: 4294967295.|
| limit czasu — -t               | W sekundach limit czasu serwera.  Domyślnie: 60.|
| --oczekiwania czas między błędów | Poczekaj czas (w sekundach) między kolejnych błędów w ramach pojedynczego iteracji.  Domyślne: 20.|
| --oczekiwania czas między iteracji| Czas — rozdzielenie (w sekundach) między dwoma kolejnych iteracji milczenia.  Domyślne: 30.|
| — Ostrzeżenie jako błąd         | Podczas oceny kondycji klastra podczas Chaos, Traktuj ostrzeżenia o tej samej ważność jako błędy.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug                    | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h                  | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o                | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.           Domyślne: json.|
| — zapytania                    | Ciąg zapytania JMESPath. Zobacz http://jmespath.org/ dodatkowe informacje i przykłady.|
| -verbose                  | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-chaos-stop"></a>Zatrzymaj chaos sfctl
Zatrzymuje Chaos w klastrze, jeśli jest już uruchomiona, w przeciwnym razie go nie działa.

Zatrzymuje Chaos z planowania dalsze błędy; jednak nie dotyczy locie usterek.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| limit czasu — -t| W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug  | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h| Pokaż ten komunikat pomocy i Zakończ.|
| --output -o | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania  | Ciąg zapytania JMESPath. Zobacz http://jmespath.org/ dodatkowe informacje i przykłady.|
| -verbose| Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="next-steps"></a>Następne kroki
- [Instalator](service-fabric-cli.md) sieci szkieletowej usług interfejsu wiersza polecenia.
- Dowiedz się, jak używać przy użyciu interfejsu wiersza polecenia usługi sieć szkieletowa [przykładowe skrypty](/azure/service-fabric/scripts/sfctl-upgrade-application).