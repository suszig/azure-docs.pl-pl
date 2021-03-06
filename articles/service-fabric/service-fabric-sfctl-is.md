---
title: Azure Service Fabric interfejsu wiersza polecenia - sfctl jest | Dokumentacja firmy Microsoft
description: "Zawiera opis usługi sieci szkieletowej interfejsu wiersza polecenia sfctl jest poleceń."
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
ms.openlocfilehash: d3a4c7a308bdf7132cebffc13a5e7214ec73eaf2
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-is"></a>sfctl is
Zapytania i wysyłać polecenia do usługi infrastruktury.

## <a name="commands"></a>Polecenia

|Polecenie|Opis|
| --- | --- |
|    polecenie| Wywołuje polecenie administracyjne dla danego wystąpienia usługi infrastruktury.|
|    query  | Wywołuje zapytanie tylko do odczytu w wystąpieniu usługi danej infrastruktury.|


## <a name="sfctl-is-command"></a>polecenie jest sfctl
Wywołuje polecenie administracyjne dla danego wystąpienia usługi infrastruktury.

W przypadku klastrów ma co najmniej jedno wystąpienie usługi infrastruktury skonfigurowane ten interfejs API umożliwia wysyłanie poleceń specyficznych dla infrastruktury do konkretnego wystąpienia usługi infrastruktury. Dostępnych poleceń i ich odpowiednich formatów odpowiedzi zależy od infrastruktury, na którym działa klastra. Ten interfejs API obsługuje platformy Service Fabric; nie jest przeznaczona do użycia bezpośrednio w kodzie. 

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — polecenie [wymagane]| Tekst polecenia do wywołania. Zawartość polecenia jest specyficzne dla infrastruktury. |
| --service-id     | Tożsamość usługi infrastruktury. Identyfikator jest pełna nazwa usługi infrastruktury bez "fabric:" schemat identyfikatora URI. Ten parametr jest wymagany tylko w przypadku klastrów, które mają więcej niż jedno wystąpienie usługi infrastruktury uruchomiona.|
| limit czasu — -t     | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug          | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h        | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o      | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania          | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/.|
| -verbose        | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-is-query"></a>sfctl jest zapytania
Wywołuje zapytanie tylko do odczytu w wystąpieniu usługi danej infrastruktury.

W przypadku klastrów ma co najmniej jedno wystąpienie usługi infrastruktury skonfigurowany ten interfejs API umożliwia wysyłanie zapytań specyficzne dla infrastruktury do konkretnego wystąpienia usługi infrastruktury. Dostępnych poleceń i ich odpowiednich formatów odpowiedzi zależy od infrastruktury, na którym działa klastra. Ten interfejs API obsługuje platformy Service Fabric; nie jest przeznaczona do użycia bezpośrednio w kodzie.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| — polecenie [wymagane]| Tekst polecenia do wywołania. Zawartość polecenia jest specyficzne dla infrastruktury.|
| --service-id     | Tożsamość usługi infrastruktury. Identyfikator jest pełna nazwa usługi infrastruktury bez "fabric:" schemat identyfikatora URI. Ten parametr jest wymagany tylko w przypadku klastrów, które mają więcej niż jedno wystąpienie usługi infrastruktury uruchomiona.|
| limit czasu — -t     | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug          | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h        | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o      | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania          | Ciąg zapytania JMESPath. Aby uzyskać więcej informacji zobacz http://jmespath.org/.|
| -verbose        | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="next-steps"></a>Kolejne kroki
- [Konfigurowanie](service-fabric-cli.md) interfejsu wiersza polecenia usługi sieci szkieletowej.
- Dowiedz się, jak używać przy użyciu interfejsu wiersza polecenia usługi sieć szkieletowa [przykładowe skrypty](/azure/service-fabric/scripts/sfctl-upgrade-application).