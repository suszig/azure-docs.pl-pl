<properties
    pageTitle="Tworzenie pierwszej fabryki danych (PowerShell) | Microsoft Azure"
    description="W tym samouczku przedstawiono tworzenie przykładowego potoku usługi Azure Data Factory przy użyciu programu Azure PowerShell."
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

# Tworzenie pierwszej fabryki danych Azure przy użyciu programu Azure PowerShell
> [AZURE.SELECTOR]
- [Omówienie samouczka](data-factory-build-your-first-pipeline.md)
- [Korzystanie z Edytora fabryki danych](data-factory-build-your-first-pipeline-using-editor.md)
- [Korzystanie z programu PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Korzystanie z programu Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Korzystanie z szablonu usługi Resource Manager](data-factory-build-your-first-pipeline-using-arm.md)
- [Korzystanie z interfejsu API REST](data-factory-build-your-first-pipeline-using-rest-api.md)


Ten artykuł zawiera instrukcje korzystania z programu Azure PowerShell w celu utworzenia pierwszej fabryki danych Azure. 


## Wymagania wstępne
Oprócz wymagań wstępnych wymienionych w artykule Omówienie samouczka wymagane jest:

- **Przeczytanie** artykułu [Omówienie samouczka](data-factory-build-your-first-pipeline.md) oraz wykonanie kroków wymagań wstępnych.
- Zainstalowanie programu **Azure PowerShell**. Postępuj zgodnie z instrukcjami w artykule [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell), aby zainstalować najnowszą wersję programu Azure PowerShell na komputerze.
- (opcjonalnie) Ten artykuł nie obejmuje wszystkich poleceń cmdlet dla usługi Fabryka danych. Pełna dokumentacja dotycząca poleceń cmdlet dla usługi Fabryka danych znajduje się w artykule [Data Factory Cmdlet Reference](https://msdn.microsoft.com/library/dn820234.aspx) (Dokumentacja dotycząca poleceń cmdlet dla usługi Fabryka danych). 

W przypadku korzystania z programu Azure PowerShell w **wersji starszej niż 1.0** trzeba będzie użyć poleceń cmdlet, które zostały opisane [tutaj](https://msdn.microsoft.com/library/azure/dn820234.aspx). Przed zastosowaniem poleceń cmdlet dla usługi Data Factory trzeba będzie również uruchomić następujące polecenia: 
 
1. Uruchom program Azure PowerShell i uruchom następujące polecenia. Nie zamykaj programu Azure PowerShell, zanim nie wykonasz wszystkich instrukcji z tego samouczka. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.
    1. Uruchom polecenie **Add-AzureAccount** i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal.
    2. Uruchom polecenie **Get-AzureSubscription**, aby wyświetlić wszystkie subskrypcje dla tego konta.
    3. Uruchom polecenie **Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext**, aby wybrać subskrypcję, której chcesz używać. Zastąp ciąg **NameOfAzureSubscription** nazwą subskrypcji platformy Azure.
4. Włącz tryb AzureResourceManager, gdyż polecenia cmdlet dla usługi Azure Data Factory są dostępne w tym trybie: **Switch-AzureMode AzureResourceManager**.


## Tworzenie fabryki danych

W tym kroku opisano użycie programu Azure PowerShell do utworzenia fabryki danych Azure o nazwie **FirstDataFactoryPSH**. Fabryka danych może obejmować jeden lub wiele potoków. Potok może obejmować jedno lub wiele działań. Na przykład działanie kopiowania w celu kopiowania danych ze źródła do docelowego magazynu danych oraz działanie programu Hive w usłudze HDInsight w celu uruchamiania skryptu programu Hive służącego do przekształcania danych wejściowych w dane wyjściowe produktu. Zacznijmy tworzenie fabryki danych w tym kroku. 

1. Uruchom program Azure PowerShell i uruchom następujące polecenie. Nie zamykaj programu Azure PowerShell, zanim nie wykonasz wszystkich instrukcji z tego samouczka. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.
    - Uruchom polecenie **Login-AzureRmAccount** i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal.  
    - Uruchom polecenie **Get-AzureRmSubscription**, aby wyświetlić wszystkie subskrypcje dla tego konta.
    - Uruchom polecenie **Select-AzureRmSubscription <Name of the subscription>**, aby wybrać subskrypcję, z którą chcesz pracować. Ta subskrypcja powinna być taka sama jak używana w witrynie Azure Portal.
3. Utwórz grupę zasobów Azure o nazwie **ADFTutorialResourceGroup** przez uruchomienie następującego polecenia.

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    W niektórych krokach w tym samouczku zakłada się, że używana jest grupa zasobów o nazwie ADFTutorialResourceGroup. Jeśli używasz innej grupy zasobów, podczas wykonywania instrukcji w tym samouczku trzeba będzie wstawić jej nazwę zamiast nazwy ADFTutorialResourceGroup.
4. Uruchom polecenie cmdlet **New-AzureRmDataFactory**, aby utworzyć fabrykę danych o nazwie **FirstDataFactoryPSH**.  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"


Pamiętaj o następujących kwestiach:
 
- Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli wystąpi błąd **Nazwa fabryki danych „FirstDataFactoryPSH” jest niedostępna**, zmień nazwę (np. TwojaNazwaFirstDataFactoryPSH). Użyj tej nazwy zamiast ADFTutorialFactoryPSH podczas wykonywania kroków w tym samouczku. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.
- Aby tworzyć wystąpienia usługi Fabryka danych, musisz być współautorem/administratorem subskrypcji Azure
- W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.
- Jeśli zostanie wyświetlony komunikat o błędzie: „**Subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw Microsoft.DataFactory**”, wykonaj jedną z następujących czynności i spróbuj opublikować ponownie: 

    - W programie Azure PowerShell uruchom następujące polecenie, aby zarejestrować dostawcę usługi Fabryka danych. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Można uruchomić następujące polecenie, aby potwierdzić, że dostawca usługi Data Factory jest zarejestrowany. 
    
            Get-AzureRmResourceProvider
    - Zaloguj się przy użyciu subskrypcji Azure do [portalu Azure](https://portal.azure.com) i przejdź do bloku Fabryka danych lub utwórz fabrykę danych w portalu Azure. To powoduje automatyczne zarejestrowanie dostawcy.

Przed utworzeniem potoku musisz utworzyć kilka jednostek usługi Fabryka danych. Najpierw utwórz połączone usługi, aby połączyć usługi magazynu danych/usługi obliczeniowe ze swoim magazynem danych, zdefiniuj wejściowe i wyjściowe zestawy danych do reprezentowania danych w połączonych magazynach danych, a następnie utwórz potok zawierający działanie, które korzysta z tych zestawów danych. 

## Tworzenie połączonych usług 
W tym kroku opisano połączenie konta usługi Azure Storage oraz klastra Azure HDInsight na żądanie z fabryką danych. Konto usługi Azure Storage będzie przechowywać dane wejściowe i wyjściowe dla potoku w tym przykładzie. Połączona usługa HDInsight służy do uruchamiania skryptu programu Hive określonego w działaniu potoku w tym przykładzie. Trzeba zidentyfikować usługi magazynu danych/usługi obliczeniowe, które są używane w tym scenariuszu oraz połączyć te usługi z fabryką danych przez utworzenie połączonych usług.

### Tworzenie połączonej usługi Azure Storage
W tym kroku opisano łączenie konta usługi Azure Storage z fabryką danych. Na potrzeby tego samouczka do przechowywania danych wejściowych/wyjściowych oraz pliku skryptu HQL używa się tego samego konta usługi Azure Storage.

1. W folderze C:\ADFGetStarted utwórz plik JSON o nazwie StorageLinkedService.json o następującej zawartości. Utwórz folder ADFGetStarted, jeśli jeszcze nie istnieje.

        {
            "name": "StorageLinkedService",
            "properties": {
                "type": "AzureStorage",
                "description": "",
                "typeProperties": {
                    "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        }

    Zastąp wartość **account name** nazwą konta usługi Azure Storage oraz wartość **account key** kluczem dostępu konta usługi Azure Storage. Informacje na temat pobierania klucza dostępu do magazynu znajdują się w artykule [Wyświetlanie, kopiowanie i ponowne generowanie kluczy dostępu do magazynu](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys).

2. W programie PowerShell Azure przejdź do folderu ADFGetStarted.
3. Możesz użyć polecenia cmdlet **New-AzureRmDataFactoryLinkedService**, aby utworzyć połączoną usługę. To polecenie cmdlet i inne polecenia cmdlet usługi Data Factory używane w tym samouczku wymagają przekazania wartości dla parametrów *ResourceGroupName* i *DataFactoryName*. Możesz też użyć polecenia **Get-AzureRmDataFactory**, aby pobrać obiekt **DataFactory** i przekazać obiekt bez wpisywania parametrów *ResourceGroupName* i *DataFactoryName* za każdym razem, gdy uruchamiasz polecenie cmdlet. Uruchom następujące polecenie, aby przypisać dane wyjściowe polecenia cmdlet **Get-AzureRmDataFactory** do zmiennej **$df**.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH

4. Teraz uruchom polecenie cmdlet **New-AzureRmDataFactoryLinkedService**, aby utworzyć połączoną usługę **StorageLinkedService**.

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    Jeśli polecenie cmdlet **Get-AzureRmDataFactory** nie zostało uruchomione i nie przypisano danych wyjściowych do zmiennej **$df**, trzeba określić wartości dla parametrów *ResourceGroupName* i *DataFactoryName* w następujący sposób.

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json

    Jeśli zamkniesz program Azure PowerShell w trakcie wykonywania samouczka, w celu dokończenia go trzeba będzie uruchomić polecenie cmdlet **Get-AzureRmDataFactory** po następnym uruchomieniu programu Azure PowerShell.

### Tworzenie połączonej usługi Azure HDInsight
W tym kroku przedstawiono łączenie klastra usługi HDInsight na żądanie z fabryką danych. Klaster usługi HDInsight jest automatycznie tworzony w czasie wykonywania oraz usuwany po zakończeniu przetwarzania i określonym czasie bezczynności. Możesz użyć własnego klastra usługi HDInsight zamiast klastra usługi HDInsight na żądanie. Szczegółowe informacje znajdują się w artykule [Compute Linked Services](data-factory-compute-linked-services.md) (Połączone usługi obliczeniowe).  

1. W folderze **C:\ADFGetStarted** utwórz plik JSON o nazwie **HDInsightOnDemandLinkedService**.json o następującej zawartości.

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "StorageLinkedService"
            }
          }
        }

    Poniższa tabela zawiera opis właściwości kodu JSON użytych w tym fragmencie kodu:

  	| Właściwość | Opis |
  	| :------- | :---------- |
  	| Wersja | Oznacza, że wersja utworzonej usługi HDInsight to 3.2. | 
  	| ClusterSize | Powoduje utworzenie klastra usługi HDInsight obejmującego jeden węzeł. | 
  	| TimeToLive | Określa czas bezczynności, po którym klaster usługi HDInsight zostanie usunięty. |
  	| linkedServiceName | Określa konto magazynu używane do przechowywania dzienników generowanych w usłudze HDInsight. |

    Pamiętaj o następujących kwestiach: 
    
    - Usługa Fabryka danych tworzy klaster usługi HDInsight **oparty na systemie Windows** za pomocą powyższego kodu JSON. Możliwe jest również utworzenie klastra usługi HDInsight **opartego na systemie Linux**. Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie). 
    - Możesz użyć **własnego klastra usługi HDInsight** zamiast klastra usługi HDInsight na żądanie. Szczegółowe informacje znajdują się w artykule [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (Połączona usługa HDInsight).
    - Klaster usługi HDInsight tworzy **kontener domyślny** w magazynie obiektów blob określonym w kodzie JSON (**linkedServiceName**). Usługa HDInsight nie powoduje usunięcia tego kontenera w przypadku usunięcia klastra. Jest to celowe. W przypadku połączonej usługi HDInsight na żądanie klaster usługi HDInsight jest tworzony za każdym razem, gdy trzeba przetworzyć wycinek — o ile w tym momencie nie istnieje aktywny klaster (**timeToLive**) — i zostaje usunięty po zakończeniu przetwarzania.
    
        Po przetworzeniu większej liczby wycinków w usłudze Azure Blob Storage będzie widocznych wiele kontenerów. Jeśli nie są potrzebne do rozwiązywania problemów z zadaniami, można je usunąć, aby zmniejszyć koszt przechowywania. Nazwy tych kontenerów są zgodne ze wzorcem: „adf**twojanazwafabrykidanych**-**nazwapołączonejusługi**-znacznikdatygodziny”. Aby usunąć kontenery z magazynu obiektów blob Azure, użyj takich narzędzi, jak [Microsoft Storage Explorer](http://storageexplorer.com/).

    Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie). 
2. Uruchom polecenie cmdlet **New-AzureRmDataFactoryLinkedService**, aby utworzyć połączoną usługę o nazwie HDInsightOnDemandLinkedService.

        New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json


## Tworzenie zestawów danych
W tym kroku opisano tworzenie zestawów danych do reprezentowania danych wejściowych i wyjściowych na potrzeby przetwarzania przy użyciu programu Hive. Te zestawy danych dotyczą elementu **StorageLinkedService** utworzonego wcześniej w ramach tego samouczka. Połączona usługa wskazuje konto usługi Azure Storage, a zestawy danych określają kontener, folder i nazwę pliku w magazynie, w którym przechowywane są dane wejściowe i wyjściowe.   

### Tworzenie wejściowego zestawu danych
1. W folderze **C:\ADFGetStarted**twórz plik JSON o nazwie **InputTable.json** o następującej zawartości:

        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "StorageLinkedService",
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

    Powyższy kod JSON definiuje zestaw danych o nazwie **AzureBlobInput**, który reprezentuje dane wejściowe dla działania w potoku. Ponadto określa, że dane wejściowe znajdują się w kontenerze obiektów blob o nazwie **adfgetstarted** oraz w folderze o nazwie **inputdata**.

    Poniższa tabela zawiera opis właściwości kodu JSON użytych w tym fragmencie kodu:

  	| Właściwość | Opis |
  	| :------- | :---------- |
  	| type | Właściwość type jest ustawiona na wartość AzureBlob, ponieważ dane znajdują się w magazynie obiektów blob Azure. |  
  	| linkedServiceName | Odnosi się do elementu StorageLinkedService utworzonego wcześniej. |
  	| fileName | Ta właściwość jest opcjonalna. Jeśli tę właściwość pominiesz, zostaną wybrane wszystkie pliki z folderu folderPath. W tym przypadku zostanie przetworzony tylko plik input.log. |
  	| type | Pliki dziennika są w formacie tekstowym, więc używana jest wartość TextFormat. | 
  	| columnDelimiter | Kolumny w plikach dziennika są rozdzielane przecinkami (,). |
  	| frequency/interval | Właściwość frequency (częstotliwość) jest ustawiona na wartość Month (Miesiąc), a wartość interwału wynosi 1, co oznacza, że wycinki wejściowe są dostępne co miesiąc. | 
  	| external | Ta właściwość ma wartość true (prawda), jeśli dane wejściowe nie są generowane przez usługę Fabryka danych. | 

2. Uruchom następujące polecenie w programie Azure PowerShell, aby utworzyć zestaw danych usługi Fabryka danych.

        New-AzureRmDataFactoryDataset $df -File .\InputTable.json

### Tworzenie wyjściowego zestawu danych
Teraz utworzysz wyjściowy zestaw danych do reprezentowania danych wyjściowych przechowywanych w usłudze Azure Blob Storage.

1. W folderze **C:\ADFGetStarted**twórz plik JSON o nazwie **OutputTable.json** o następującej zawartości:

        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
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

    Powyższy kod JSON definiuje zestaw danych o nazwie **AzureBlobOutput**, który reprezentuje dane wyjściowe dla działania w potoku. Ponadto określa, że wyniki są przechowywane w kontenerze obiektów blob o nazwie **adfgetstarted** oraz folderze o nazwie **partitioneddata**. W sekcji **availability** (dostępność) określono, że wyjściowy zestaw danych jest generowany co miesiąc.

2. Uruchom następujące polecenie w programie Azure PowerShell, aby utworzyć zestaw danych usługi Fabryka danych.

        New-AzureRmDataFactoryDataset $df -File .\OutputTable.json

## Tworzenie potoku
W tym kroku opisano tworzenie pierwszego potoku za pomocą działania **HDInsightHive**. Wycinek danych wejściowych jest dostępny co miesiąc (frequency: Month, interval: 1), wycinek danych wyjściowych jest generowany co miesiąc, a właściwość scheduler dla działania jest również ustawiona na wartość miesięczną (zobacz poniżej). Ustawienia dla wyjściowego zestawu danych i harmonogramu działania muszą być zgodne. W tym przypadku wyjściowy zestaw danych jest elementem wpływającym na ustawienia harmonogramu, więc musisz utworzyć wyjściowy zestaw danych nawet wtedy, gdy działanie nie generuje żadnych danych wyjściowych. Jeśli w działaniu nie są używane żadne dane wejściowe, możesz pominąć tworzenie zestawu danych wejściowych. Właściwości użyte w poniższym fragmencie kodu JSON zostały wyjaśnione na końcu tej sekcji. 


1. W folderze C:\ADFGetStarted utwórz plik JSON o nazwie MyFirstPipelinePSH.json o następującej zawartości:

    > [AZURE.IMPORTANT] Zastąp wartość **storageaccountname** w kodzie JSON nazwą swojego konta magazynu.
        
        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "StorageLinkedService",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
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
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    Ten fragment kodu JSON służy do utworzenia potoku obejmującego jedno działanie, które korzysta z programu Hive do przetwarzania danych w klastrze usługi HDInsight.
    
    Plik skryptu programu Hive **partitionweblogs.hql** jest przechowywany na koncie usługi Azure Storage (określonym za pomocą elementu scriptLinkedService o nazwie **StorageLinkedService**) oraz w folderze **script** w kontenerze **adfgetstarted**.

    Sekcja **defines** służy do określenia ustawień środowiska uruchomieniowego, które zostaną przekazane do skryptu programu Hive w formie wartości konfiguracyjnych programu Hive (np. ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).

    Właściwości **start** i **end** potoku określają aktywny okres potoku.

    W kodzie JSON dotyczącym działania określasz, że skrypt programu Hive jest uruchamiany w usłudze obliczeniowej określonej przez właściwość **linkedServiceName** — **HDInsightOnDemandLinkedService**.

    > [ACOM.NOTE] Artykuł [Anatomy of a Pipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline) (Anatomia potoku) zawiera szczegółowe informacje dotyczące właściwości kodu JSON użytych w przykładzie powyżej. 
2.  Upewnij się, że plik **input.log** jest wyświetlany w folderze **adfgetstarted/inputdata** w magazynie obiektów blob Azure, i uruchom następujące polecenie, aby wdrożyć potok. Ponieważ właściwości **start** i **end** są ustawione na wartość w przeszłości i właściwość **isPaused** została ustawiona na wartość „false”, potok (działanie w potoku) jest uruchamiany natychmiast po wdrożeniu. 

        New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
5. Gratulacje! Udało Ci się utworzyć pierwszy potok przy użyciu programu Azure PowerShell!

## Monitorowanie potoku
W tym kroku opisano użycie programu Azure PowerShell do monitorowania tego, co dzieje się w fabryce danych platformy Azure.

1. Uruchom polecenie **Get-AzureRmDataFactory** i przypisz dane wyjściowe do zmiennej **$df**.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH

2. Uruchom polecenie **Get-AzureRmDataFactorySlice**, aby uzyskać szczegółowe informacje na temat wszystkich wycinków elementu **EmpSQLTable**, który stanowi tabelę wyjściową potoku.  

        Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2014-02-01

    Zauważ, że określana tutaj właściwość StartDateTime oznacza taki sam czas rozpoczęcia jak określony w potoku JSON. Powinny pojawić się dane wyjściowe podobne do następujących:

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : FirstDataFactoryPSH
        DatasetName       : AzureBlobOutput
        Start             : 2/1/2014 12:00:00 AM
        End               : 3/1/2014 12:00:00 AM
        RetryCount        : 0
        State             : InProgress
        SubState          :
        LatencyStatus     :
        LongRetryCount    : 0


3. Uruchom polecenie **Get-AzureRmDataFactoryRun**, aby uzyskać szczegółowe informacje o uruchomieniach działania dla określonego wycinka.

        Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2014-02-01

    Powinny pojawić się dane wyjściowe podobne do następujących:
        
        Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : FirstDataFactoryPSH
        DatasetName         : AzureBlobOutput
        ProcessingStartTime : 12/18/2015 4:50:33 AM
        ProcessingEndTime   : 12/31/9999 11:59:59 PM
        PercentComplete     : 0
        DataSliceStart      : 2/1/2014 12:00:00 AM
        DataSliceEnd        : 3/1/2014 12:00:00 AM
        Status              : AllocatingResources
        Timestamp           : 12/18/2015 4:50:33 AM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : RunSampleHiveActivity
        PipelineName        : MyFirstPipeline
        Type                : Script

    Możesz kontynuować uruchamianie tego polecenia cmdlet do momentu, gdy wycinek będzie widoczny w stanie **Gotowe** lub **Niepowodzenie**. Gdy wycinek będzie w stanie Gotowe, sprawdź folder **partitioneddata** w kontenerze **adfgetstarted** w magazynie obiektów blob pod kątem danych wyjściowych.  Tworzenie klastra usługi HDInsight na żądanie zwykle zajmuje trochę czasu.

    ![Dane wyjściowe](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)


> [AZURE.IMPORTANT] Po pomyślnym przetworzeniu wycinka plik wejściowy zostanie usunięty. Tak więc, jeśli chcesz ponownie uruchomić wycinek lub ponownie wykonać instrukcje z tego samouczka, przekaż plik wejściowy (input.log) do folderu inputdata kontenera adfgetstarted.

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
| [Dokumentacja dotycząca poleceń cmdlet usługi Fabryka danych](https://msdn.microsoft.com/library/azure/dn820234.aspx) |  Zobacz pełną dokumentację dotyczącą poleceń cmdlet w usłudze Fabryka danych. |
| [Działania przekształcania danych](data-factory-data-transformation-activities.md) | Ten artykuł zawiera listę działań przekształcania danych (takich jak przekształcenie programu Hive w usłudze HDInsight używane w tym samouczku) obsługiwanych w usłudze Fabryka danych Azure. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono aspekty dotyczące planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) | Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory oraz ze sposobem konstruowania za ich pomocą przepływów pracy typu end-to-end opartych na danych na potrzeby scenariusza lub firmy. |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory.
| [Monitorowanie potoków i zarządzanie nimi przy użyciu bloków w witrynie Azure Portal](data-factory-monitor-manage-pipelines.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu bloków w witrynie Azure Portal. |
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. 




<!--HONumber=sep16_HO1-->


