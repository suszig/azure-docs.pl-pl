---
title: "Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu interfejsu API REST | Microsoft Docs"
description: "Ten samouczek zawiera instrukcje tworzenia potoku usługi Azure Data Factory za pomocą działania kopiowania przy użyciu interfejsu API REST."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/16/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: ebc5dbf790ca6012cfe9a7ea9ccee9fdacb46ffd
ms.openlocfilehash: ac1c60e04b42e2804ef17ba35368dd28c1d748a4


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-rest-api"></a>Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu interfejsu API REST
> [!div class="op_single_selector"]
> * [Przegląd i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kreator kopiowania](data-factory-copy-data-wizard-tutorial.md)
> * [Witryna Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Program Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [Program PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Szablon usługi Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [Interfejs API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [Interfejs API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Ten samouczek pokazuje, jak utworzyć i monitorować fabrykę danych Azure przy użyciu interfejsu API REST. Potok w fabryce danych używa działania kopiowania w celu kopiowania danych z usługi Azure Blob Storage do usługi Azure SQL Database.

> [!NOTE]
> Ten artykuł nie obejmuje całego interfejsu API REST usługi Data Factory. Pełna dokumentacja dotycząca poleceń cmdlet usługi Data Factory znajduje się w artykule [Data Factory Cmdlet Reference](https://msdn.microsoft.com/library/azure/dn906738.aspx) (Dokumentacja dotycząca interfejsu API REST usługi Data Factory).
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
* Zapoznaj się z artykułem [Omówienie samouczka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) i wykonaj kroki **wymagań wstępnych**.
* Zainstaluj na komputerze narzędzie [Curl](https://curl.haxx.se/dlwiz/). W połączeniu z poleceniami REST umożliwia ono utworzenie fabryki danych. 
* Postępuj zgodnie z instrukcjami zawartymi w [tym artykule](../azure-resource-manager/resource-group-create-service-principal-portal.md), aby wykonać następujące czynności: 
  1. Utworzenie aplikacji sieci Web o nazwie **ADFCopyTutorialApp** w usłudze Azure Active Directory.
  2. Pobranie **identyfikatora klienta** i **klucza tajnego**. 
  3. Uzyskanie **identyfikatora dzierżawy**. 
  4. Przypisanie aplikacji **ADFCopyTutorialApp** do roli **Współautor Data Factory**.  
* Zainstaluj program [Azure PowerShell](/powershell/azureps-cmdlets-docs).  
* Uruchom program **PowerShell** i uruchom następujące polecenie. Nie zamykaj programu Azure PowerShell, zanim nie wykonasz wszystkich instrukcji z tego samouczka. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.
  
  1. Uruchom poniższe polecenie i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal.
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. Uruchom poniższe polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta.

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. Uruchom poniższe polecenie, aby wybrać subskrypcję, z którą chcesz pracować. Zastąp ciąg **&lt;NameOfAzureSubscription**&gt; nazwą subskrypcji platformy Azure. 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. Utwórz grupę zasobów platformy Azure o nazwie **ADFTutorialResourceGroup** przez uruchomienie następującego polecenia w programie PowerShell.  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      Jeśli istnieje już grupa zasobów, można określić, czy należy ją zaktualizować (Y), czy zachować (N). 
     
      W niektórych krokach w tym samouczku zakłada się, że używana jest grupa zasobów o nazwie ADFTutorialResourceGroup. Jeśli używasz innej grupy zasobów, podczas wykonywania instrukcji w tym samouczku trzeba będzie wstawić jej nazwę zamiast nazwy ADFTutorialResourceGroup.

## <a name="create-json-definitions"></a>Tworzenie definicji JSON
W folderze, w którym znajduje się narzędzie curl.exe, utwórz następujące pliki w formacie JSON. 

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Nazwa musi być globalnie unikatowa — można o to zadbać, dodając do niej prefiks/sufiks ADFCopyTutorialDF. 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Zastąp wartości **accountname** i **accountkey** nazwą konta usługi Azure Storage oraz jego kluczem. Informacje na temat pobierania klucza dostępu do magazynu znajdują się w artykule [Wyświetlanie, kopiowanie i ponowne generowanie kluczy dostępu do magazynu](../storage/storage-create-storage-account.md#manage-your-storage-access-keys).
> 
> 

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="azuersqllinkedservicejson"></a>azuersqllinkedservice.json
> [!IMPORTANT]
> Zastąp parametry **servername**, **databasename**, **username** oraz **password** nazwą serwera SQL Azure, bazy danych, konta użytkownika i hasłem do konta.  
> 
> 

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "AzureStorageLinkedService",
    "typeProperties": {
      "folderPath": "adftutorial/",
      "fileName": "emp.txt",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ","
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

Powyższa definicja JSON określa zestaw danych o nazwie **AzureBlobInput**, który reprezentuje dane wejściowe dla działania w potoku. Ponadto określa, że dane wejściowe znajdują się w pliku **emp.txt**, który jest w kontenerze obiektów blob o nazwie **adftutorial**. 

 Pamiętaj o następujących kwestiach: 

* parametr **type** zestawu danych został ustawiony na **AzureBlob**.
* parametr **linkedServiceName** został ustawiony na **AzureStorageLinkedService**. 
* Parametr **folderPath** został ustawiony na kontener **adftutorial**, a parametr **fileName** ma wartość **emp.txt**.  
* Parametr **type** formatu został ustawiony na **TextFormat**
* W pliku tekstowym znajdują się dwa pola — **FirstName** i **LastName** — oddzielone przecinkiem (**columnDelimiter**)    
* Parametr **availability** został ustawiony na wartość **hourly** (parametr frequency ma wartość hour, a interval — 1). W związku z tym usługa Data Factory szuka danych wejściowych co godzinę w folderze głównym określonego kontenera obiektów blob (**adftutorial**). 

Jeśli nie określisz parametru **fileName** dla wejściowego zestawu danych, wszystkie pliki/obiekty blob z folderu danych wejściowych (**folderPath**) będą traktowane jako dane wejściowe. Jeśli określisz parametr fileName w kodzie JSON, tylko określony plik/obiekt blob będzie traktowany jako dane wejściowe.

Jeśli nie określisz parametru **fileName** dla **tabeli wyjściowej**, wygenerowane pliki w lokalizacji **folderPath** otrzymają nazwy w następującym formacie: Data.&lt;Guid&gt;.txt (np. Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).

Aby ustawić parametry **folderPath** i **fileName** dynamicznie w oparciu o czas **SliceStart**, użyj właściwości **partitionedBy**. W poniższym przykładzie parametr folderPath używa elementów Year, Month i Day z parametru SliceStart (czas rozpoczęcia przetwarzania wycinka), a parametr fileName używa elementu Hour z parametru SliceStart. Na przykład jeśli wycinek jest generowany dla czasu 2014-10-20T08:00:00, parametr folderName zostaje ustawiony na wikidatagateway/wikisampledataout/2014/10/20, a parametr fileName zostaje ustawiony na wartość 08.csv. 

```JSON
  "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy": 
[
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
],
```

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "emp"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

Powyższa definicja JSON określa zestaw danych o nazwie **AzureSqlOutput**, który reprezentuje dane wyjściowe dla działania w potoku. Ponadto określa, że wyniki są przechowywane w tabeli **emp** w bazie danych reprezentowanej przez usługę AzureSqlLinkedService. W sekcji **availability** określono, że wyjściowy zestaw danych jest generowany co godzinę (frequency: hour, interval: 1).

Pamiętaj o następujących kwestiach: 

* parametr **type** zestawu danych został ustawiony na wartość **AzureSQLTable**.
* Parametr **linkedServiceName** został ustawiony na wartość **AzureSqlLinkedService**.
* Parametr **tablename** został ustawiony na wartość **emp**.
* Tabela emp bazy danych zawiera trzy kolumny — **ID**, **FirstName** i **LastName**. ID to kolumna tożsamości, więc należy określić tylko wartości **FirstName** i **LastName**.
* Parametr **availability** (dostępność) został ustawiony na wartość **hourly** (co godzinę) (parametr **frequency** [częstotliwość] został ustawiony na **hour** [godzinę], a **interval** [interwał] został ustawiony na wartość **1**).  Usługa Data Factory co godzinę generuje wycinek danych wyjściowych w tabeli **emp** w bazie danych Azure SQL Database.

### <a name="pipelinejson"></a>pipeline.json

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-08-12T00:00:00Z",
    "end": "2016-08-13T00:00:00Z"
  }
}
```

Pamiętaj o następujących kwestiach:

* W sekcji działań jest tylko jedno działanie, którego parametr **type** (typ) został ustawiony na wartość **CopyActivity**.
* Dane wejściowe dla działania mają ustawienie **AzureBlobInput**, a dane wyjściowe — **AzureSqlOutput**.
* W sekcji **przekształcania** parametr **BlobSource** został określony jako typ źródłowy, a parametr **SqlSink** został określony jako typ ujścia.

Zastąp wartość właściwości **start** datą bieżącą, a wartość **end** datą jutrzejszą. Możesz określić tylko część daty i pominąć część godziny parametru data/godzina. Na przykład „2015-02-03” jest odpowiednikiem „2015-02-03T00:00:00Z”

Zarówno data/godzina rozpoczęcia, jak i data/godzina zakończenia muszą być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-10-14T16:32:41Z. Czas **end** jest opcjonalny, ale w tym samouczku zostanie użyty. 

Jeśli nie określisz wartości dla właściwości **end**, zostanie ona obliczona jako „**czas rozpoczęcia + 48 godzin**”. Aby uruchomić potok bezterminowo, określ **9999-09-09** jako wartość właściwości **end**.

W tym przykładzie występują 24 wycinki danych, ponieważ poszczególne wycinki są generowane co godzinę.

> [!NOTE]
> Artykuł [Anatomy of a Pipeline](data-factory-create-pipelines.md) (Anatomia potoku) zawiera szczegółowe informacje dotyczące właściwości kodu JSON użytych w przykładzie powyżej.
> 
> 

## <a name="set-global-variables"></a>Ustawianie zmiennych globalnych
Po zastąpieniu wartości własnymi wykonaj następujące polecenia w programie Azure PowerShell:

> [!IMPORTANT]
> Zobacz sekcję [Wymagania wstępne](#prerequisites), aby uzyskać instrukcje dotyczące pobierania identyfikatora klienta, klucza tajnego klienta, identyfikatora dzierżawy oraz identyfikatora subskrypcji.   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a>Uwierzytelnianie przy użyciu usługi AAD
Uruchom następujące polecenie w celu uwierzytelniania za pomocą usługi Azure Active Directory (AAD). 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a>Tworzenie fabryki danych
W tym kroku opisano tworzenie fabryki danych Azure o nazwie **ADFCopyTutorialDF**. Fabryka danych może obejmować jeden lub wiele potoków. Potok może obejmować jedno lub wiele działań. Na przykład działanie kopiowania w celu kopiowania danych ze źródła do docelowego magazynu danych. Albo działanie programu Hive w usłudze HDInsight w celu uruchamiania skryptu programu Hive służącego do przekształcania danych. Uruchom następujące polecenia, aby utworzyć fabrykę danych: 

1. Przypisz polecenie do zmiennej o nazwie **cmd**. 
   
    Upewnij się, że określona tutaj nazwa fabryki danych (ADFCopyTutorialDF) odpowiada nazwie podanej w pliku **datafactory.json**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF?api-version=2015-10-01};
    ```
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Przejrzyj wyniki. Jeśli fabryka danych została utworzona pomyślnie, w **wynikach** będzie widoczny kod JSON tej fabryki. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.  
   
    ```
    Write-Host $results
    ```

Pamiętaj o następujących kwestiach:

* Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli w wynikach jest wyświetlany błąd: **Nazwa fabryki danych „ADFCopyTutorialDF” jest niedostępna**, wykonaj następujące czynności:  
  
  1. Zmień nazwę fabryki (np. twojanazwaADFCopyTutorialDF) w pliku **datafactory.json**.
  2. W pierwszym poleceniu, w którym zmiennej **$cmd** jest przypisywana wartość, zastąp nazwę ADFCopyTutorialDF nową nazwą i uruchom to polecenie. 
  3. Uruchom kolejne dwa polecenia, aby wywołać interfejs API REST w celu utworzenia fabryki danych i wyświetlić wyniki tej operacji. 
     
     Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.
* Aby tworzyć wystąpienia usługi Fabryka danych, musisz być współautorem/administratorem subskrypcji Azure
* W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.
* Jeśli zostanie wyświetlony komunikat o błędzie: „**Subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw Microsoft.DataFactory**”, wykonaj jedną z następujących czynności i spróbuj opublikować ponownie: 
  
  * W programie Azure PowerShell uruchom następujące polecenie, aby zarejestrować dostawcę usługi Fabryka danych. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Można uruchomić następujące polecenie, aby potwierdzić, że dostawca usługi Data Factory jest zarejestrowany. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Zaloguj się przy użyciu subskrypcji Azure do [portalu Azure](https://portal.azure.com) i przejdź do bloku Fabryka danych lub utwórz fabrykę danych w portalu Azure. Ta akcja powoduje automatyczne zarejestrowanie dostawcy.

Przed utworzeniem potoku musisz utworzyć kilka jednostek usługi Fabryka danych. Najpierw utwórz połączone usługi, aby połączyć źródłowy i docelowy magazyn danych ze swoim magazynem danych. Następnie zdefiniuj wejściowy i wyjściowy zestaw danych w celu reprezentowania danych w połączonych magazynach danych. Na koniec utwórz potok z działaniem używającym tych zestawów danych.

## <a name="create-linked-services"></a>Tworzenie połączonych usług
Połączone usługi łączą magazyny danych lub usługi obliczeniowe z fabryką danych Azure. Magazynem danych może być usługa Azure Storage, usługa Azure SQL Database lub lokalna baza danych SQL Server, która zawiera dane wejściowe lub przechowuje dane wyjściowe dla potoku usługi Data Factory. Usługą obliczeniową jest usługa, która przetwarza dane wejściowe i generuje dane wyjściowe. 

W tym kroku opisano tworzenie dwóch połączonych usług: **AzureStorageLinkedService** i **AzureSqlLinkedService**. Połączona usługa AzureStorageLinkedService łączy z fabryką danych **ADFCopyTutorialIDF** konto usługi Azure Storage, a usługa AzureSqlLinkedService — bazę danych Azure SQL Database. W dalszej części tego samouczka opisano tworzenie potoku, który kopiuje dane z kontenera obiektów blob w usłudze AzureStorageLinkedService do tabeli SQL w usłudze AzureSqlLinkedService.

### <a name="create-azure-storage-linked-service"></a>Tworzenie połączonej usługi Azure Storage
W tym kroku opisano łączenie konta usługi Azure Storage z fabryką danych. W tym samouczku do przechowywania danych wejściowych używa się konta usługi Azure Storage. 

1. Przypisz polecenie do zmiennej o nazwie **cmd**. 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Przejrzyj wyniki. Jeśli połączona usługa została utworzona pomyślnie, w **wynikach** będzie widoczny kod JSON tej usługi. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a>Tworzenie połączonej usługi SQL Azure
W tym kroku opisano łączenie bazy danych Azure SQL Database z fabryką danych. W tym samouczku do przechowywania danych wyjściowych jest używana ta sama baza danych Azure SQL Database.

1. Przypisz polecenie do zmiennej o nazwie **cmd**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Przejrzyj wyniki. Jeśli połączona usługa została utworzona pomyślnie, w **wynikach** będzie widoczny kod JSON tej usługi. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Tworzenie zestawów danych
W poprzednim kroku utworzono połączone usługi **AzureStorageLinkedService** i **AzureSqlLinkedService** w celu powiązania konta usługi Azure Storage i bazy danych Azure SQL Database z fabryką danych **ADFCopyTutorialDF**. W tym kroku przedstawiono tworzenie zestawów danych reprezentujących dane wejściowe i wyjściowe dla działania kopiowania w potoku, który zostanie utworzony w następnym kroku. 

Wejściowy zestaw danych w tym samouczku odnosi się do kontenera obiektu blob w usłudze Azure Storage, na który wskazuje element AzureStorageLinkedService. Wyjściowy zestaw danych odwołuje się do tabeli SQL w bazie danych Azure SQL Database, na którą wskazuje element AzureSqlLinkedService.  

### <a name="prepare-azure-blob-storage-and-azure-sql-database-for-the-tutorial"></a>Przygotowanie magazynu obiektów blob Azure i bazy danych SQL Azure na potrzeby samouczka
Aby przygotować magazyn obiektów blob Azure i bazę danych Azure SQL Database w ramach tego samouczka, wykonaj następujące kroki. 

* Utwórz kontener obiektów blob o nazwie **adftutorial** w magazynie obiektów blob Azure, na który wskazuje usługa **AzureStorageLinkedService**. 
* Utwórz i przekaż plik tekstowy **emp.txt** jako obiekt blob do kontenera **adftutorial**. 
* Utwórz tabelę o nazwie **emp** w bazie danych SQL Azure, którą wskazuje usługa **AzureSqlLinkedService**.

1. Uruchom program Notatnik, wklej następujący tekst i zapisz go jako plik **emp.txt** w folderze **C:\ADFGetStartedPSH** na dysku twardym. 

    ```   
    John, Doe
    Jane, Doe
    ```
2. Użyj narzędzi takich jak [Eksplorator magazynu Azure](https://azurestorageexplorer.codeplex.com/) do utworzenia kontenera **adftutorial** i przekazania pliku **emp.txt** do kontenera.
   
    ![Eksplorator magazynu Azure](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Poniższy skrypt SQL umożliwia utworzenie tabeli **emp** w bazie danych SQL Azure.  

    ```SQL
    CREATE TABLE dbo.emp 
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 
    ```

    Jeśli masz zainstalowany na komputerze program SQL Server 2014, wykonaj instrukcje z sekcji [Step 2: Connect to SQL Database (Krok 2. Łączenie z bazą danych SQL) w artykule Managing Azure SQL Database using SQL Server Management Studio (Zarządzanie bazą danych Azure SQL Database przy użyciu programu SQL Server Management Studio)][sql-management-studio], aby połączyć się z serwerem SQL Azure i uruchomić skrypt SQL.

    Jeśli klient nie ma dostępu do serwera SQL Azure, musisz skonfigurować zaporę serwera SQL Azure tak, aby dostęp z Twojego komputera (adresu IP) był dozwolony. W [tym artykule](../sql-database/sql-database-configure-firewall-settings.md) opisano kroki konfigurowania zapory dla serwera SQL Azure.

### <a name="create-input-dataset"></a>Tworzenie wejściowego zestawu danych
W tym kroku opisano tworzenie zestawu danych o nazwie **AzureBlobInput** wskazującego na kontener obiektów blob w usłudze Azure Storage reprezentowany przez połączoną usługę **AzureStorageLinkedService**. Ten kontener obiektów blob (**adftutorial**) zawiera dane wejściowe w pliku **emp.txt**. 

1. Przypisz polecenie do zmiennej o nazwie **cmd**. 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Przejrzyj wyniki. Jeśli zestaw danych został utworzony pomyślnie, w **wynikach** będzie widoczny kod JSON tego zestawu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Tworzenie wyjściowego zestawu danych
W tym kroku tworzona jest tabela danych wyjściowych o nazwie **AzureSqlOutput**. Ten zestaw danych wskazuje tabelę SQL (**emp**) w bazie danych Azure SQL Database reprezentowanej przez usługę **AzureSqlLinkedService**. Potok kopiuje dane z wejściowego obiektu blob do tabeli **emp**. 

1. Przypisz polecenie do zmiennej o nazwie **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Przejrzyj wyniki. Jeśli zestaw danych został utworzony pomyślnie, w **wynikach** będzie widoczny kod JSON tego zestawu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a>Tworzenie potoku
W tym kroku opisano tworzenie potoku za pomocą **działania kopiowania**, w którym parametr **AzureBlobInput** jest używany jako dane wejściowe, a parametr **AzureSqlOutput** jako dane wyjściowe.

1. Przypisz polecenie do zmiennej o nazwie **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Przejrzyj wyniki. Jeśli zestaw danych został utworzony pomyślnie, w **wynikach** będzie widoczny kod JSON tego zestawu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.  

    ```PowerShell   
    Write-Host $results
    ```

**Gratulacje!** Fabryka danych Azure z potokiem kopiującym dane z usługi Azure Blob Storage do bazy danych Azure SQL Database została pomyślnie utworzona.

## <a name="monitor-pipeline"></a>Monitorowanie potoku
W tym kroku interfejs API REST usługi Data Factory służy do monitorowania wycinków generowanych przez potok.

```PowerShell
$ds ="AzureSqlOutput"
```

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

Uruchamiaj te polecenia cmdlet do momentu, gdy wycinek będzie widoczny w stanie **Gotowe** lub **Niepowodzenie**. Gdy wycinek jest w stanie Gotowe, sprawdź, czy w tabeli **emp** w bazie danych Azure SQL Database znajdują się dane wyjściowe. 

Dla każdego wycinka do tabeli emp w bazie danych Azure SQL Database zostają skopiowane dwa wiersze danych z pliku źródłowego. Po pomyślnym przetworzeniu wycinków (stan Gotowe) w tabeli emp będą widoczne 24 nowe rekordy. 

## <a name="summary"></a>Podsumowanie
W tym samouczku opisano tworzenie fabryki danych Azure za pomocą interfejsu API REST w celu kopiowania danych z obiektu blob Azure do bazy danych Azure SQL Database. Główne kroki opisane w tym samouczku:  

1. Tworzenie **fabryki danych** Azure.
2. Tworzenie **połączonych usług**:
   1. Połączona usługa Azure Storage, która ma nawiązać połączenie z kontem usługi Azure Storage, na którym przechowywane są dane wejściowe.     
   2. Połączona usługa SQL Azure, która ma nawiązać połączenie z bazą danych Azure SQL Database, w której przechowywane są dane wyjściowe. 
3. Tworzenie **zestawów danych** opisujących dane wejściowe i wyjściowe dla potoków.
4. Tworzenie **potoku** za pomocą działania kopiowania, w którym źródłem jest element BlobSource, a ujściem element SqlSink. 

## <a name="see-also"></a>Zobacz też
| Temat | Opis |
|:--- |:--- |
| [Działania przenoszenia danych](data-factory-data-movement-activities.md) |Ten artykuł zawiera szczegółowe informacje dotyczące działania kopiowania używanego w tym samouczku. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) |W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) |Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory oraz ze sposobem konstruowania za ich pomocą przepływów pracy typu end-to-end opartych na danych na potrzeby scenariusza lub firmy. |
| [Zestawy danych](data-factory-create-datasets.md) |Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory. |
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) |Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. |

[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: /powershell/azureps-cmdlets-docs
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md



<!--HONumber=Dec16_HO4-->


