<properties
    pageTitle="Tworzenie pierwszej fabryki danych (REST) | Microsoft Azure"
    description="W tym samouczku przedstawiono instrukcje tworzenia przykładowego potoku usługi Azure Data Factory przy użyciu interfejsu API REST usługi Data Factory."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# Tworzenie pierwszej fabryki danych Azure przy użyciu interfejsu API REST usługi Data Factory
> [AZURE.SELECTOR]
- [Omówienie samouczka](data-factory-build-your-first-pipeline.md)
- [Korzystanie z Edytora fabryki danych](data-factory-build-your-first-pipeline-using-editor.md)
- [Korzystanie z programu PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Korzystanie z programu Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Korzystanie z szablonu usługi Resource Manager](data-factory-build-your-first-pipeline-using-arm.md)
- [Korzystanie z interfejsu API REST](data-factory-build-your-first-pipeline-using-rest-api.md)

Ten artykuł zawiera instrukcje korzystania z interfejsu API REST usługi Data Factory w celu utworzenia pierwszej fabryki danych Azure.

## Wymagania wstępne

- Zapoznaj się z artykułem [Omówienie samouczka](data-factory-build-your-first-pipeline.md). Ten artykuł ułatwia zapoznanie się z podstawowymi informacjami dotyczącymi usługi Azure Data Factory. 
- Zainstaluj na komputerze narzędzie [Curl](https://curl.haxx.se/dlwiz/). W połączeniu z poleceniami REST umożliwia ono utworzenie fabryki danych. 
- Postępuj zgodnie z instrukcjami zawartymi w [tym artykule](../resource-group-create-service-principal-portal.md), aby wykonać następujące czynności: 
    1. Utworzenie aplikacji sieci Web o nazwie **ADFGetStartedApp** w usłudze Azure Active Directory.
    2. Pobranie **identyfikatora klienta** i **klucza tajnego**. 
    3. Uzyskanie **identyfikatora dzierżawy**. 
    4. Przypisanie aplikacji **ADFGetStartedApp** do roli **Współautor Data Factory**.  
- Zainstaluj program [Azure PowerShell](../powershell-install-configure.md).  
- Uruchom program **PowerShell** i uruchom następujące polecenie. Nie zamykaj programu Azure PowerShell, zanim nie wykonasz wszystkich instrukcji z tego samouczka. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.
    1. Uruchom polecenie **Login-AzureRmAccount** i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal.  
    2. Uruchom polecenie **Get-AzureRmSubscription**, aby wyświetlić wszystkie subskrypcje dla tego konta.
    3. Uruchom polecenie **Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext**, aby wybrać subskrypcję, której chcesz używać. Zastąp ciąg **NameOfAzureSubscription** nazwą subskrypcji platformy Azure. 
3. Utwórz grupę zasobów platformy Azure o nazwie **ADFTutorialResourceGroup** przez uruchomienie następującego polecenia w programie PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    W niektórych krokach w tym samouczku zakłada się, że używana jest grupa zasobów o nazwie ADFTutorialResourceGroup. Jeśli używasz innej grupy zasobów, podczas wykonywania instrukcji w tym samouczku trzeba będzie wstawić jej nazwę zamiast nazwy ADFTutorialResourceGroup.

## Tworzenie definicji JSON
W folderze, w którym znajduje się narzędzie curl.exe, utwórz następujące pliki w formacie JSON. 

### datafactory.json 
> [AZURE.IMPORTANT] Nazwa musi być globalnie unikatowa — można o to zadbać, dodając do niej prefiks/sufiks ADFCopyTutorialDF. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### azurestoragelinkedservice.json
> [AZURE.IMPORTANT] Zastąp wartości **accountname** i **accountkey** nazwą konta usługi Azure Storage oraz jego kluczem. Informacje na temat pobierania klucza dostępu do magazynu znajdują się w artykule [Wyświetlanie, kopiowanie i ponowne generowanie kluczy dostępu do magazynu](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### hdinsightondemandlinkedservice.json

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Poniższa tabela zawiera opis właściwości kodu JSON użytych w tym fragmencie kodu:

| Właściwość | Opis |
| :------- | :---------- |
| Wersja | Oznacza, że wersja utworzonej usługi HDInsight to 3.2. | 
| ClusterSize | Rozmiar klastra usługi HDInsight. | 
| TimeToLive | Określa czas bezczynności, po którym klaster usługi HDInsight zostanie usunięty. |
| linkedServiceName | Określa konto magazynu używane do przechowywania dzienników generowanych w usłudze HDInsight. |

Pamiętaj o następujących kwestiach: 

- Usługa Fabryka danych tworzy klaster usługi HDInsight **oparty na systemie Windows** za pomocą powyższego kodu JSON. Możliwe jest również utworzenie klastra usługi HDInsight **opartego na systemie Linux**. Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie). 
- Możesz użyć **własnego klastra usługi HDInsight** zamiast klastra usługi HDInsight na żądanie. Szczegółowe informacje znajdują się w artykule [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (Połączona usługa HDInsight).
- Klaster usługi HDInsight tworzy **kontener domyślny** w magazynie obiektów blob określonym w kodzie JSON (**linkedServiceName**). Usługa HDInsight nie powoduje usunięcia tego kontenera w przypadku usunięcia klastra. To zachowanie jest celowe. W przypadku połączonej usługi HDInsight na żądanie klaster usługi HDInsight jest tworzony przy każdym przetwarzaniu wycinka — o ile w tym momencie nie istnieje aktywny klaster (**timeToLive**) — i zostaje usunięty po zakończeniu przetwarzania.

    Po przetworzeniu większej liczby wycinków w usłudze Azure Blob Storage będzie widocznych wiele kontenerów. Jeśli nie są potrzebne do rozwiązywania problemów z zadaniami, można je usunąć, aby zmniejszyć koszt przechowywania. Nazwy tych kontenerów są zgodne ze wzorcem: „adf**twojanazwafabrykidanych**-**nazwapołączonejusługi**-znacznikdatygodziny”. Aby usunąć kontenery z magazynu obiektów blob Azure, użyj takich narzędzi, jak [Microsoft Storage Explorer](http://storageexplorer.com/).

Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie). 

### inputdataset.json

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


Ten kod JSON definiuje zestaw danych o nazwie **AzureBlobInput**, który reprezentuje dane wejściowe dla działania w potoku. Ponadto określa, że dane wejściowe znajdują się w kontenerze obiektów blob o nazwie **adfgetstarted** oraz w folderze o nazwie **inputdata**.

Poniższa tabela zawiera opis właściwości kodu JSON użytych w tym fragmencie kodu:

| Właściwość | Opis |
| :------- | :---------- |
| type | Właściwość type jest ustawiona na wartość AzureBlob, ponieważ dane znajdują się w magazynie obiektów blob Azure. |  
| linkedServiceName | Odnosi się do elementu StorageLinkedService utworzonego wcześniej. |
| fileName | Ta właściwość jest opcjonalna. Jeśli tę właściwość pominiesz, zostaną wybrane wszystkie pliki z folderu folderPath. W tym przypadku zostanie przetworzony tylko plik input.log. |
| type | Pliki dziennika są w formacie tekstowym, więc używana jest wartość TextFormat. | 
| columnDelimiter | Kolumny w plikach dziennika są rozdzielane przecinkami (,) |
| frequency/interval | Właściwość frequency (częstotliwość) jest ustawiona na wartość Month (Miesiąc), a wartość interwału wynosi 1, co oznacza, że wycinki wejściowe są dostępne co miesiąc. | 
| external | Ta właściwość ma wartość true (prawda), jeśli dane wejściowe nie są generowane przez usługę Fabryka danych. | 

### outputdataset.json

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

Ten kod JSON definiuje zestaw danych o nazwie **AzureBlobOutput**, który reprezentuje dane wyjściowe dla działania w potoku. Ponadto określa, że wyniki są przechowywane w kontenerze obiektów blob o nazwie **adfgetstarted** oraz folderze o nazwie **partitioneddata**. W sekcji **availability** (dostępność) określono, że wyjściowy zestaw danych jest generowany co miesiąc.

### pipeline.json
> [AZURE.IMPORTANT] Zastąp wartość **storageaccountname** nazwą konta usługi Azure Storage. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

Ten fragment kodu JSON służy do utworzenia potoku obejmującego jedno działanie, które korzysta z programu Hive do przetwarzania danych w klastrze usługi HDInsight.

Plik skryptu programu Hive **partitionweblogs.hql** jest przechowywany na koncie usługi Azure Storage (określonym za pomocą elementu scriptLinkedService o nazwie **StorageLinkedService**) oraz w folderze **script** w kontenerze **adfgetstarted**.

Sekcja **defines** określa ustawienia środowiska uruchomieniowego, które są przekazywane do skryptu programu Hive w formie wartości konfiguracyjnych programu Hive (np. ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).

Właściwości **start** i **end** potoku określają aktywny okres potoku.

W kodzie JSON dotyczącym działania określasz, że skrypt programu Hive jest uruchamiany w usłudze obliczeniowej określonej przez właściwość **linkedServiceName** — **HDInsightOnDemandLinkedService**.

> [AZURE.NOTE] Artykuł [Anatomy of a Pipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline) (Anatomia potoku) zawiera szczegółowe informacje dotyczące właściwości kodu JSON użytych w przykładzie powyżej. 

## Ustawianie zmiennych globalnych

Po zastąpieniu wartości własnymi wykonaj następujące polecenia w programie Azure PowerShell:

> [AZURE.IMPORTANT] Zobacz sekcję [Wymagania wstępne](#prerequisites), aby uzyskać instrukcje dotyczące pobierania identyfikatora klienta, klucza tajnego klienta, identyfikatora dzierżawy oraz identyfikatora subskrypcji.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## Uwierzytelnianie przy użyciu usługi AAD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## Tworzenie fabryki danych

W tym kroku opisano tworzenie fabryki danych Azure o nazwie **FirstDataFactoryREST**. Fabryka danych może obejmować jeden lub wiele potoków. Potok może obejmować jedno lub wiele działań. Na przykład działanie kopiowania w celu kopiowania danych ze źródła do docelowego magazynu danych oraz działanie programu Hive w usłudze HDInsight w celu uruchamiania skryptu programu Hive służącego do przekształcania danych. Uruchom następujące polecenia, aby utworzyć fabrykę danych: 

1. Przypisz polecenie do zmiennej o nazwie **cmd**. 

    Upewnij się, że określona tutaj nazwa fabryki danych (ADFCopyTutorialDF) odpowiada nazwie podanej w pliku **datafactory.json**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Przejrzyj wyniki. Jeśli fabryka danych została utworzona pomyślnie, w **wynikach** będzie widoczny kod JSON tej fabryki. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.  

        Write-Host $results

Pamiętaj o następujących kwestiach:
 
- Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli w wynikach jest wyświetlany błąd: **Nazwa fabryki danych „FirstDataFactoryREST” jest niedostępna**, wykonaj następujące czynności:  
    1. Zmień nazwę fabryki (np. twojanazwaFirstDataFactoryREST) w pliku **datafactory.json**. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.
    2. W pierwszym poleceniu, w którym zmiennej **$cmd** jest przypisywana wartość, zastąp nazwę FirstDataFactoryREST nową nazwą i uruchom to polecenie. 
    3. Uruchom kolejne dwa polecenia, aby wywołać interfejs API REST w celu utworzenia fabryki danych i wyświetlić wyniki tej operacji. 
- Aby tworzyć wystąpienia usługi Fabryka danych, musisz być współautorem/administratorem subskrypcji Azure
- W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.
- Jeśli zostanie wyświetlony komunikat o błędzie: „**Subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw Microsoft.DataFactory**”, wykonaj jedną z następujących czynności i spróbuj opublikować ponownie: 

    - W programie Azure PowerShell uruchom następujące polecenie, aby zarejestrować dostawcę usługi Fabryka danych. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Można uruchomić następujące polecenie, aby potwierdzić, że dostawca usługi Data Factory jest zarejestrowany. 
    
            Get-AzureRmResourceProvider
    - Zaloguj się przy użyciu subskrypcji Azure do [portalu Azure](https://portal.azure.com) i przejdź do bloku Fabryka danych lub utwórz fabrykę danych w portalu Azure. Ta akcja powoduje automatyczne zarejestrowanie dostawcy.

Przed utworzeniem potoku musisz utworzyć kilka jednostek usługi Fabryka danych. Najpierw utwórz połączone usługi, aby połączyć usługi magazynu danych/usługi obliczeniowe ze swoim magazynem danych, oraz zdefiniuj wejściowe i wyjściowe zestawy danych do reprezentowania danych w połączonych magazynach danych. 

## Tworzenie połączonych usług 
W tym kroku opisano połączenie konta usługi Azure Storage oraz klastra Azure HDInsight na żądanie z fabryką danych. Konto usługi Azure Storage będzie przechowywać dane wejściowe i wyjściowe dla potoku w tym przykładzie. Połączona usługa HDInsight służy do uruchamiania skryptu programu Hive określonego w działaniu potoku w tym przykładzie. 

### Tworzenie połączonej usługi Azure Storage
W tym kroku opisano łączenie konta usługi Azure Storage z fabryką danych. W tym samouczku do przechowywania danych wejściowych/wyjściowych oraz pliku skryptu HQL używa się tego samego konta usługi Azure Storage.

1. Przypisz polecenie do zmiennej o nazwie **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Przejrzyj wyniki. Jeśli połączona usługa została utworzona pomyślnie, w **wynikach** będzie widoczny kod JSON tej usługi. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.
  
        Write-Host $results

### Tworzenie połączonej usługi Azure HDInsight
W tym kroku przedstawiono łączenie klastra usługi HDInsight na żądanie z fabryką danych. Klaster usługi HDInsight jest automatycznie tworzony w czasie wykonywania oraz usuwany po zakończeniu przetwarzania i określonym czasie bezczynności. Możesz użyć własnego klastra usługi HDInsight zamiast klastra usługi HDInsight na żądanie. Szczegółowe informacje znajdują się w artykule [Compute Linked Services](data-factory-compute-linked-services.md) (Połączone usługi obliczeniowe).  

1. Przypisz polecenie do zmiennej o nazwie **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Przejrzyj wyniki. Jeśli połączona usługa została utworzona pomyślnie, w **wynikach** będzie widoczny kod JSON tej usługi. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.  

        Write-Host $results

## Tworzenie zestawów danych
W tym kroku opisano tworzenie zestawów danych do reprezentowania danych wejściowych i wyjściowych na potrzeby przetwarzania przy użyciu programu Hive. Te zestawy danych dotyczą elementu **StorageLinkedService** utworzonego wcześniej w ramach tego samouczka. Połączona usługa wskazuje konto usługi Azure Storage, a zestawy danych określają kontener, folder i nazwę pliku w magazynie, w którym przechowywane są dane wejściowe i wyjściowe.   

### Tworzenie wejściowego zestawu danych
W tym kroku opisano tworzenie wejściowego zestawu danych do reprezentowania danych wejściowych przechowywanych w usłudze Azure Blob Storage.

1. Przypisz polecenie do zmiennej o nazwie **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Przejrzyj wyniki. Jeśli zestaw danych został utworzony pomyślnie, w **wynikach** będzie widoczny kod JSON tego zestawu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.
  
        Write-Host $results
### Tworzenie wyjściowego zestawu danych
W tym kroku opisano tworzenie wyjściowego zestawu danych do reprezentowania danych wyjściowych przechowywanych w usłudze Azure Blob Storage.

1. Przypisz polecenie do zmiennej o nazwie **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Przejrzyj wyniki. Jeśli zestaw danych został utworzony pomyślnie, w **wynikach** będzie widoczny kod JSON tego zestawu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.
  
        Write-Host $results 

## Tworzenie potoku
W tym kroku opisano tworzenie pierwszego potoku za pomocą działania **HDInsightHive**. Wycinek danych wejściowych jest dostępny co miesiąc (frequency: Month, interval: 1), wycinek danych wyjściowych jest generowany co miesiąc, a właściwość scheduler dla działania jest również ustawiona na wartość miesięczną. Ustawienia dla wyjściowego zestawu danych i harmonogramu działania muszą być zgodne. W tym przypadku wyjściowy zestaw danych jest elementem wpływającym na ustawienia harmonogramu, więc musisz utworzyć wyjściowy zestaw danych nawet wtedy, gdy działanie nie generuje żadnych danych wyjściowych. Jeśli w działaniu nie są używane żadne dane wejściowe, możesz pominąć tworzenie zestawu danych wejściowych.  

Upewnij się, że plik **input.log** jest wyświetlany w folderze **adfgetstarted/inputdata** w magazynie obiektów blob Azure, i uruchom następujące polecenie, aby wdrożyć potok. Ponieważ właściwości **start** i **end** są ustawione na wartość w przeszłości i właściwość **isPaused** została ustawiona na wartość „false”, potok (działanie w potoku) jest uruchamiany natychmiast po wdrożeniu. 

1. Przypisz polecenie do zmiennej o nazwie **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Uruchom to polecenie przy użyciu polecenia **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Przejrzyj wyniki. Jeśli zestaw danych został utworzony pomyślnie, w **wynikach** będzie widoczny kod JSON tego zestawu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie.  

        Write-Host $results
5. Gratulacje! Udało Ci się utworzyć pierwszy potok przy użyciu programu Azure PowerShell!

## Monitorowanie potoku
W tym kroku interfejs API REST usługi Data Factory służy do monitorowania wycinków generowanych przez potok.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }

Uruchom polecenie Invoke-Command i kolejne polecenie, aż zobaczysz, że wycinek jest w stanie **Gotowe** lub **Niepowodzenie**. Gdy wycinek będzie w stanie Gotowe, sprawdź folder **partitioneddata** w kontenerze **adfgetstarted** w magazynie obiektów blob pod kątem danych wyjściowych.  Tworzenie klastra usługi HDInsight na żądanie zwykle zajmuje trochę czasu.

![Dane wyjściowe](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Po pomyślnym przetworzeniu wycinka plik wejściowy zostanie usunięty. Tak więc, jeśli chcesz ponownie uruchomić wycinek lub ponownie wykonać instrukcje z tego samouczka, przekaż plik wejściowy (input.log) do folderu inputdata kontenera adfgetstarted.

Monitorowanie wycinków i rozwiązywanie problemów jest również możliwe za pośrednictwem witryny Azure Portal. Aby poznać szczegóły, zapoznaj się z informacjami dotyczącymi [monitorowania potoków w witrynie Azure Portal](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline).  

## Podsumowanie 
W tym samouczku opisano tworzenie fabryki danych Azure do przetwarzania danych przez uruchomienie skryptu programu Hive w klastrze platformy Hadoop w usłudze HDInsight. Użyto Edytora fabryki danych w witrynie Azure Portal, aby:  

1.  Utworzyć **fabrykę danych** Azure.
2.  Utworzyć dwie **połączone usługi**:
    1.  Połączoną usługę **Azure Storage** w celu połączenia magazynu obiektów blob Azure, w którym przechowywane są pliki wejściowe/wyjściowe, z fabryką danych.
    2.  Połączoną usługę **Azure HDInsight** na żądanie w celu połączenia klastra platformy Hadoop w usłudze HDInsight na żądanie z fabryką danych. Usługa Fabryka danych Azure tworzy klaster just in time platformy Hadoop w usłudze HDInsight, aby przetwarzać dane wejściowe i generować dane wyjściowe. 
3.  Utworzyć dwa **zestawy danych** zawierające dane wejściowe i wyjściowe dla działania programu Hive w usłudze HDInsight w potoku. 
4.  Utworzyć **potok** za pomocą działania **programu Hive w usłudze HDInsight**. 

## Następne kroki
W tym artykule opisano tworzenie potoku za pomocą działania przekształcania (działanie usługi HDInsight), które uruchamia skrypt programu Hive w klastrze usługi HDInsight platformy Azure na żądanie. Instrukcje dotyczące korzystania z działania kopiowania w celu kopiowania danych z magazynu obiektów blob Azure do usług SQL Azure znajdują się w artykule [Tutorial: Copy data from an Azure Blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Samouczek: kopiowanie danych z magazynu obiektów blob Azure do usług SQL Azure).

## Zobacz też
| Temat | Opis |
| :---- | :---- |
| [Dokumentacja interfejsu API REST usługi Data Factory](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Zobacz pełną dokumentację dotyczącą poleceń cmdlet w usłudze Fabryka danych. |
| [Działania przekształcania danych](data-factory-data-transformation-activities.md) | Ten artykuł zawiera listę działań przekształcania danych (takich jak przekształcenie programu Hive w usłudze HDInsight używane w tym samouczku) obsługiwanych w usłudze Fabryka danych Azure. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono aspekty dotyczące planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) | Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory oraz ze sposobem konstruowania za ich pomocą przepływów pracy typu end-to-end opartych na danych na potrzeby scenariusza lub firmy. |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory.
| [Monitorowanie potoków i zarządzanie nimi przy użyciu bloków w witrynie Azure Portal](data-factory-monitor-manage-pipelines.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu bloków w witrynie Azure Portal. |
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. 




<!--HONumber=sep16_HO1-->


