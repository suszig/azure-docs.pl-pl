---
title: "Symulacja błędu podczas uzyskiwania dostępu do odczytu nadmiarowego magazynu na platformie Azure | Dokumentacja firmy Microsoft"
description: "Symulacja błędu podczas uzyskiwania dostępu do magazynu geograficznie nadmiarowego dostęp do odczytu"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/05/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 151e875bd72598b0b788d68eee7fb186fca86f46
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Symulacja błędu podczas uzyskiwania dostępu do magazynu geograficznie nadmiarowego dostęp do odczytu

W tym samouczku jest częścią dwóch serii. W tym samouczku można wstrzyknąć odpowiedzi nie powiodło się z Fiddler żądań [dostęp do odczytu z magazynu geograficznie nadmiarowego](../common/storage-redundancy.md#read-access-geo-redundant-storage) konta magazynu (RA-GRS) symulację awarii i aplikacji do odczytu z punktu końcowego dodatkowej.

![Scenariusz aplikacji](media/storage-simulate-failure-ragrs-account-app/scenario.png)

W części dwóch serii dowiesz się, jak:

> [!div class="checklist"]
> * Uruchomić i zatrzymać aplikację
> * Symulację awarii
> * Symulowanie przywracania podstawowy punkt końcowy

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* Pobierz i zainstaluj [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Do ukończenia tego samouczka, należy wykonać poprzednie samouczek magazynu: [wysokiej dostępności danych aplikacji z usługą Azure storage][previous-tutorial].

## <a name="launch-fiddler"></a>Uruchamianie narzędzia fiddler

Otwórz Fiddler, wybierz **reguły** i **dostosować zasady**.

![Dostosuj zasady Fiddler](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Fiddler ScriptEditor uruchamia przedstawiający **SampleRules.js** pliku. Ten plik jest używany do dostosowania Fiddler. Wklej następujący przykładowy kod w `OnBeforeResponse` funkcji. Nowy kod jest oznaczone jako komentarz, aby upewnić się, że logika tworzonego nie jest zaimplementowana natychmiast. Po jego zakończeniu wybierz **pliku** i **zapisać** Aby zapisać zmiany.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![Wklej reguły niestandardowe](media/storage-simulate-failure-ragrs-account-app/figure2.png)

## <a name="start-and-pause-the-application"></a>Uruchomić i zatrzymać aplikację

W programie Visual Studio, naciśnij klawisz **F5** lub wybierz **Start** można rozpocząć debugowania aplikacji. Gdy aplikacja rozpoczyna odczyt z podstawowego punktu końcowego, naciśnij klawisz **dowolny klawisz,** w oknie konsoli można wstrzymać aplikacji.

## <a name="simulate-failure"></a>Symulacja błędu

Z aplikacją wstrzymana teraz Usuń komentarz niestandardowej reguły zapisaliśmy poprzedni krok w narzędziu Fiddler. Ten kod przykładowy szuka żądania do konta magazynu RA-GRS, jeśli ścieżka zawiera nazwę obrazu, `HelloWorld`, zwraca kod odpowiedzi `503 - Service Unavailable`.

Przejdź do Fiddler i wybierz **reguły** -> **dostosować zasady...** .  Usuń komentarz poniższej wierszy, Zastąp `STORAGEACCOUNTNAME` z nazwą konta magazynu. Wybierz **pliku** -> **zapisać** Aby zapisać zmiany.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Aby wznowić aplikacji, naciśnij klawisz **dowolny klawisz,** .

Po uruchomieniu aplikacji ponownie, żądania do punktu końcowego podstawowego zacząć się nie powieść. Aplikacja próbuje ponownie połączyć się z punktem końcowym głównej 5 razy. Po próg niepowodzeń pięć prób obraz żąda od dodatkowej punktu końcowego tylko do odczytu. Po aplikacji pomyślnie pobiera obraz 20 razy z pomocniczy punkt końcowy, aplikacja próbuje połączyć się podstawowy punkt końcowy. Jeśli podstawowy punkt końcowy jest nadal jest nieosiągalny, aplikacja wznawia odczytu z punktu końcowego dodatkowej. Ten wzorzec jest [wyłącznika](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) wzorzec opisanych w poprzednich instrukcji.

![Wklej reguły niestandardowe](media/storage-simulate-failure-ragrs-account-app/figure3.png)

## <a name="simulate-primary-endpoint-restoration"></a>Symulowanie przywracania podstawowy punkt końcowy

Żądania do punktu końcowego podstawowego z Fiddler niestandardowego zestawu reguł w poprzednim kroku, zakończyć się niepowodzeniem. Aby symulować podstawowy punkt końcowy ponownie, należy usunąć logiki iniekcję `503` błędu.

Aby wstrzymać aplikacja, naciśnij klawisz **dowolny klawisz,**.

### <a name="remove-the-custom-rule"></a>Usuń niestandardową regułę

Przejdź do Fiddler i wybierz **reguły** i **dostosować zasady...** .  Komentarz dotyczący lub usuń niestandardowej logiki w `OnBeforeResponse` funkcji, pozostawiając domyślnej funkcji. Wybierz **pliku** i **zapisać** Aby zapisać zmiany.

![Usuń regułę dostosowane](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Po zakończeniu naciśnij klawisz **dowolny klawisz,** wznowić działania aplikacji. Odczyt z podstawowego punktu końcowego aplikacji będzie nadal występować, do momentu jej Trafienia odczytów 999.

![Wznów aplikacji](media/storage-simulate-failure-ragrs-account-app/figure4.png)

## <a name="next-steps"></a>Kolejne kroki

W części dwóch serii poznanie symulację awarii do testowania magazynu geograficznie nadmiarowego dostęp do odczytu, np.:

> [!div class="checklist"]
> * Uruchomić i zatrzymać aplikację
> * Symulację awarii
> * Symulowanie przywracania podstawowy punkt końcowy

Wykonaj to łącze, aby wyświetlić przykłady wbudowanych magazynu.

> [!div class="nextstepaction"]
> [Przykłady skryptów magazynu Azure](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md