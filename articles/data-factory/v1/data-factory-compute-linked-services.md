---
title: "Obliczeniowe środowiskach obsługiwanych przez usługi fabryka danych Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat środowiska obliczeniowego, w których można użyć w potokach fabryki danych Azure (np. Azure HDInsight) do przechowywania danych transformacji lub proces."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/01/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 7733ea111de896ab0f825c85b89be25ebafdbd85
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Obliczenia bazy danych środowiskach obsługiwanych przez usługi fabryka danych Azure
> [!NOTE]
> Ten artykuł dotyczy wersji 1 usługi Data Factory, która jest ogólnie dostępna (GA). Jeśli używasz wersji 2 usługi fabryka danych, która jest w wersji zapoznawczej, zobacz [obliczeniowe połączonych usług w wersji 2](../compute-linked-services.md).

W tym artykule opisano różne środowiska obliczeniowe, w których można użyć do procesu lub przekształcenia danych. Podano również szczegółowe informacje o różnych konfiguracjach (na żądanie i użycie własnego) obsługiwane przez fabrykę danych podczas konfigurowania usług połączonych łączenia tych obliczeniowe środowisk do fabryki danych Azure.

Poniższa tabela zawiera listę środowiska obliczeniowe obsługiwane przez fabryki danych i działań, które można uruchomić na nich. 

| Środowisko obliczeniowe                      | activities                               |
| ---------------------------------------- | ---------------------------------------- |
| [Klaster usługi HDInsight na żądanie](#azure-hdinsight-on-demand-linked-service) lub [klastrem usługi HDInsight](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [przesyłania strumieniowego usługi Hadoop](data-factory-hadoop-streaming-activity.md) |
| [Partia zadań Azure](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Działania usługi Machine Learning: wykonywanie wsadowe i aktualizacja zasobów](data-factory-azure-ml-batch-execution-activity.md) |
| [Usługi Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Język U-SQL usługi Data Lake Analytics](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [magazyn danych Azure SQL](#azure-sql-data-warehouse-linked-service), [programu SQL Server](#sql-server-linked-service) | [Procedura składowana](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Obsługiwane wersje usługi HDInsight w fabryce danych Azure
Usługa Azure HDInsight obsługuje wielu wersjach klastra Hadoop, które można wdrożyć w dowolnym momencie. Każdy wybór wersji tworzy określonej wersji programu dystrybucji Hortonworks Data Platform (HDP) i zestaw składników, które są zawarte w tej dystrybucji. Microsoft zachowuje aktualizowania listę obsługiwanych wersji usługi hdinsight w celu zapewnienia najnowsze składniki ekosystemu Hadoop i poprawki. Aby uzyskać szczegółowe informacje, zobacz [obsługiwane wersje HDInsight](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> HDInsight opartych na systemie Linux wersji 3.3 została wycofana w 31 lipca 2017 r. Klienci usługi połączonej usługi HDInsight na żądanie v1 fabryki danych otrzymały do 15 grudnia 2017 do testowania i uaktualnienia do nowszej wersji usługi hdinsight. Na 31 lipca 2018 zostaną wycofane HDInsight opartych na systemie Windows.
>
> 

**Co się stanie po dacie wycofania** 

Po 15 grudnia 2017 r.:

- Nie można utworzyć wersji 3.3 HDInsight opartych na systemie Linux (i starszych) klastrów za pomocą połączoną usługą usługi HDInsight na żądanie w wersji 1 usługi fabryka danych Azure. 
- Jeśli [osType i/lub właściwość Version](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) nie zostały jawnie określone w istniejących definicji JSON usługi połączonej usługi HDInsight na żądanie v1 fabryki danych Azure, wartością domyślną będzie można zmienić z **w wersji 3.1, osType = = Windows** do **wersji =[najnowszej wersji domyślnej HDI](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions), osType = Linux**.

Po 31 lipca 2018:

- Nie można utworzyć dowolnej wersji systemu Windows w usłudze hdinsight przy użyciu połączoną usługą usługi HDInsight na żądanie w fabryce danych Azure w wersji 1. 

 **Zalecane akcje** 

- Aktualizacja [osType i/lub właściwość Version](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) wpływ na definicji v1 fabryki danych Azure połączoną usługą usługi HDInsight na żądanie do nowszej HDInsight opartych na systemie Linux wersje (HDInsight 3,6), upewnij się, że można użyć najnowszej platformy Hadoop składniki ekosystemu i poprawki. 
- Aby przetestować 15 grudnia 2017 Hive V1 fabryki danych Azure, Pig, MapReduce i Hadoop przesyłania strumieniowego działań, które odwołują się ryzyko połączonej usługi, aby upewnić się, są one zgodne z nowego *osType* i/lub  *Wersja* wartość domyślna (wersja = 3,6, osType = Linux) lub jawne wersji usługi HDInsight i osType jest uaktualniany do. Aby dowiedzieć się więcej na temat zgodności, zapoznaj się z tematem [migracji z klastra usługi HDInsight opartej na systemie Windows do klastra z systemem Linux](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) i [jakie są składniki platformy Hadoop i wersje dostępne w usłudze HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions) Dokumentacja stron sieci Web. 
- Jawnie ustawione osType Windows przed 15 grudnia 2017 r, jeśli chcesz kontynuować tworzenie klastrów usługi HDInsight opartych na systemie Windows przy użyciu fabryki danych Azure żądanie v1On HDInsight połączonej usługi. Jednak nadal rekomendujemy migrację do klastrów usługi HDInsight opartych na systemie Linux przed 31 lipca 2018. 
- Aktualizacji definicji DotNet niestandardowe działania w formacie JSON, aby użyć Azure partii połączonej usługi, korzystając z połączoną usługą usługi HDInsight na żądanie można wykonać działania niestandardowe v1DotNet fabryki danych Azure. Dowiedz się więcej na [skorzystać z działań niestandardowych w potoku Azure DataFactory](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) strony sieci Web w dokumentacji. 

>[!Note]
>Klienci, którzy korzystają z istniejących Bring Your Own klastra (BYOC) połączoną usługą usługi HDInsight w wersji 1 usługi fabryka danych Azure lub tych, którzy korzystają z BYOC i na żądanie usługi HDInsightLinked w fabryce danych Azure w wersji 2 najnowsza wersja działu pomocy technicznej ofAzure zasad HDInsight klastry już jest wymuszana, w związku z tym jest wymagana żadna akcja. 
>
> 


## <a name="on-demand-compute-environment"></a>Środowiska obliczeniowe na żądanie
W tym typie konfiguracji środowiska komputerowego pełni zarządza usługi fabryka danych Azure. Została ona utworzona automatycznie przez usługi fabryka danych przed zadania jest przesłane do przetwarzania danych i usuwane po zakończeniu zadania. Można utworzyć połączonej usługi dla środowiska obliczeń na żądanie, jest skonfigurowana i sterowanie ustawieniami szczegółowego do wykonywania zadań zarządzania klastrem i uruchamianie akcji.

> [!NOTE]
> Konfiguracja na żądanie jest obecnie obsługiwane tylko w przypadku klastrów usługi HDInsight Azure.
>
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight na żądanie połączona usługa
Usługi fabryka danych Azure może automatycznie tworzyć opartych na systemie Windows/Linux klastra usługi HDInsight na żądanie do przetwarzania danych. Klaster jest tworzony w tym samym regionie co skojarzone z klastrem konta magazynu (właściwość linkedServiceName w formacie JSON).

Należy pamiętać, że **ważne** punktów o HDInsight na żądanie połączonej usługi:

* Nie ma klastra usługi HDInsight na żądanie utworzone w ramach subskrypcji platformy Azure. usługi fabryka danych Azure zarządza klastrem usługi HDInsight na żądanie w Twoim imieniu.
* Dzienniki dla zadań, które są uruchamiane w klastrze usługi HDInsight na żądanie są kopiowane do konta magazynu skojarzone z klastrem usługi HDInsight. Te dzienniki są dostępne w portalu Azure w **szczegóły uruchomienia działania** bloku. Zobacz [monitorować i zarządzać potoki](data-factory-monitor-manage-pipelines.md) artykułu, aby uzyskać szczegółowe informacje.
* Naliczane są opłaty tylko do czasu, gdy klaster usługi HDInsight jest uruchomiony i uruchomionych zadań.

> [!IMPORTANT]
> Zwykle trwa **20 minut** lub więcej, aby udostępnić klaster Azure HDInsight na żądanie.
>
> 

### <a name="example"></a>Przykład
Następujące JSON definiuje opartych na systemie Linux usługi HDInsight połączony na żądanie. Usługi fabryka danych automatycznie tworzy **opartych na systemie Linux** klastra usługi HDInsight podczas przetwarzania wycinka danych. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.6",
            "osType": "Linux",
            "clusterSize": 1,
            "timeToLive": "00:05:00",            
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

> [!IMPORTANT]
> Klaster usługi HDInsight tworzy **kontener domyślny** w magazynie obiektów blob określonym w kodzie JSON (**linkedServiceName**). Usługa HDInsight nie powoduje usunięcia tego kontenera w przypadku usunięcia klastra. To zachowanie jest celowe. W przypadku połączonej usługi HDInsight na żądanie klaster usługi HDInsight jest tworzony za każdym razem, gdy trzeba przetworzyć wycinek — o ile w tym momencie nie istnieje aktywny klaster (**timeToLive**) — i zostaje usunięty po zakończeniu przetwarzania. 
>
> Po przetworzeniu większej liczby wycinków w usłudze Azure Blob Storage będzie widocznych wiele kontenerów. Jeśli nie są potrzebne do rozwiązywania problemów z zadaniami, można je usunąć, aby zmniejszyć koszt przechowywania. Nazwy tych kontenerów są zgodne z następującym wzorcem: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Aby usunąć kontenery z usługi Azure Blob Storage, użyj takich narzędzi, jak [Microsoft Storage Explorer](http://storageexplorer.com/).
>
> 

### <a name="properties"></a>Właściwości
| Właściwość                     | Opis                              | Wymagane |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Powinien mieć ustawioną właściwość type **HDInsightOnDemand**. | Yes      |
| Wartość ClusterSize                  | Liczba węzłów procesu roboczego/danych w klastrze. Klaster usługi HDInsight jest tworzony z głównymi węzłami 2 wraz z liczbą węzłów procesu roboczego, które określisz dla tej właściwości. Węzły mają rozmiar Standard_D3, który ma 4 rdzenie, więc klastra z węzłem procesu roboczego 4 przyjmuje 24 rdzenie (4\*4 = 16 rdzenie dla węzłów procesu roboczego, a także 2\*rdzenie 4 = 8 dla węzłów głównych). Zobacz [utworzyć Linux opartych klastrów Hadoop w usłudze HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) szczegółowe informacje na temat warstwy Standard_D3. | Yes      |
| wartość TimeToLive                   | Limit czasu bezczynności klastra usługi HDInsight na żądanie. Określa, jak długo klastra usługi HDInsight na żądanie pozostaje aktywne po zakończeniu działania uruchamiania, jeśli w klastrze nie ma żadnych aktywnych działań.<br/><br/>Na przykład jeśli uruchomienia działania trwa 6 minut i timetolive jest ustawiony na 5 minut, klaster pozostanie aktywności 5 minut po uruchomieniu 6 minut przetwarzania działania. Jeśli inny uruchamiania działania jest wykonywane z okna 6 minut, jednak jest przetwarzany przez tego samego klastra.<br/><br/>Tworzenie klastra usługi HDInsight na żądanie jest kosztowna operacja (może to potrwać pewien czas), użyj tak, to ustawienie jako potrzebne do zwiększenia wydajności fabryki danych przez ponowne użycie klastra usługi HDInsight na żądanie.<br/><br/>Jeśli wartość timetolive jest ustawiona na 0, klastra jest usuwany natychmiast po zakończeniu wykonywania działania. Natomiast jeśli ustawisz wysokiej wartości, klaster może pozostać bezczynny, co niepotrzebnie wysokich kosztów. Dlatego jest ważne, aby ustawić odpowiednią wartość, na podstawie Twoich potrzeb.<br/><br/>Jeśli skonfigurowana wartość timetolive właściwości wielu potoki można udostępniać wystąpienia klastra usługi HDInsight na żądanie. | Yes      |
| wersja                      | Wersja klastra usługi HDInsight, zobacz [obsługiwane wersje HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions) dla dozwolone wersji usługi HDInsight. Jeśli nie zostanie określony, używa [najnowszej wersji domyślnej HDI](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions). | Nie       |
| linkedServiceName            | Azure połączonej usługi magazynu do użycia przez klaster na żądanie do przechowywania i przetwarzania danych. W tym samym regionie co konto usługi Azure Storage jest tworzenie klastra usługi HDInsight.<p>Obecnie nie można utworzyć klastra usługi HDInsight na żądanie, która używa usługi Azure Data Lake Store jako magazynu. Jeśli chcesz przechowywać dane wynikowe z HDInsight przetwarzania w usłudze Azure Data Lake Store, umożliwia działanie kopiowania skopiować dane z magazynu obiektów Blob Azure do usługi Azure Data Lake Store. </p> | Yes      |
| additionalLinkedServiceNames | Określa, że dodatkowe konta magazynu dla usługi HDInsight połączonej usługi, dzięki czemu usługi fabryka danych można zarejestrować je w Twoim imieniu. Te konta magazynu musi być w tym samym regionie co klaster usługi HDInsight, który jest tworzony w tym samym regionie co konto magazynu, określony przez linkedServiceName. | Nie       |
| osType                       | Typ systemu operacyjnego. Dozwolone wartości to: Linux i Windows. Jeśli nie zostanie określony, Linux jest używany domyślnie.  <br/>Zdecydowanie recommand się przy użyciu systemu Linux na podstawie klastrów usługi HDInsight jako dacie wycofania dla usługi HDInsight w systemie Windows jest 31 lipca 2018. | Nie       |
| hcatalogLinkedServiceName    | Nazwa programu Azure SQL połączonej usługi, które HCatalog bazy danych. Klaster usługi HDInsight na żądanie jest tworzona przy użyciu bazy danych Azure SQL jako potrzeby magazynu metadanych. | Nie       |

#### <a name="additionallinkedservicenames-json-example"></a>przykład JSON additionalLinkedServiceNames

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Zaawansowane właściwości
Można również określić następujące właściwości do szczegółowej konfiguracji klastra usługi HDInsight na żądanie.

| Właściwość               | Opis                              | Wymagane |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Określa parametry konfiguracji podstawowej (jak core-site.xml) do klastra usługi HDInsight, który ma zostać utworzony. | Nie       |
| hBaseConfiguration     | Określa parametry konfiguracji bazy danych HBase (hbase-site.xml) dla klastra usługi HDInsight. | Nie       |
| hdfsConfiguration      | Określa parametry konfiguracji systemu plików HDFS (systemu plików hdfs-site.xml) dla klastra usługi HDInsight. | Nie       |
| hiveConfiguration      | Określa parametry konfiguracji gałąź (gałąź site.xml) dla klastra usługi HDInsight. | Nie       |
| mapReduceConfiguration | Określa parametry konfiguracji MapReduce (mapred-site.xml) dla klastra usługi HDInsight. | Nie       |
| oozieConfiguration     | Określa parametry konfiguracji Oozie (oozie-site.xml) dla klastra usługi HDInsight. | Nie       |
| stormConfiguration     | Określa parametry konfiguracji Storm (storm-site.xml) dla klastra usługi HDInsight. | Nie       |
| yarnConfiguration      | Określa parametry konfiguracji Yarn (yarn-site.xml) dla klastra usługi HDInsight. | Nie       |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Przykład — konfiguracji klastra usługi HDInsight na żądanie z właściwości zaawansowane

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "version": "3.6",
      "osType": "Linux",
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Rozmiary węzła
Można określić head, danych i węzły dozorcy z następującymi właściwościami: 

| Właściwość          | Opis                              | Wymagane |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Określa rozmiar węzła głównego. Wartość domyślna to: Standard_D3. Zobacz **określania rozmiarów węzła** sekcji, aby uzyskać szczegółowe informacje. | Nie       |
| dataNodeSize      | Określa rozmiar węzeł danych. Wartość domyślna to: Standard_D3. | Nie       |
| zookeeperNodeSize | Określa rozmiar dozorca Zoo węzła. Wartość domyślna to: Standard_D3. | Nie       |

#### <a name="specifying-node-sizes"></a>Określanie rozmiary węzła
Zobacz [rozmiary maszyn wirtualnych](../../virtual-machines/linux/sizes.md) artykułu wartości ciągu, należy określić dla właściwości, o których wspomniano w poprzedniej sekcji. Wartości muszą być zgodna z **poleceń cmdlet i interfejsów API** przywoływany w artykule. Jak widać w artykule, węzeł danych o rozmiarze duży (ustawienie domyślne) zawiera 7 GB pamięci, która nie może być dobrym dla danego scenariusza. 

Jeśli chcesz utworzyć D4 o rozmiarze węzłów głównych i węzłów procesu roboczego, określ **Standard_D4** jako wartość właściwości headNodeSize i dataNodeSize. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Jeśli określono nieprawidłową wartość dla tych właściwości, może pojawić się następujące **błąd:** nie można utworzyć klastra. Wyjątek: nie można ukończyć operacji tworzenia klastra. Operacja zakończona niepowodzeniem z kodem „400”. Końcowy stan klastra: „Błąd”. Komunikat o błędzie: "PreClusterCreationValidationFailure". Gdy zostanie wyświetlony ten błąd, upewnij się, że używasz **polecenia CMDLET i interfejsów API** nazwa z tabeli w [rozmiary maszyn wirtualnych](../../virtual-machines/linux/sizes.md) artykułu.  

> [!NOTE]
> Fabryka danych Azure nie obsługuje obecnie klastrów usługi HDInsight przy użyciu usługi Azure Data Lake Store jako podstawowy magazynu. Użyj magazynu Azure jako podstawowy magazynu dla klastrów usługi HDInsight. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Przełącz środowiska obliczeniowe
W tym typie konfiguracji użytkownicy mogą zarejestrować już istniejącego środowiska komputerowego jako połączonej usługi z fabryki danych. Środowiska komputerowego jest zarządzana przez użytkownika i używa jej do wykonania działania usługi fabryka danych.

Ten typ konfiguracji jest obsługiwane w następujących środowiskach obliczeniowych:

* Usługa Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Bazy danych Azure SQL, magazynu danych Azure SQL, programu SQL Server

## <a name="azure-hdinsight-linked-service"></a>Usługa Azure HDInsight połączona usługa
Można utworzyć usługi Azure HDInsight połączony do zarejestrowania klastrem usługi HDInsight przy użyciu fabryki danych.

### <a name="example"></a>Przykład

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Właściwości
| Właściwość          | Opis                              | Wymagane |
| ----------------- | ---------------------------------------- | -------- |
| type              | Powinien mieć ustawioną właściwość type **HDInsight**. | Yes      |
| clusterUri        | Identyfikator URI klastra usługi HDInsight.        | Yes      |
| nazwa użytkownika          | Określ nazwę użytkownika, który ma być używany do nawiązania połączenia z istniejącym klastrze usługi HDInsight. | Yes      |
| hasło          | Określ hasło dla konta użytkownika.   | Yes      |
| linkedServiceName | Nazwa usługi Azure Storage połączone usługi, która odwołuje się do magazynu obiektów blob platformy Azure, używane przez klaster usługi HDInsight. <p>Obecnie nie można określić, czy usługa Azure Data Lake Store połączonej usługi dla tej właściwości. Jeśli klaster usługi HDInsight ma dostęp do usługi Data Lake Store, może uzyskać dostępu do danych w usłudze Azure Data Lake Store, ze skryptów usługi Hive/Pig. </p> | Yes      |

## <a name="azure-batch-linked-service"></a>Partia zadań Azure połączona usługa
Można utworzyć usługi partia zadań Azure połączony do zarejestrowania puli partii maszynach wirtualnych (VM) do fabryki danych. Możesz uruchomić niestandardowych działań platformy .NET przy użyciu partii zadań Azure lub usługi Azure HDInsight.

Zobacz następujące tematy, jeśli jesteś nowym użytkownikiem usługi partia zadań Azure:

* [Podstawy usługi partia zadań Azure](../../batch/batch-technical-overview.md) Omówienie usługi partia zadań Azure.
* [Nowy AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) polecenia cmdlet, aby utworzyć konto partii zadań Azure (lub) [portalu Azure](../../batch/batch-account-create-portal.md) Aby utworzyć konto partii zadań Azure za pomocą portalu Azure. Zobacz [przy użyciu programu PowerShell do zarządzania konta usługi partia zadań Azure](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) tematu, aby uzyskać szczegółowe instrukcje na temat używania polecenia cmdlet.
* [Nowy AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) polecenia cmdlet, aby utworzyć pulę partii zadań Azure.

### <a name="example"></a>Przykład

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

Dołącz "**.\< Nazwa regionu\>**"do nazwy konta usługi partia zadań dla **accountName** właściwości. Przykład:

```json
"accountName": "mybatchaccount.eastus"
```

Inną opcją jest zapewnienie batchUri punktu końcowego, jak pokazano w poniższym przykładzie:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Właściwości
| Właściwość          | Opis                              | Wymagane |
| ----------------- | ---------------------------------------- | -------- |
| type              | Powinien mieć ustawioną właściwość type **AzureBatch**. | Yes      |
| Nazwa konta       | Nazwa konta partii zadań Azure.         | Yes      |
| accessKey         | Klucz dostępu dla konta usługi partia zadań Azure.  | Yes      |
| poolName          | Nazwa puli maszyn wirtualnych.    | Yes      |
| linkedServiceName | Nazwa usługi Azure Storage połączonej usługi skojarzone z tą usługą partii zadań Azure połączone. Tej połączonej usługi jest używany dla tymczasowych plików wymaganych do uruchomienia działania i przechowywanie dzienniki wykonywania działania. | Yes      |

## <a name="azure-machine-learning-linked-service"></a>Uczenie maszynowe Azure połączona usługa
Można utworzyć usługi Azure Machine Learning połączony do zarejestrowania partii uczenia maszynowego punktu końcowego oceniania z fabryką danych.

### <a name="example"></a>Przykład

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Właściwości
| Właściwość   | Opis                              | Wymagane |
| ---------- | ---------------------------------------- | -------- |
| Typ       | Powinien mieć ustawioną właściwość type: **uczenie maszynowe Azure**. | Yes      |
| mlEndpoint | Adres URL wsadowego oceniania.                   | Yes      |
| apiKey     | Interfejs API modelu opublikowanych obszaru roboczego.     | Yes      |

## <a name="azure-data-lake-analytics-linked-service"></a>Usługi Azure Data Lake Analytics połączona usługa
Możesz utworzyć **Azure Data Lake Analytics** połączonej usługi, aby połączyć z usługą Azure Data Lake Analytics obliczeniowe usługi fabryka danych Azure. Data Lake Analytics U-SQL działania w potoku odwołuje się do tej połączonej usługi. 

Poniższa tabela zawiera opisy ogólne właściwości używane w definicji JSON. Dodatkowo można wybrać nazwy głównej usługi i uwierzytelnianie poświadczeń użytkownika.

| Właściwość                 | Opis                              | Wymagane                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **Typ**                 | Powinien mieć ustawioną właściwość type: **AzureDataLakeAnalytics**. | Yes                                      |
| **Nazwa konta**          | Nazwa konta usługi Azure Data Lake Analytics.  | Yes                                      |
| **Element dataLakeAnalyticsUri** | Identyfikator URI, usługi Azure Data Lake Analytics.           | Nie                                       |
| **Identyfikator subskrypcji**       | Identyfikator subskrypcji platformy Azure                    | Nie (Jeśli nie zostanie określony, używany subskrypcji fabryki danych). |
| **grupy zasobów o nazwie**    | Nazwa grupy zasobów platformy Azure                | Nie (Jeśli nie zostanie określony, używana grupa zasobów z fabryką danych). |

### <a name="service-principal-authentication-recommended"></a>Uwierzytelnianie główna usługi (zalecane)
Aby używać uwierzytelniania głównej usługi, Zarejestruj podmiot aplikacji w usłudze Azure Active Directory (Azure AD) i przyznać jej dostęp do usługi Data Lake Store. Aby uzyskać szczegółowe instrukcje, zobacz [do usługi uwierzytelniania](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Zwróć uwagę na następujące wartości, które służą do definiowania połączonej usługi:
* Identyfikator aplikacji
* Klucz aplikacji 
* Identyfikator dzierżawy

Uwierzytelnianie usługi głównej przez określenie następujących właściwości:

| Właściwość                | Opis                              | Wymagane |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Określ identyfikator aplikacji klienta.     | Yes      |
| **servicePrincipalKey** | Określ klucz aplikacji.           | Yes      |
| **dzierżawy**              | Określ informacje dzierżawy (identyfikator nazwy lub dzierżawy domeny), w którym znajduje się aplikacja. Można go pobrać, ustawiając kursor myszy w prawym górnym rogu portalu Azure. | Yes      |

**Przykład: Usługa podmiotu zabezpieczeń uwierzytelniania**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Uwierzytelnianie poświadczeń użytkownika
Alternatywnie można uwierzytelnienia poświadczeń użytkownika dla usługi Data Lake Analytics przez określenie następujących właściwości:

| Właściwość          | Opis                              | Wymagane |
| :---------------- | :--------------------------------------- | :------- |
| **autoryzacji** | Kliknij przycisk **autoryzacji** przycisk Edytor fabryki danych i wprowadź Twoje poświadczenia, który przypisuje do tej właściwości adresu URL autoryzacji wygenerowana automatycznie. | Yes      |
| **Identyfikator sesji**     | Identyfikator sesji OAuth z sesji autoryzacji OAuth. Każdy identyfikator sesji jest unikatowy i mogą być użyte tylko raz. To ustawienie jest generowane automatycznie, gdy używasz Edytor fabryki danych. | Yes      |

**Przykład: Użytkownik poświadczeń uwierzytelniania**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Wygaśnięcia tokenu
Kod autoryzacji wygenerowanych przy użyciu **autoryzacji** przycisk wygaśnie po upływie pewnego czasu. Czas wygaśnięcia dla różnych typów kont użytkowników znajduje się w tabeli poniżej. Może zostać wyświetlony następujący błąd komunikatu podczas uwierzytelniania **wygaśnięcia tokenu**: poświadczeń błąd operacji: invalid_grant - AADSTS70002: błąd podczas sprawdzania poprawności poświadczeń. AADSTS70008: Udzielone prawa dostępu jest wygasnąć lub zostać odwołane. Identyfikator śledzenia: Identyfikator korelacji d18629e8-af88-43c5-88e3-d8419eb1fca1: sygnatura czasowa fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z

| Typ użytkownika                                | Wygasa po                            |
| :--------------------------------------- | :--------------------------------------- |
| Konta użytkowników, które nie są zarządzane przez usługę Azure Active Directory (@hotmail.com, @live.comitp.) | 12 godzin                                 |
| Konta użytkowników zarządzanych przez usługi Azure Active Directory (AAD) | Uruchom 14 dni od ostatniego wycinka. <br/><br/>90 dni, jeśli wycinek oparte na podstawie OAuth połączonej usługi jest uruchamiana co najmniej raz na 14 dni. |

Aby uniknąć/Rozwiąż ten błąd, ponownie autoryzować przy użyciu **autoryzacji** przycisku, gdy **wygaśnięcia tokenu** i wdrożenie połączonej usługi. Można również tworzyć wartości **sessionId** i **autoryzacji** właściwości programowo przy użyciu kodu w następujący sposób:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Zobacz [klasy AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [klasy AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), i [klasy AuthorizationSessionGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) tematy, aby uzyskać więcej informacji o klasach fabryki danych używana w kodzie. Dodaj odwołanie do: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll dla klasy WindowsFormsWebAuthenticationDialog. 

## <a name="azure-sql-linked-service"></a>Azure SQL połączona usługa
Tworzenie Azure połączoną usługą SQL i użyj go przy użyciu [działania dotyczącego procedury składowanej](data-factory-stored-proc-activity.md) aby wywołać procedurę składowaną z potoku fabryki danych. Zobacz [Łącznik usług SQL Azure](data-factory-azure-sql-connector.md#linked-service-properties) artykułu, aby uzyskać szczegółowe informacje o tej połączonej usługi.

## <a name="azure-sql-data-warehouse-linked-service"></a>Połączona usługa Magazyn danych Azure SQL
Tworzenie usługi Azure SQL Data Warehouse połączone i używać go z [działania dotyczącego procedury składowanej](data-factory-stored-proc-activity.md) aby wywołać procedurę składowaną z potoku fabryki danych. Zobacz [łącznika magazynu danych SQL Azure](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artykułu, aby uzyskać szczegółowe informacje o tej połączonej usługi.

## <a name="sql-server-linked-service"></a>Program SQL Server połączona usługa
Tworzenie usługi SQL Server połączone i używać go z [działania dotyczącego procedury składowanej](data-factory-stored-proc-activity.md) aby wywołać procedurę składowaną z potoku fabryki danych. Zobacz [łącznika programu SQL Server](data-factory-sqlserver-connector.md#linked-service-properties) artykułu, aby uzyskać szczegółowe informacje o tej połączonej usługi.

