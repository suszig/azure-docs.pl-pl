---
title: Azure Service Fabric interfejsu wiersza polecenia sfctl magazynu | Dokumentacja firmy Microsoft
description: "Zawiera opis poleceń interfejsu wiersza polecenia usługi sieć szkieletowa sfctl magazynu."
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
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: d4ca3c35c34736c3b4824f956a6a72002c891877
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-store"></a>sfctl magazynu
Wykonywanie operacji poziomu podstawowego pliku magazynu obrazu klastra.

## <a name="commands"></a>Polecenia

|Polecenie|Opis|
| --- | --- |
|    usuwanie| Usuwa istniejące obrazu przechowywania zawartości.|
|    root-info| Pobiera informacje o zawartości w katalogu głównym magazynu obrazów.|
|    stat  | Pobiera informacje o zawartości magazynu obrazów.|


## <a name="sfctl-store-delete"></a>Usuń magazyn sfctl
Usuwa istniejące obrazu przechowywania zawartości.

Usuwa istniejące zawartości magazynu obrazu zostanie znaleziony w ciągu danego obrazu przechowywać ścieżki względnej. To polecenie umożliwia usunięcie pakietów przekazano aplikacji po ich udostępnieniu.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| --zawartości path [wymagane]| Ścieżka względna do pliku lub folderu w magazynie obrazu z elementem głównym.|
| limit czasu — -t          | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug               | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h             | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o           | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania               | Ciąg zapytania JMESPath. Aby uzyskać dodatkowe informacje i przykłady zobacz http://jmespath.org/.|
| -verbose             | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="sfctl-store-stat"></a>stat magazynu sfctl
Pobiera informacje o zawartości magazynu obrazów.

Zwraca informacje o zawartości magazynu obrazu w określonym contentPath względem katalogu głównego magazynu obrazów.

### <a name="arguments"></a>Argumenty

|Argument|Opis|
| --- | --- |
| --zawartości path [wymagane]| Ścieżka względna do pliku lub folderu w magazynie obrazu z elementem głównym.|
| limit czasu — -t          | W sekundach limit czasu serwera.  Domyślnie: 60.|

### <a name="global-arguments"></a>Argumenty globalne

|Argument|Opis|
| --- | --- |
| --debug               | Zwiększenie szczegółowości rejestrowania, aby pokazać wszystkie debugowania dzienniki.|
| — Pomoc -h             | Pokaż ten komunikat pomocy i Zakończ.|
| --output -o           | Format danych wyjściowych.  Dozwolone wartości: json, jsonc, tabeli, tsv.  Domyślne: json.|
| — zapytania               | Ciąg zapytania JMESPath. Zobacz http://jmespath.org/ dodatkowe informacje i przykłady.|
| -verbose             | Zwiększ poziom szczegółowości rejestrowania. Użycie--debugowania dla dzienników debugowania pełna.|

## <a name="next-steps"></a>Kolejne kroki
- [Instalator](service-fabric-cli.md) sieci szkieletowej usług interfejsu wiersza polecenia.
- Dowiedz się, jak używać przy użyciu interfejsu wiersza polecenia usługi sieć szkieletowa [przykładowe skrypty](/azure/service-fabric/scripts/sfctl-upgrade-application).