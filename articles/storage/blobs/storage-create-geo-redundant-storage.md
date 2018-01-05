---
title: "Wysokiej dostępności danych aplikacji na platformie Azure | Dokumentacja firmy Microsoft"
description: "Umożliwia dostęp do odczytu magazynu geograficznie nadmiarowego wysokiej dostępności danych aplikacji"
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
ms.date: 11/15/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 63ca91c2eadf7b003427e9716d99621fca1b1a19
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Wysokiej dostępności danych aplikacji z usługą Azure storage

W tym samouczku wchodzi w jednej serii. W tym samouczku przedstawiono sposób wysokiej dostępności danych aplikacji na platformie Azure. Po zakończeniu, masz operacji przekazywania, który pobiera obiekt blob do aplikacji konsoli .NET core [dostęp do odczytu z magazynu geograficznie nadmiarowego](../common/storage-redundancy.md#read-access-geo-redundant-storage) konta magazynu (RA-GRS). RA-GRS polega na replikowanie transakcji z serwera podstawowego w regionie pomocniczym. Ten proces replikacji gwarantuje, że dane w regionie pomocniczym jest ostatecznie spójna. Aplikacja używa [wyłącznika](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) wzorzec, aby określić, które punktu końcowego do nawiązania połączenia. Aplikacja przełącza do dodatkowej punktu końcowego, gdy symulacji awarii.

W części jednej serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Tworzenie konta magazynu
> * Pobierz przykład
> * Ustawianie parametrów połączenia
> * Uruchom aplikację konsolową

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/downloads/) z następującymi pakietami roboczymi:
  - **Tworzenie aplikacji na platformie Azure**

  ![Programowanie Azure (znajduje się w sieci Web i w chmurze)](media/storage-create-geo-redundant-storage/workloads.png)

* Pobierz i zainstaluj [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Konto magazynu zapewnia unikatową przestrzeń nazw do przechowywania i udostępniania obiektów danych usługi Azure storage.

Wykonaj następujące kroki, aby utworzyć konto magazynu geograficznie nadmiarowego dostęp do odczytu:

1. Wybierz **nowy** znaleziono przycisku w lewym górnym rogu portalu Azure.

2. Wybierz **magazynu** z **nowy** i wybrać opcję **konta magazynu — obiekt blob, plików, tabeli, kolejki** w obszarze **proponowanym**.
3. Wypełnij formularz konta magazynu, z następującymi informacjami, jak pokazano w poniższych obrazu i wybierz **Utwórz**:

   | Ustawienie       | Sugerowana wartość | Opis |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nazwa** | mojekontomagazynu | Unikatową wartość dla konta magazynu |
   | **Model wdrażania** | Resource Manager  | Menedżer zasobów zawiera najnowsze funkcje.|
   | **Typ konta** | Zastosowania ogólne | Aby uzyskać więcej informacji dotyczących typów kont, zobacz [typy kont magazynu](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Wydajność** | Standardowa (Standard) | Standard jest wystarczająca dla przykładowy scenariusz. |
   | **Replikacja**| Magazyn geograficznie nadmiarowy dostępny do odczytu (RA-GRS) | Jest to konieczne, na przykład działał. |
   |**Zapewnienia bezpiecznego transferu wymagane** | Disabled (Wyłączony)| Zapewnienia bezpiecznego transferu nie jest wymagane dla tego scenariusza. |
   |**Subskrypcja** | Subskrypcji |Aby uzyskać szczegółowe informacje o subskrypcjach, zobacz [Subskrypcje](https://account.windowsazure.com/Subscriptions). |
   |**Grupa zasobów** | myResourceGroup |Prawidłowe nazwy grup zasobów opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). |
   |**Lokalizacja** | Wschodnie stany USA | Wybierz lokalizację. |

![Tworzenie konta magazynu](media/storage-create-geo-redundant-storage/figure1.png)

## <a name="download-the-sample"></a>Pobierz przykład

[Pobierz przykładowy projekt](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip).

Wyodrębnij (Rozpakuj) storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip pliku.
Przykładowy projekt zawiera aplikacji konsoli.

## <a name="set-the-connection-string"></a>Ustawianie parametrów połączenia

W aplikacji należy wprowadzić parametry połączenia konta magazynu. Zaleca się do przechowywania tych parametrów połączenia w zmiennej środowiskowej na komputerze lokalnym działania aplikacji. Wykonaj jedną z przykłady poniżej, w zależności od systemu operacyjnego, można utworzyć zmiennej środowiskowej.

W portalu Azure przejdź do swojego konta magazynu. Wybierz **klucze dostępu** w obszarze **ustawienia** na koncie magazynu. Kopiuj **ciąg połączenia** z klucz podstawowy lub pomocniczy. Zastąp \<yourconnectionstring\> z połączeniem rzeczywiste oparte na ciągach, uruchamiając jeden z następujących poleceń w systemie operacyjnym. To polecenie zapisuje zmienną środowiskową na komputerze lokalnym. W systemie Windows, zmiennej środowiskowej nie jest dostępne do czasu ponownie załadować **wiersza polecenia** lub korzystasz z powłoki. Zastąp  **\<storageConnectionString\>**  w następującym przykładzie:

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

![plik konfiguracji aplikacji](media/storage-create-geo-redundant-storage/figure2.png)

## <a name="run-the-console-application"></a>Uruchom aplikację konsolową

W programie Visual Studio, naciśnij klawisz **F5** lub wybierz **Start** można rozpocząć debugowania aplikacji. Visual studio automatycznie przywracania brakujących pakietów NuGet Jeśli skonfigurowana, odwiedź stronę do [Instalowanie i ponownie zainstalować pakiety z Przywracanie pakietu](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) Aby dowiedzieć się więcej.

Uruchamia okno konsoli i rozpocznie się aplikacja uruchomiona. Aplikacja wysyła **HelloWorld.png** obrazu z rozwiązania do konta magazynu. Aplikacja sprawdza zapewnienie obraz został zreplikowany do dodatkowej RA-GRS punktu końcowego. Następnie rozpoczyna pobieranie obrazu do 999 razy. Odczyt każdy jest reprezentowany przez **P** lub **S**. Gdzie **P** reprezentuje podstawowy punkt końcowy i **S** reprezentuje punkt końcowy dodatkowej.

![Uruchomieniu aplikacji konsoli](media/storage-create-geo-redundant-storage/figure3.png)

W przykładowym kodzie `RunCircuitBreakerAsync` zadań w `Program.cs` plik jest używany do pobierania obrazu z konta magazynu przy użyciu [DownloadToFileAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet) metody. Przed pobierania [elementu OperationContext](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) jest zdefiniowany. Kontekst operacji definiuje procedury obsługi zdarzeń, wyzwalać podczas pobierania zostało ukończone pomyślnie, lub jeśli pobieranie kończy się niepowodzeniem i jest ponowną próbą.

### <a name="retry-event-handler"></a>Ponów próbę wykonania programu obsługi zdarzeń

`OperationContextRetrying` Program obsługi zdarzeń jest wywoływana podczas pobierania obrazu nie powiedzie się i jest ustawiona, aby ponowić próbę. Jeśli osiągnięciu maksymalnej liczby ponownych prób, które są zdefiniowane w aplikacji [LocationMode](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) żądania jest zmieniana na `SecondaryOnly`. Ustawienie to wymusza aplikacja próbuje pobrać obrazu z punktu końcowego dodatkowej. Ta konfiguracja ogranicza czas potrzebny na żądanie dotyczące obrazu jako podstawowy punkt końcowy nie podjęta przez nieograniczony czas.

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>Program obsługi zdarzeń żądanie zostało ukończone

`OperationContextRequestCompleted` Program obsługi zdarzeń jest wywoływana po pomyślnym zakończeniu operacji pobierania obrazu. Jeśli aplikacja używa dodatkowej punktu końcowego, aplikacja będzie później nadal używać tego punktu końcowego do 20 razy. Po 20 razy zestawów aplikacji [LocationMode](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) do `PrimaryThenSecondary` i ponawia podstawowy punkt końcowy. Jeśli żądanie zakończy się pomyślnie, aplikacja nadal odczytywać podstawowy punkt końcowy.

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times, 
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

## <a name="next-steps"></a>Kolejne kroki

W części jednej serii wiesz o wprowadzaniu aplikacji wysokiej dostępności z kontami magazynu RA-GRS, np.:

> [!div class="checklist"]
> * Tworzenie konta magazynu
> * Pobierz przykład
> * Ustawianie parametrów połączenia
> * Uruchom aplikację konsolową

Przejście do dwóch częścią serii, aby dowiedzieć się, jak symulację awarii i wymuszanie aplikacji umożliwiająca użycie dodatkowej RA-GRS punktu końcowego.

> [!div class="nextstepaction"]
> [Symulację awarii w powiązaniu z punktu końcowego podstawowego magazynu](storage-simulate-failure-ragrs-account-app.md)
