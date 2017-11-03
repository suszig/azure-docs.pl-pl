---
title: "Przegląd interfejsu API usługi Mobile Engagement eksportu"
description: "Poznaj podstawy eksportowanie danych pierwotnych generowany przez urządzenia użytkownika, aby wykorzystać go w własnych narzędzi — informacje"
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: 
ms.assetid: 9380d47b-d7fa-4d4c-888f-97e6482196bb
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kapiteir
ms.openlocfilehash: 346e0e480ff84ee849f135a7605d27df9e32f966
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="mobile-engagement-export-api-overview"></a>Przegląd interfejsu API usługi Mobile Engagement eksportu
## <a name="introduction"></a>Wprowadzenie
W tym dokumencie dowiesz się podstawowe informacje o eksportowaniu danych pierwotnych generowany przez urządzenia użytkownika, aby wykorzystać go w własnych narzędzi.

## <a name="pre-requisites"></a>Wymagania wstępne
Eksportowanie danych pierwotnych z usługi Mobile Engagement wymaga:

* Konfigurowanie uwierzytelniania interfejsu API umożliwia korzystanie z interfejsów API (zobacz [instalacji ręcznej uwierzytelniania](mobile-engagement-api-authentication-manual.md)),
* Użyj API REST lub [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
* Konto magazynu Azure.

> [!NOTE]
> Również zaleca znakomity [Eksploratora usługi Microsoft Azure Storage](http://storageexplorer.com/), co najmniej w fazie projektowania, ponieważ zapewnia łatwy w użyciu interfejsu użytkownika do interakcji z usługą Azure Storage.
> 
> 

## <a name="what-can-be-exported"></a>Co można eksportować?
Usługa Mobile Engagement umożliwia użytkownikom zebrać wiele typów danych i w związku z tym obejmuje kilka typów eksportu nadaje się do tych różnych typów danych.
Istnieją 2 podstawowych typów eksportu:

* Migawki: używane zwykle do eksportowania danych, który reprezentuje stan i dla którego usługa Mobile Engagement ma historii. W tym na przykład tag (app-info), tokeny lub wypychanej kampanii opinie. W rezultacie tych typów eksportu nie są związane z datą.
* historyczne: ten typ eksportu jest używany dla danych, które akumuluje wraz z upływem czasu, takich jak działania lub zdarzenia, na przykład.

W poniższej tabeli opisano wyczerpujący wszystkie możliwe eksportami:

| Typ eksportu | Typ danych | Opis |
| --- | --- | --- |
| Snapshot |Wypychanie |Generuje eksportu kampanie wypychania opinie na podstawie na deviceid/nazwa użytkownika |
| Snapshot |Tag |Generuje eksportu tag (app-info) skojarzonego z każdego urządzenia |
| Snapshot |Urządzenie |Generuje eksportu większość danych dotyczących urządzeń, takich jak technicals (modelu, ustawienia regionalne, strefa czasowa,...), tagi, po raz pierwszy dostępny... |
| Snapshot |Token |Generuje eksportu prawidłowych tokenów |
| Historyczne |Działanie |Generuje eksportu wszystkie operacje dla każdego urządzenia w danym okresie czasu |
| Historyczne |Wydarzenie |Generuje eksportu wszystkie operacje dla każdego urządzenia w danym okresie czasu |
| Historyczne |Zadanie |Generuje eksportu wszystkie zadania dla każdego urządzenia w danym okresie czasu |
| Historyczne |Błąd |Generuje eksportu wszystkich błędów dla każdego urządzenia w danym okresie czasu |

## <a name="how-does-it-work"></a>Jak to działa?
Eksportowanie jest długa uruchomionych zadań, które mogą być bardzo duże pliki danych. Z tego powodu nie można wywołać do zwrócenia natychmiast pliku do pobrania.
Aby wyeksportować dane z usługi Mobile Engagement, musisz utworzyć **zadania eksportu** za pomocą interfejsu API, które zazwyczaj określić:

* Typ eksportu (migawki lub historycznych)
* Typ danych
* **Kontenera magazynu Azure** (w tym prawidłowe sygnatury dostępu Współdzielonego z uprawnieniami do zapisu) którym zostanie zapisany wynik eksportowania.
* np. parametr adresu URL kontenera przykład byłoby https://[StorageAccountName].blob.core.windows.net/[ContainerName]? [SASWritePermissionsToken]  

Oto przykład rzeczywistych. https://testazmeexport.blob.Core.Windows.NET/test1234azme?SV=2015-12-11&ss=b&SRT=SCO&SP=rwdlac&SE=2016-12-17T04:59:26Z & st = 2016-12-16T20:59:26Z & spr = https & sig = KRF3aVWjp2NEJDzjlmoplmu0M9HHlLdkBWRPAFmw90Q % 3D

Należy pamiętać, że może potrwać kilka minut, aż zadanie można uruchomić, a następnie może uruchomić z niewielki rozmiar aplikacji kilka sekund do kilku godzin dla aplikacji z wielu użytkowników lub działania.

Po utworzeniu zadania jest możliwe sprawdzenie jego stan, aby zobaczyć, czy jest nadal uruchomiona, czy została ukończona.

Gdy zadanie zakończyło się pomyślnie, wynikowy plik danych jest dostępna na kontenerze magazynu udostępnionego.

