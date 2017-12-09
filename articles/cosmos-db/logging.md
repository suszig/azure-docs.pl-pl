---
title: "Azure DB rozwiązania Cosmos rejestrowania diagnostycznego | Dokumentacja firmy Microsoft"
description: "Użyj w tym samouczku ułatwią rozpoczęcie pracy z bazy danych Azure rozwiązania Cosmos rejestrowania."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: mimig
ms.openlocfilehash: 608222da9cbe2895914e14c9f76cc5629ef65684
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-cosmos-db-diagnostic-logging"></a>Azure DB rozwiązania Cosmos rejestrowania diagnostycznego

Po rozpoczęciu przy użyciu co najmniej jeden bazy danych Azure rozwiązania Cosmos baz danych można monitorować, kiedy baz danych są dostępne. Rejestrowanie w usłudze Azure DB rozwiązania Cosmos diagnostyczne umożliwia monitorowanie. Po włączeniu rejestrowania diagnostycznego, możesz wysłać dzienniki, aby [usługi Azure Storage](https://azure.microsoft.com/services/storage/), ich do strumienia [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), i/lub wyeksportować je do [analizy dzienników](https://azure.microsoft.com/services/log-analytics/), który jest częścią [ Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite).

![Rejestrowania diagnostycznego do magazynu, usługa Event Hubs lub Operations Management Suite za pomocą analizy dzienników](./media/logging/azure-cosmos-db-logging-overview.png)

Użyj tego samouczka, aby rozpocząć korzystanie z bazy danych rozwiązania Cosmos Azure rejestrowania za pomocą portalu Azure, interfejsu wiersza polecenia lub środowiska PowerShell.

## <a name="what-is-logged"></a>Co to jest rejestrowane?

* Wszystkie uwierzytelnione żądania usługi DocumentDB REST (SQL) interfejsu API są rejestrowane, w tym żądań zakończonych niepowodzeniem w wyniku uprawnienia dostępu, błędów systemu lub błędów w żądaniach. Obsługa bazy danych MongoDB, wykres i tabelę interfejsów API nie jest obecnie dostępna.
* Operacje w bazie danych, w tym operacji CRUD na wszystkie dokumenty, kontenery i baz danych.
* Operacje na klucze konta, które obejmują tworzenie, modyfikowanie lub usuwanie tych kluczy.
* Nieuwierzytelnione żądania, które powodują uzyskanie odpowiedzi 401. Na przykład żądania, które nie mają tokenu elementu nośnego, są nieprawidłowo sformułowane, wygasły lub mają nieprawidłowy token.

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka, musi mieć następujące zasoby:

* Istniejące bazy danych Azure rozwiązania Cosmos konta bazy danych i kontenera. Aby uzyskać instrukcje tworzenia tych zasobów, zobacz [Tworzenie konta bazy danych przy użyciu portalu Azure](create-documentdb-dotnet.md#create-a-database-account), [przykłady interfejsu wiersza polecenia](cli-samples.md), lub [przykłady środowiska PowerShell](powershell-samples.md).

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Włącz rejestrowanie w portalu Azure

1. W [portalu Azure](https://portal.azure.com), w Azure rozwiązania Cosmos DB konta, kliknij przycisk **dzienniki diagnostyczne** nawigacji po lewej stronie, a następnie kliknij polecenie **Włącz diagnostykę**.

    ![Włączanie rejestrowania diagnostyki dla bazy danych Azure rozwiązania Cosmos w portalu Azure](./media/logging/turn-on-portal-logging.png)

2. W **ustawień diagnostycznych** strony, wykonaj następujące czynności: 

    * **Nazwa**. Wprowadź nazwę dla dzienników do utworzenia.

    * **Archiwum na konto magazynu**. Aby użyć tej opcji, należy istniejące konto magazynu, aby nawiązać połączenie. Aby utworzyć nowe konto magazynu w portalu, zobacz [Utwórz konto magazynu](../storage/common/storage-create-storage-account.md) i postępuj zgodnie z instrukcjami, aby utworzyć Resource Manager konta ogólnego przeznaczenia. Następnie wróć do tej strony w portalu, aby wybrać konta magazynu. Może upłynąć kilka minut dla kont magazynu nowo utworzony pojawią się w menu rozwijanym.
    * **Strumień do Centrum zdarzeń**. Aby użyć tej opcji, należy istniejących Centrum zdarzeń przestrzeni nazw i zdarzenia koncentratora do nawiązania połączenia. Aby utworzyć przestrzeń nazw usługi Event Hubs, zobacz [tworzenie przestrzeni nazw usługi Event Hubs i Centrum zdarzeń za pomocą portalu Azure](../event-hubs/event-hubs-create.md). Następnie wróć do tej strony w portalu, aby wybrać nazwę przestrzeni nazw i zasad Centrum zdarzeń.
    * **Wyślij do analizy dzienników**.     Aby użyć tej opcji, użyj istniejący obszar roboczy lub utworzyć nowy obszar roboczy analizy dzienników, wykonując następujące kroki, aby [Utwórz nowy obszar roboczy](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) w portalu. Aby uzyskać więcej informacji o wyświetlaniu dzienników w analizy dzienników, zobacz [Wyświetl dzienniki w analizy dzienników](#view-in-loganalytics).
    * **Zaloguj się DataPlaneRequests**. Wybierz tę opcję, aby rejestrować diagnostyki dla konta usługi DocumentDB, wykres i tabelę interfejsu API. Jeśli archiwizacji do konta magazynu można wybrać okres przechowywania dzienników diagnostycznych. Dzienniki są autodeleted, po wygaśnięciu okresu przechowywania.
    * **Zaloguj się MongoRequests**. Wybierz tę opcję, aby dziennika diagnostyki dla kont API bazy danych MongoDB. Jeśli archiwizacji do konta magazynu można wybrać okres przechowywania dzienników diagnostycznych. Dzienniki są autodeleted, po wygaśnięciu okresu przechowywania.
    * **Metryka żądania**. Wybierz tę opcję, aby przechowywać pełne dane w [metryki Azure](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftdocumentdbdatabaseaccounts-cosmosdb). Jeśli archiwizacji do konta magazynu można wybrać okres przechowywania dzienników diagnostycznych. Dzienniki są autodeleted, po wygaśnięciu okresu przechowywania.

3. Kliknij pozycję **Zapisz**.

    Jeśli zostanie wyświetlony komunikat o błędzie informujący "nie można zaktualizować diagnostyki dla \<nazwa obszaru roboczego >. Subskrypcja \<identyfikator subskrypcji > nie jest zarejestrowany do użycia w elemencie microsoft.insights. " Postępuj zgodnie z [Rozwiązywanie problemów z diagnostyki Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) instrukcjami, aby zarejestrować konto, następnie ponów próbę wykonania tej procedury.

    Jeśli chcesz zmienić sposób dzienników diagnostycznych są zapisywane w dowolnym momencie w przyszłości, można powrócić do tej strony w w każdej chwili można zmodyfikować ustawienia dziennika diagnostycznego konta.

## <a name="turn-on-logging-using-cli"></a>Włącz rejestrowanie przy użyciu interfejsu wiersza polecenia

Aby włączyć metryki i rejestrowanie danych diagnostycznych przy użyciu wiersza polecenia platformy Azure, użyj następujących poleceń:

- Aby włączyć magazyn dzienników diagnostycznych na konto magazynu, należy użyć tego polecenia:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   `resourceId` Jest nazwa konta bazy danych Azure rozwiązania Cosmos. `storageId` Jest nazwą konta magazynu, do którego chcesz wysłać dzienniki.

- Do przesyłania strumieniowego dzienników diagnostycznych do Centrum zdarzeń, użyj tego polecenia:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   `resourceId` Jest nazwa konta bazy danych Azure rozwiązania Cosmos. `serviceBusRuleId` Jest ciągiem o następującym formacie:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Aby włączyć wysyłanie dzienników diagnostycznych do obszaru roboczego analizy dzienników, użyj tego polecenia:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Można połączyć tych parametrów, aby włączyć wiele opcji danych wyjściowych.

## <a name="turn-on-logging-using-powershell"></a>Włącz rejestrowanie przy użyciu programu PowerShell

Aby włączyć rejestrowanie za pomocą programu PowerShell, należy przy użyciu minimalnej wersji 1.0.1 programu Azure Powershell.

Aby zainstalować program Azure PowerShell i skojarzyć go z subskrypcją platformy Azure, zobacz [Sposób instalowania i konfigurowania programu Azure PowerShell](/powershell/azure/overview).

Jeśli jest już zainstalowany program Azure PowerShell, a nie wiadomo, wersji, z poziomu konsoli programu PowerShell, wpisz `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Nawiązywanie połączenia z subskrypcjami
Uruchom sesję programu PowerShell Azure i zaloguj się na konto platformy Azure przy użyciu następującego polecenia:  

```powershell
Login-AzureRmAccount
```

W podręcznym oknie przeglądarki wprowadź nazwę użytkownika i hasło dla konta platformy Azure. Program Azure PowerShell pobierze wszystkie subskrypcje, które są skojarzone z tym kontem, i domyślnie użyje pierwszej.

Jeśli masz wiele subskrypcji, określ konkretne konto, które zostało użyte do utworzenia usługi Azure Key Vault. Wpisz następujące polecenie, aby zobaczyć subskrypcje dla swojego konta:

```powershell
Get-AzureRmSubscription
```

Następnie aby określić subskrypcję skojarzoną z bazy danych Azure rozwiązania Cosmos konto logowania, wpisz:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Jeśli masz wiele subskrypcji skojarzonych z Twoim kontem jest ważne określić subskrypcję.
>
>

Aby uzyskać więcej informacji na temat konfigurowania programu Azure PowerShell, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

### <a id="storage"></a>Tworzenie nowego konta magazynu dla dzienników
Mimo że można użyć istniejącego konta magazynu dla dzienników, w tym samouczku utworzymy nowe konto magazynu do dzienników bazy danych rozwiązania Cosmos Azure w wersji dedykowanej. Dla wygody, możemy są przechowywane szczegóły konta magazynu do zmiennej o nazwie **sa**.

W celu ułatwienia zarządzania w tym samouczku używamy tej samej grupie zasobów, która zawiera naszej bazie danych bazy danych Azure rozwiązania Cosmos. Zastąp wartości ContosoResourceGroup, contosocosmosdblogs i "Północno-środkowe Stany" własne wartości zgodnie z wymaganiami:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Jeśli zdecydujesz się używać istniejącego konta magazynu, należy użyć tej samej subskrypcji jako subskrypcji bazy danych Azure rozwiązania Cosmos i musi używać modelu wdrażania usługi Resource Manager, a nie klasycznego modelu wdrożenia.
>
>

### <a id="identify"></a>Zidentyfikowanie konta bazy danych rozwiązania Cosmos Azure dla dzienników
Ustaw nazwę konta Azure DB rozwiązania Cosmos do zmiennej o nazwie **konta**, gdzie ResourceName jest nazwa konta bazy danych Azure rozwiązania Cosmos.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Włączanie rejestrowania
Aby włączyć rejestrowanie dla bazy danych rozwiązania Cosmos platformy Azure, użyj polecenia cmdlet Set-AzureRmDiagnosticSetting wraz ze zmiennymi dla nowego konta magazynu i konto bazy danych Azure rozwiązania Cosmos kategorii, dla której chcesz włączyć dzienniki. Uruchom następujące polecenie, ustawienie **-włączone** flaga **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

Dane wyjściowe polecenia powinny wyglądać w następujący sposób:

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

Potwierdza to, że włączono rejestrowanie dla bazy danych, informacje są zapisywane na koncie magazynu.

Opcjonalnie można także ustawić zasady przechowywania dla dzienników tak, aby starsze dzienniki były automatycznie usuwane. Na przykład ustaw zasady przechowywania, ustawiając wartość flagi **-RetentionEnable** na **$true** i wartość parametru **-RetentionInDays** na **90**, aby dzienniki starsze niż 90 dni były automatycznie usuwane.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Uzyskiwanie dostępu do dzienników
Azure DB rozwiązania Cosmos w dziennikach **DataPlaneRequests** kategorii są przechowywane w **insights dzienniki danych płaszczyzny — żądania** kontenera w podanym przez Ciebie koncie magazynu. 

Najpierw utwórz zmienną dla nazwy kontenera. Umożliwi to w dalszej części przewodnika.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Aby wyświetlić listę wszystkich obiektów blob w tym kontenerze, wpisz polecenie:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Dane wyjściowe będą wyglądać podobnie do poniższych:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

Jak widać w przedstawionych danych wyjściowych, obiekty BLOB zgodne z konwencją nazewnictwa:`resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Wartości daty i godziny używają czasu UTC.

Ponieważ to samo konto magazynu może służyć do zbierania dzienników dla wielu zasobów, identyfikator zasobu w pełni kwalifikowana nazwa obiektu blob jest bardzo przydatna na uzyskanie dostępu i pobranie tylko tych obiektów blob, które są potrzebne. Jednak zanim do tego przejdziemy, najpierw zostanie omówiony sposób pobierania wszystkich obiektów blob.

Najpierw utwórz folder w celu pobrania obiektów blob. Na przykład:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Następnie uzyskaj listę wszystkich obiektów blob:  

```powershell
$blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Prześlij tę listę potokiem do polecenia „Get-AzureStorageBlobContent”, aby pobrać obiekty blob do folderu docelowego:

```powershell
$blobs | Get-AzureStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Po uruchomieniu tego drugiego polecenia  **/**  ogranicznik w nazwach obiektów blob utworzy pełną strukturę folderów w folderze docelowym. Ta struktura folderu służy do pobierania i przechowywania obiektów blob jako plików.

Aby selektywnie pobierać obiekty blob, użyj symboli wieloznacznych. Na przykład:

* Jeśli masz wiele baz danych i chcesz pobrać dzienniki dla tylko jednej bazy danych, o nazwie CONTOSOCOSMOSDB3:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Jeśli masz wiele grup zasobów i chcesz pobrać dzienniki dla tylko jednej grupy zasobów, użyj parametru `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Jeśli chcesz pobrać wszystkie dzienniki dla miesiąca 2017 lipca, użyj `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Ponadto:

* Aby zbadać stan ustawień diagnostycznych dla zasobu bazy danych:`Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
* Aby wyłączyć rejestrowanie **DataPlaneRequests** kategorii dla zasobu konta bazy danych:`Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`


Obiekty BLOB, które są zwracane w każdym z tych kwerend są przechowywane jako tekst w formacie obiektu blob JSON, jak pokazano w poniższym kodzie. 

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

Aby dowiedzieć się więcej o danych w każdym obiekcie blob JSON, zobacz [interpretowanie dzienników bazy danych Azure rozwiązania Cosmos](#interpret).

## <a name="managing-your-logs"></a>Zarządzanie dzienników

Dzienniki są udostępniane na koncie dwóch godzin od momentu w którym wykonano operację bazy danych Azure rozwiązania Cosmos. To Ty zarządzasz dziennikami na swoim koncie magazynu:

* Użyj standardowych metod kontroli dostępu platformy Azure w celu zabezpieczenia dzienników, wprowadzając ograniczenia co do tego, kto może uzyskiwać do nich dostęp.
* Usuń dzienniki, których nie chcesz już przechowywać na koncie magazynu.
* Okres przechowywania danych płaszczyzny żądań zarchiwizowane na konto magazynu jest skonfigurowany w portalu po **DataPlaneRequests dziennika** jest zaznaczone. Aby zmienić to ustawienie, zobacz [Włącz rejestrowanie w portalu Azure](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-log-analytics"></a>Wyświetl dzienniki w analizy dzienników

W przypadku wybrania **wysyłać do analizy dzienników** opcji po włączeniu rejestrowania danych diagnostycznych z kolekcji jest przekazywane do analizy dzienników w ciągu dwóch godzin. Oznacza to, że jeśli analizy dzienników przyjrzeć się natychmiast po włączeniu rejestrowania, nie będą widzieć żadnych danych. Po prostu Zaczekaj dwie godziny i spróbuj ponownie. 

Przed wyświetleniem dzienników, należy sprawdzić i zobaczyć, jeśli obszaru roboczego analizy dzienników został uaktualniony do użycia nowego języka zapytań usługi Analiza dzienników. Aby to sprawdzić, należy otworzyć [portalu Azure](https://portal.azure.com), kliknij przycisk **analizy dzienników** po lewej stronie, następnie wybierz nazwę obszaru roboczego jak pokazano na poniższej ilustracji. **Obszarem roboczym pakietu OMS** zostanie wyświetlona strona, jak pokazano na poniższej ilustracji.

![Analiza dzienników w portalu Azure](./media/logging/azure-portal.png)

Jeśli zostanie wyświetlony następujący komunikat na **obszarem roboczym pakietu OMS** strony, obszar roboczy nie został uaktualniony do użycia w nowym języku. Aby uzyskać więcej informacji na temat uaktualniania do nowego języka zapytań, zobacz [uaktualnienia obszaru roboczego analizy dzienników Azure do nowego wyszukiwania dziennika](../log-analytics/log-analytics-log-search-upgrade.md). 

![Powiadomienie uaktualnienia log analytics](./media/logging/upgrade-notification.png)

Aby wyświetlić dane diagnostyczne analizy dzienników, otwórz stronę wyszukiwania dziennika z menu po lewej stronie lub z obszaru zarządzania strony, jak pokazano na poniższej ilustracji.

![Opcje wyszukiwania dziennika w portalu Azure](./media/logging/log-analytics-open-log-search.png)

Teraz, po włączeniu funkcji zbierania danych, uruchom w poniższym przykładzie wyszukiwania dziennika przy użyciu nowego języka zapytań na dziennikach 10 najnowszych `AzureDiagnostics | take 10`.

![Przykładowe potrwać 10 wyszukiwania dziennika](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Zapytania

Poniżej przedstawiono kilka dodatkowych zapytań można wprowadzić do **wyszukiwania dziennika** pole, aby ułatwić sobie monitorowanie kontenerów bazy danych Azure rozwiązania Cosmos. Te zapytania pracować z [nowy język](../log-analytics/log-analytics-log-search-upgrade.md). 

Aby uzyskać informacje o znaczeniu danych zwróconych przez każdego wyszukiwania dziennika, zobacz [interpretowanie dzienników bazy danych Azure rozwiązania Cosmos](#interpret).

* Wszystkie dzienniki diagnostyczne z bazy danych usługi Azure rozwiązania Cosmos w określonym przedziale czasu.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* Większość dziesięć ostatnio zarejestrowane zdarzenia.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* Wszystkie operacje pogrupowane według typu operacji.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Wszystkie operacje pogrupowane według zasobu.

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Wszystkie działania użytkownika, pogrupowane według zasobu. Należy pamiętać, że jest to dziennik aktywności nie dzienników diagnostycznych.

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Operacje trwać dłużej niż 3 milisekundy.

    ```
    AzureDiagnostics | where toint(duration_s) > 3000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Które agent jest uruchomiony operacje.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Jeśli wykonano długotrwałej operacji.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Aby uzyskać dodatkowe informacje na temat używania nowego języka wyszukiwania dziennika, zobacz [opis dziennika przeszukuje analizy dzienników](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Interpretowanie dzienników

Przechowywane w usłudze Azure Storage i analizy dzienników danych diagnostycznych Użyj bardzo podobne schematu. 

W poniższej tabeli opisano zawartości każdego wpisu dziennika.

| Usługa Azure Storage pola lub właściwości | Właściwości analizy dzienników | Opis |
| --- | --- | --- |
| time | TimeGenerated | Data i godzina (UTC), gdy podczas operacji. |
| resourceId | Zasób | Konto bazy danych Azure rozwiązania Cosmos, dla których dzienniki są włączone.|
| category | Kategoria | W przypadku dzienników bazy danych Azure rozwiązania Cosmos DataPlaneRequests jest dostępne tylko wartość. |
| operationName | OperationName | Nazwa operacji. Ta wartość może być dowolną z następujących operacji: tworzenia, aktualizacji, odczytu, ReadFeed, Usuń, Zastąp, Execute, SqlQuery, zapytania, JSQuery, Head, HeadFeed lub Upsert.   |
| properties | Nie dotyczy | Zawartość tego pola są opisane w kolejnych wierszach. |
| Identyfikator działania | activityId_g | Unikatowy identyfikator GUID dla zarejestrowanych operacji. |
| Agent użytkownika | userAgent_s | Ciąg określający agent użytkownika klienta wykonywania żądania. Format to {nazwa agenta użytkownika} / {wersja}.|
| Typ zasobu | ResourceType | Typ dostęp do zasobów. Ta wartość może być dowolny z następujących zasobów: bazy danych, kolekcji, dokument, załącznika, użytkownika, uprawnienia, procedura składowana, wyzwalacz, UserDefinedFunction lub oferty. |
| statusCode |statusCode_s | Stan odpowiedzi operacji. |
| requestResourceId | Identyfikator zasobu | Element resourceId odnoszące się do żądania, może wskazywać databaseRid, collectionRid lub documentRid w zależności od operacji wykonywane.|
| clientIpAddress | clientIpAddress_s | Adres IP klienta. |
| requestCharge | requestCharge_s | Liczba RUs używane w operacji |
| collectionRid | collectionId_s | Unikatowy identyfikator dla kolekcji.|
| Czas trwania | duration_s | Czas trwania operacji w taktach. |
| requestLength | requestLength_s | Długość żądania, w bajtach. |
| responseLength | responseLength_s | Długość odpowiedzi w bajtach.|
| resourceTokenUserRid | resourceTokenUserRid_s | Jest to pusty kiedy [tokenów zasobów](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) są używane do uwierzytelniania i wskazuje na identyfikator zasobu użytkownika. |

## <a name="next-steps"></a>Następne kroki

- Uzyskanie zrozumienia nie tylko sposób włączania rejestrowania, ale także kategorie metryki i dziennika, obsługiwane przez różnych Azure usługi odczytu zarówno [omówienie metryk w Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) i [Omówienie usługi Azure Dzienniki diagnostyczne](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artykułów.
- Przeczytaj następujące artykuły, aby dowiedzieć się więcej o usłudze event hubs:
   - [Co to jest usługa Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Rozpoczynanie pracy z usługą Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Odczyt [pobrać metryki i dzienników diagnostycznych z usługi Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
- Odczyt [opis dziennika przeszukuje analizy dzienników](../log-analytics/log-analytics-log-search-new.md)
