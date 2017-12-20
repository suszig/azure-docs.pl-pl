---
title: Tworzenie pierwszej fabryki danych (Azure Portal) | Microsoft Docs
description: "W tym samouczku przedstawiono instrukcje tworzenia przykładowego potoku usługi Azure Data Factory przy użyciu Edytora fabryki danych w witrynie Azure Portal."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/01/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 5c4fb60bde7599daa0ac9d7704c398adcdb0d471
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Samouczek: tworzenie pierwszej fabryki danych platformy Azure przy użyciu witryny Azure Portal
> [!div class="op_single_selector"]
> * [Przegląd i wymagania wstępne](data-factory-build-your-first-pipeline.md)
> * [Witryna Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Program Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [Program PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Szablon usługi Resource Manager](data-factory-build-your-first-pipeline-using-arm.md)
> * [Interfejs API REST](data-factory-build-your-first-pipeline-using-rest-api.md)


> [!NOTE]
> Ten artykuł dotyczy wersji 1 usługi Data Factory, która jest ogólnie dostępna (GA). Jeśli używasz wersji 2 usługi Data Factory, która jest w wersji zapoznawczej, zobacz [Szybki start: tworzenie fabryki danych przy użyciu usługi Azure Data Factory w wersji 2](../quickstart-create-data-factory-dot-net.md).

Ten artykuł zawiera instrukcje korzystania z witryny [Azure Portal](https://portal.azure.com/) w celu utworzenia pierwszej fabryki danych Azure. Aby wykonać instrukcje z tego samouczka przy użyciu innych narzędzi/zestawów SDK, wybierz jedną z opcji z listy rozwijanej. 

Potok w tym samouczku zawiera jedno działanie: **działanie Hive usługi HDInsight**. To działanie uruchamia skrypt Hive w klastrze Azure HDInsight, który przekształca dane wejściowe, aby wygenerować dane wyjściowe. Uruchamianie potoku zaplanowano raz w miesiącu między określonym czasem rozpoczęcia i zakończenia. 

> [!NOTE]
> Potok danych przedstawiony w tym samouczku przekształca dane wejściowe w celu wygenerowania danych wyjściowych. Aby zapoznać się z samouczkiem dotyczącym kopiowania danych przy użyciu usługi Azure Data Factory, zobacz [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Samouczek: Kopiowanie danych z usługi Blob Storage do usługi SQL Database).
> 
> Potok może obejmować więcej niż jedno działanie. Dwa działania można połączyć w łańcuch (uruchomić jedno działanie po drugim), ustawiając wyjściowy zestaw danych jednego działania jako zestaw wejściowy drugiego. Więcej informacji znajduje się w artykule dotyczącym [planowania i wykonywania w usłudze Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

## <a name="prerequisites"></a>Wymagania wstępne
1. Przeczytanie artykułu [Omówienie samouczka](data-factory-build-your-first-pipeline.md) oraz wykonanie kroków **wymagań wstępnych**.
2. Ten artykuł nie zawiera omówienia koncepcyjnego usługi Azure Data Factory. Aby zapoznać się ze szczegółowym omówieniem usługi, zalecamy przeczytanie artykułu [Wprowadzenie do usługi Fabryka danych Azure](data-factory-introduction.md).  

## <a name="create-data-factory"></a>Tworzenie fabryki danych
Fabryka danych może obejmować jeden lub wiele potoków. Potok może obejmować jedno lub wiele działań. Na przykład działanie kopiowania może służyć do skopiowania danych ze źródła do docelowego magazynu danych, a działanie programu Hive w usłudze HDInsight do uruchomienia skryptu programu Hive, który przekształci dane wejściowe w dane wyjściowe produktu. Zacznijmy tworzenie fabryki danych w tym kroku.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Kliknij przycisk **NOWY** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij przycisk **Data Factory**.

   ![Blok tworzenia](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. W bloku **Nowa fabryka danych** wprowadź **GetStartedDF** w polu nazwy.

   ![Blok Nowa fabryka danych](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli wystąpi błąd: **Nazwa fabryki danych „GetStartedDF” nie jest dostępna**. Zmień nazwę fabryki danych (na przykład twojanazwaGetStartedDF) i ponów próbę utworzenia. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.
   >
   > W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa **DNS**, a wówczas stanie się widoczna publicznie.
   >
   >
4. Wybierz **subskrypcję Azure**, w ramach której chcesz utworzyć fabrykę danych.
5. Wybierz istniejącą **grupę zasobów** lub utwórz grupę zasobów. Na potrzeby tego samouczka utwórz grupę zasobów o nazwie: **ADFGetStartedRG**.
6. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście rozwijanej są wyświetlane tylko regiony obsługiwane przez usługę Data Factory.
7. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**. 
8. Kliknij przycisk **Utwórz** w bloku **Nowa fabryka danych**.

   > [!IMPORTANT]
   > Aby utworzyć wystąpienia usługi Data Factory, użytkownik musi być członkiem roli [współautora usługi Data Factory](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na poziomie subskrypcji/grupy zasobów.
   >
   >
7. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: Wdrażanie fabryki danych.    

   ![Stan tworzenia fabryki danych](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Gratulacje! Udało Ci się utworzyć pierwszą fabrykę danych. Po pomyślnym utworzeniu fabryki danych zostanie wyświetlona strona fabryki danych z zawartością fabryki danych.     

    ![Blok Fabryka danych](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Przed utworzeniem potoku w fabryce danych musisz utworzyć kilka jednostek usługi Data Factory. Najpierw utwórz połączone usługi, aby połączyć usługi magazynu danych/usługi obliczeniowe ze swoim magazynem danych, zdefiniuj wejściowe i wyjściowe zestawy danych do reprezentowania danych wejściowych/wyjściowych w połączonych magazynach danych, a następnie utwórz potok zawierający działanie, które korzysta z tych zestawów danych.

## <a name="create-linked-services"></a>Tworzenie połączonych usług
W tym kroku opisano połączenie konta usługi Azure Storage oraz klastra Azure HDInsight na żądanie z fabryką danych. Konto usługi Azure Storage będzie przechowywać dane wejściowe i wyjściowe dla potoku w tym przykładzie. Połączona usługa HDInsight służy do uruchamiania skryptu programu Hive określonego w działaniu potoku w tym przykładzie. Zidentyfikuj usługi [magazynu danych](data-factory-data-movement-activities.md)/[usługi obliczeniowe](data-factory-compute-linked-services.md), które są używane w tym scenariuszu, oraz połącz te usługi z fabryką danych przez utworzenie połączonych usług.  

### <a name="create-azure-storage-linked-service"></a>Tworzenie połączonej usługi Azure Storage
W tym kroku opisano łączenie konta usługi Azure Storage z fabryką danych. Na potrzeby tego samouczka do przechowywania danych wejściowych/wyjściowych oraz pliku skryptu HQL używa się tego samego konta usługi Azure Storage.

1. Kliknij przycisk **Utwórz i wdróż** w bloku **FABRYKA DANYCH** dla pozycji **GetStartedDF**. Powinien zostać uruchomiony Edytor fabryki danych.

   ![Kafelek Utwórz i wdróż](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. Kliknij przycisk **Nowy magazyn danych** i wybierz opcję **Azure Storage**.

   ![Nowy magazyn danych — Azure Storage — menu](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. Powinien zostać wyświetlony skrypt JSON do tworzenia połączonej usługi Azure Storage w edytorze.

   ![Połączona usługa Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Zastąp wartość **account name** nazwą konta usługi Azure Storage oraz wartość **account key** kluczem dostępu konta usługi Azure Storage. Aby dowiedzieć się, jak uzyskać klucz dostępu do magazynu, zapoznaj się z informacjami na temat sposobów wyświetlania, kopiowania i ponownego generowania kluczy dostępu do magazynu w sekcji [Zarządzanie kontem magazynu](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć połączoną usługę.

    ![Przycisk Wdróż](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Po pomyślnym wdrożeniu połączonej usługi okno **Wersja robocza-1** powinno zniknąć i w widoku drzewa po lewej stronie zostanie wyświetlona pozycja **AzureStorageLinkedService**.

    ![Połączona usługa Storage w menu](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Tworzenie połączonej usługi Azure HDInsight
W tym kroku przedstawiono łączenie klastra usługi HDInsight na żądanie z fabryką danych. Klaster usługi HDInsight jest automatycznie tworzony w czasie wykonywania oraz usuwany po zakończeniu przetwarzania i określonym czasie bezczynności.

1. W **Edytorze fabryki danych** kliknij opcję **... więcej**, kliknij przycisk **Nowe obliczenie** i wybierz **Klaster usługi HDInsight na żądanie**.

    ![Nowe obliczenie](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Skopiuj i wklej poniższy fragment kodu do okna **Wersja robocza-1**. Fragment kodu JSON opisuje właściwości, które zostaną użyte do utworzenia klastra usługi HDInsight na żądanie.

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    Poniższa tabela zawiera opis właściwości kodu JSON użytych w tym fragmencie kodu:

   | Właściwość | Opis |
   |:--- |:--- |
   | ClusterSize |Określa rozmiar klastra usługi HDInsight. |
   | TimeToLive | Określa czas bezczynności, po którym klaster usługi HDInsight zostanie usunięty. |
   | linkedServiceName | Określa konto magazynu używane do przechowywania dzienników generowanych w usłudze HDInsight. |

    Pamiętaj o następujących kwestiach:

   * Usługa Data Factory tworzy klaster usługi HDInsight **oparty na systemie Linux** za pomocą kodu JSON. Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie).
   * Możesz użyć **własnego klastra usługi HDInsight** zamiast klastra usługi HDInsight na żądanie. Szczegółowe informacje znajdują się w artykule [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (Połączona usługa HDInsight).
   * Klaster usługi HDInsight tworzy **kontener domyślny** w magazynie obiektów blob określonym w kodzie JSON (**linkedServiceName**). Usługa HDInsight nie powoduje usunięcia tego kontenera w przypadku usunięcia klastra. To zachowanie jest celowe. W przypadku połączonej usługi HDInsight na żądanie klaster usługi HDInsight jest tworzony przy każdym przetwarzaniu wycinka — o ile w tym momencie nie istnieje aktywny klaster (**timeToLive**). Klaster jest automatycznie usuwany po zakończeniu przetwarzania.

       Po przetworzeniu większej liczby wycinków w usłudze Azure Blob Storage będzie widocznych wiele kontenerów. Jeśli nie są potrzebne do rozwiązywania problemów z zadaniami, można je usunąć, aby zmniejszyć koszt przechowywania. Nazwy tych kontenerów są zgodne ze wzorcem: „adf**twojanazwafabrykidanych**-**nazwapołączonejusługi**-znacznikdatygodziny”. Aby usunąć kontenery z usługi Azure Blob Storage, użyj takich narzędzi, jak [Microsoft Storage Explorer](http://storageexplorer.com/).

     Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie).
3. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć połączoną usługę.

    ![Wdrażanie połączonej usługi HDInsight na żądanie](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Upewnij się, że w widoku drzewa po lewej stronie wyświetlane są obie pozycje **AzureStorageLinkedService** oraz **HDInsightOnDemandLinkedService**.

    ![Widok drzewa z połączonymi usługami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Tworzenie zestawów danych
W tym kroku opisano tworzenie zestawów danych do reprezentowania danych wejściowych i wyjściowych na potrzeby przetwarzania przy użyciu programu Hive. Te zestawy danych dotyczą elementu **AzureStorageLinkedService** utworzonego wcześniej w ramach tego samouczka. Połączona usługa wskazuje konto usługi Azure Storage, a zestawy danych określają kontener, folder i nazwę pliku w magazynie, w którym przechowywane są dane wejściowe i wyjściowe.   

### <a name="create-input-dataset"></a>Tworzenie wejściowego zestawu danych
1. W **Edytorze fabryki danych** kliknij opcję **... więcej** na pasku poleceń, kliknij opcję **Nowy zestaw danych** i wybierz opcję **Azure Blob Storage**.

    ![Nowy zestaw danych](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Skopiuj i wklej poniższy fragment kodu do okna Wersja robocza-1. Za pomocą tego fragmentu kodu JSON tworzysz zestaw danych o nazwie **AzureBlobInput**, który reprezentuje dane wejściowe dla działania w potoku. Ponadto określasz, że dane wejściowe znajdują się w kontenerze obiektów blob o nazwie **adfgetstarted** oraz folderze o nazwie **inputdata**.

    ```JSON
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
    ```
    Poniższa tabela zawiera opis właściwości kodu JSON użytych w tym fragmencie kodu:

   | Właściwość | Opis |
   |:--- |:--- |
   | type |Właściwość typu jest ustawiona na wartość **AzureBlob**, ponieważ dane znajdują się w magazynie obiektów blob na platformie Azure. |
   | linkedServiceName |Odnosi się do utworzonego wcześniej elementu **AzureStorageLinkedService** . |
   | folderPath | Określa **kontener** obiektów blob oraz **folder**, który zawiera wejściowe obiekty blob. | 
   | fileName |Ta właściwość jest opcjonalna. Jeśli tę właściwość pominiesz, zostaną wybrane wszystkie pliki z folderu folderPath. W tym samouczku zostanie przetworzony tylko plik **input.log**. |
   | type |Pliki dziennika są w formacie tekstowym, więc używana jest wartość **TextFormat**. |
   | columnDelimiter |Kolumny w plikach dziennika są rozdzielane **znakiem przecinka (`,`)** |
   | frequency/interval |Właściwość frequency (częstotliwość) jest ustawiona na wartość **Miesiąc**, a wartość interwału wynosi **1**, co oznacza, że wycinki wejściowe są dostępne co miesiąc. |
   | external | Ta właściwość ma wartość **true** (prawda), jeśli dane wejściowe nie są generowane przez ten potok. W tym samouczku plik input.log nie jest generowany w tym potoku, dlatego możemy ustawić właściwość na true. |

    Aby uzyskać więcej informacji o tych właściwościach JSON, zobacz [artykuł dotyczący łącznika obiektu blob platformy Azure](data-factory-azure-blob-connector.md#dataset-properties).
3. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć nowo utworzony zestaw danych. Zestaw danych powinien zostać wyświetlony w widoku drzewa po lewej stronie.

### <a name="create-output-dataset"></a>Tworzenie wyjściowego zestawu danych
Teraz utworzysz wyjściowy zestaw danych do reprezentowania danych wyjściowych przechowywanych w usłudze Azure Blob Storage.

1. W **Edytorze fabryki danych** kliknij opcję **... więcej** na pasku poleceń, kliknij opcję **Nowy zestaw danych** i wybierz opcję **Azure Blob Storage**.  
2. Skopiuj i wklej poniższy fragment kodu do okna Wersja robocza-1. Za pomocą tego fragmentu kodu JSON tworzysz zestaw danych o nazwie **AzureBlobOutput** i określasz strukturę danych, które są generowane przy użyciu skryptu programu Hive. Ponadto określasz, że wyniki są przechowywane w kontenerze obiektów blob o nazwie **adfgetstarted** oraz folderze o nazwie **partitioneddata**. W sekcji **availability** (dostępność) określono, że wyjściowy zestaw danych jest generowany co miesiąc.

    ```JSON
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
    ```
    Opisy tych właściwości znajdują się w części **Tworzenie wejściowego zestawu danych**. Dla wyjściowego zestawu danych nie ustawia się właściwości external (zewnętrzne), gdyż zestaw danych jest generowany w usłudze Fabryka danych.
3. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć nowo utworzony zestaw danych.
4. Sprawdź, czy zestaw danych został utworzony pomyślnie.

    ![Widok drzewa z połączonymi usługami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Tworzenie potoku
W tym kroku opisano tworzenie pierwszego potoku za pomocą działania **HDInsightHive**. Wycinek danych wejściowych jest dostępny co miesiąc (frequency: Month, interval: 1), wycinek danych wyjściowych jest generowany co miesiąc, a właściwość scheduler dla działania jest również ustawiona na wartość miesięczną. Ustawienia dla wyjściowego zestawu danych i harmonogramu działania muszą być zgodne. W tym przypadku wyjściowy zestaw danych jest elementem wpływającym na ustawienia harmonogramu, więc musisz utworzyć wyjściowy zestaw danych nawet wtedy, gdy działanie nie generuje żadnych danych wyjściowych. Jeśli w działaniu nie są używane żadne dane wejściowe, możesz pominąć tworzenie zestawu danych wejściowych. Właściwości użyte w poniższym fragmencie kodu JSON zostały wyjaśnione na końcu tej sekcji.

1. W **Edytorze fabryki danych** kliknij opcję **Wielokropek (…) Więcej poleceń**, a następnie kliknij opcję **Nowy potok**.

    ![Przycisk nowy potok](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Skopiuj i wklej poniższy fragment kodu do okna Wersja robocza-1.

   > [!IMPORTANT]
   > Zastąp wartość **storageaccountname** w kodzie JSON nazwą swojego konta magazynu.
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
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
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    Ten fragment kodu JSON służy do utworzenia potoku obejmującego jedno działanie, które korzysta z programu Hive do przetwarzania danych w klastrze usługi HDInsight.

    Plik skryptu programu Hive **partitionweblogs.hql** jest przechowywany na koncie usługi Azure Storage (określonym za pomocą elementu scriptLinkedService o nazwie **AzureStorageLinkedService**) oraz w folderze **script** w kontenerze **adfgetstarted**.

    Sekcja **defines** służy do określania ustawień środowiska uruchomieniowego, które są przekazywane do skryptu programu Hive w formie wartości konfiguracyjnych programu Hive (np. ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).

    Właściwości **start** i **end** potoku określają aktywny okres potoku.

    W kodzie JSON dotyczącym działania określasz, że skrypt programu Hive jest uruchamiany w usłudze obliczeniowej określonej przez właściwość **linkedServiceName** — **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > Aby uzyskać szczegółowe informacje na temat właściwości kodu JSON używanych w przykładzie, zobacz sekcję „Pipeline JSON” (Kod JSON potoku) w temacie [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) (Potoki i działania w usłudze Azure Data Factory).
   >
   >
3. Upewnij się, że:

   1. Plik **input.log** istnieje w folderze **inputdata** kontenera **adfgetstarted** w magazynie obiektów blob Azure.
   2. Plik **partitionweblogs.hql** znajduje się w folderze **script** kontenera **adfgetstarted** w magazynie obiektów blob Azure. Jeśli te pliki nie są wyświetlane, wykonaj wstępnie wymagane kroki w artykule [Omówienie samouczka](data-factory-build-your-first-pipeline.md).
   3. Upewnij się, że element **storageaccountname** został zastąpiony nazwą konta magazynu w kodzie JSON potoku.
4. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć potok. Ponieważ właściwości **start** i **end** są ustawione na wartość w przeszłości i właściwość **isPaused** została ustawiona na wartość „false”, potok (działanie w potoku) jest uruchamiany natychmiast po wdrożeniu.
5. Upewnij się, że potok jest wyświetlany w widoku drzewa.

    ![Widok drzewa z potokiem](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. Gratulacje! Udało Ci się utworzyć pierwszy potok!

## <a name="monitor-pipeline"></a>Monitorowanie potoku
### <a name="monitor-pipeline-using-diagram-view"></a>Monitorowanie potoku przy użyciu widoku diagramu
1. Kliknij przycisk **X**, aby zamknąć bloki Edytora fabryki danych i przejść z powrotem do bloku Fabryka danych, a następnie kliknij przycisk **Diagram**.

    ![Kafelek Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. Na stronie Widok diagramu zostanie wyświetlony przegląd potoków i zestawów danych używanych w tym samouczku.

    ![Widok diagramu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. Aby wyświetlić wszystkie działania w potoku, kliknij prawym przyciskiem myszy potok w diagramie i kliknij polecenie Otwórz potok.

    ![Menu Otwórz potok](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. Upewnij się, że działanie HDInsightHive jest widoczne w potoku.

    ![Widok Otwórz potok](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Aby przejść z powrotem do poprzedniego widoku, kliknij pozycję **Fabryka danych** w menu linków do stron nadrzędnych u góry.
5. Na stronie **Widok diagramu** kliknij dwukrotnie zestaw danych **AzureBlobInput**. Sprawdź, czy wycinek jest w stanie **Gotowe**. Może potrwać kilka minut, zanim wycinek zostanie wyświetlony ze stanem Gotowe. Jeśli poczekasz jakiś czas i tak się nie stanie, sprawdź, czy plik wejściowy (input.log) znajduje się w odpowiednim kontenerze (adfgetstarted) i folderze (inputdata).

   ![Wycinek danych wejściowych w stanie gotowości](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. Kliknij przycisk **X**, aby zamknąć blok **AzureBlobInput**.
7. Na stronie **Widok diagramu** kliknij dwukrotnie zestaw danych **AzureBlobOutput**. Zostanie wyświetlony wycinek, który jest obecnie przetwarzany.

   ![Zestaw danych](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. Po zakończeniu przetwarzania wycinek będzie mieć stan **Gotowe**.

   ![Zestaw danych](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > Tworzenie klastra usługi HDInsight na żądanie zwykle trwa trochę czasu (około 20 minut). Dlatego należy oczekiwać, że przetworzenie wycinka przez potok zajmie **około 30 minut**.
   >
   >

9. Gdy wycinek będzie w stanie **Gotowe**, sprawdź folder **partitioneddata** w kontenerze **adfgetstarted** w magazynie obiektów blob pod kątem danych wyjściowych.  

   ![Dane wyjściowe](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. Kliknij wycinek, aby wyświetlić szczegółowe informacje na jego temat w bloku **Wycinek danych**.

   ![Szczegóły wycinka danych](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Kliknij uruchomienie działania na **liście uruchomień działań**, aby zobaczyć szczegóły dotyczące uruchamiania działania (w tym scenariuszu działanie Hive) w oknie **Szczegóły uruchamiania działania**.   

   ![Szczegóły uruchamiania działania](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   W plikach dziennika zobaczysz zapytanie Hive, które zostało wykonane, oraz informacje o stanie. Dzienniki te są przydatne w przypadku rozwiązywania różnych problemów.
   Więcej szczegółowych informacji znajduje się w artykule [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) (Monitorowanie potoków i zarządzanie nimi przy użyciu bloków w witrynie Azure Portal).

> [!IMPORTANT]
> Po pomyślnym przetworzeniu wycinka plik wejściowy zostanie usunięty. Tak więc, jeśli chcesz ponownie uruchomić wycinek lub ponownie wykonać instrukcje z tego samouczka, przekaż plik wejściowy (input.log) do folderu inputdata kontenera adfgetstarted.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitorowanie potoku przy użyciu aplikacji Monitorowanie i zarządzanie
Do monitorowania potoków danych możesz też użyć aplikacji Monitorowanie i zarządzanie. Szczegółowe informacje dotyczące korzystania z aplikacji znajdują się w artykule [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Monitorowanie potoków usługi Azure Data Factory oraz zarządzanie nimi za pomocą aplikacji Monitorowanie i zarządzanie).

1. Kliknij kafelek **Monitorowanie i zarządzanie** na stronie głównej fabryki danych.

    ![Kafelek Monitorowanie i zarządzanie](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. Powinna zostać wyświetlona **aplikacja Monitorowanie i zarządzanie**. Zmień wartość **Czas rozpoczęcia** i **Czas zakończenia** na czas rozpoczęcia i czas zakończenia potoku, a następnie kliknij przycisk **Zastosuj**.

    ![Aplikacja Monitorowanie i zarządzanie](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Wybierz okno działania z listy **okien działania**, aby zobaczyć szczegółowe informacje o nim.

    ![Szczegóły okna działania](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Podsumowanie
W tym samouczku opisano tworzenie fabryki danych Azure do przetwarzania danych przez uruchomienie skryptu programu Hive w klastrze platformy Hadoop w usłudze HDInsight. Użyto Edytora fabryki danych w witrynie Azure Portal, aby:  

1. Tworzenie **fabryki danych** Azure.
2. Utworzyć dwie **połączone usługi**:
   1. Połączoną usługę **Azure Storage** w celu połączenia magazynu obiektów blob Azure, w którym przechowywane są pliki wejściowe/wyjściowe, z fabryką danych.
   2. Połączoną usługę **Azure HDInsight** na żądanie w celu połączenia klastra platformy Hadoop w usłudze HDInsight na żądanie z fabryką danych. Usługa Fabryka danych Azure tworzy klaster just in time platformy Hadoop w usłudze HDInsight, aby przetwarzać dane wejściowe i generować dane wyjściowe.
3. Utworzyć dwa **zestawy danych** zawierające dane wejściowe i wyjściowe dla działania programu Hive w usłudze HDInsight w potoku.
4. Utworzyć **potok** za pomocą działania **programu Hive w usłudze HDInsight**.

## <a name="next-steps"></a>Następne kroki
W tym artykule opisano tworzenie potoku za pomocą działania przekształcenia (działanie usługi HDInsight), które uruchamia skrypt programu Hive w klastrze usługi HDInsight na żądanie. Instrukcje dotyczące korzystania z działania kopiowania w celu kopiowania danych z magazynu obiektów blob Azure do usług SQL Azure znajdują się w artykule [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Samouczek: kopiowanie danych z magazynu obiektów blob Azure do usług SQL Azure).

## <a name="see-also"></a>Zobacz też
| Temat | Opis |
|:--- |:--- |
| [Potoki](data-factory-create-pipelines.md) |Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory oraz ze sposobem konstruowania za ich pomocą przepływów pracy typu end-to-end opartych na danych na potrzeby scenariusza lub firmy. |
| [Zestawy danych](data-factory-create-datasets.md) |Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) |W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) |Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. |
