---
title: "Kopiowanie danych z programu SQL Server do usługi Blob Storage przy użyciu usługi Azure Data Factory | Microsoft Docs"
description: "Dowiedz się, jak skopiować dane z lokalnego magazynu danych do chmury na platformie Azure przy użyciu własnego środowiska Integration Runtime w usłudze Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: afd7735712d03110a67509a7e94d336219a65b34
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="tutorial-copy-data-from-on-premises-sql-server-to-azure-blob-storage"></a>Samouczek: Kopiowanie danych z lokalnego programu SQL Server do usługi Azure Blob Storage
W tym samouczku użyjesz programu Azure PowerShell, aby utworzyć potok fabryki danych, który kopiuje dane z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage. Utworzysz własne środowisko Integration Runtime (IR) usługi Azure Data Factory i użyjesz go w celu zintegrowania lokalnych magazynów danych oraz magazynów danych w chmurze.  Aby dowiedzieć się więcej o zastosowaniu innych narzędzi/zestawów SDK do tworzenia fabryki danych, zapoznaj się z samouczkami [Szybki start](quickstart-create-data-factory-dot-net.md).

Ten artykuł nie zawiera szczegółowego wprowadzenia do usługi Data Factory. Aby zapoznać się z wprowadzeniem do usługi Azure Data Factory, zobacz [Wprowadzenie do usługi Azure Data Factory](introduction.md). 

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [dokumentację dotyczącą usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Ten samouczek obejmuje następujące procedury:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Utwórz własne środowisko Integration Runtime.
> * Tworzenie połączonych zestawów programu SQL Server i usługi Azure Storage. 
> * Tworzenie zestawów danych programu SQL Server i usługi Azure Blob.
> * Tworzenie potoku z działaniem kopiowania do przenoszenia danych.
> * Uruchamianie potoku.
> * Monitorowanie uruchomienia potoku.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="sql-server-2014-or-2016"></a>Program SQL Server 2014 lub 2016. 
Użyj lokalnej bazy danych programu SQL Server jako **źródła** magazynu danych w tym samouczku. Utwórz tabelę o nazwie **emp** w bazie danych programu SQL Server i wstaw kilka przykładowych wpisów w tabeli.

1. Uruchom program **SQL Server Management Studio**. Jeśli używasz programu SQL Server 2016, może być konieczne zainstalowanie programu SQL Server Management Studio oddzielnie z [centrum pobierania](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms). 
2. Połącz się z serwerem SQL przy użyciu poświadczeń. 
3. Utwórz przykładową bazę danych. W widoku drzewa kliknij prawym przyciskiem myszy pozycję **Bazy danych** i kliknij pozycję **Nowa baza danych**. W oknie dialogowym **Nowa baza danych** wprowadź **nazwę** bazy danych, a następnie kliknij przycisk **OK**. 
4. Uruchom poniższy skrypt zapytania w bazie danych, aby utworzyć tabelę **emp**. W widoku drzewa kliknij prawym przyciskiem myszy utworzoną **bazę danych** i kliknij pozycję **Nowe zapytanie**. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Uruchom następujące polecenia w bazie danych, aby wstawić przykładowe dane w tabeli:

    ```sql
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="azure-storage-account"></a>Konto usługi Azure Storage
W tym samouczku używasz konta usługi Azure Storage ogólnego przeznaczenia (w szczególności usługi Blob Storage) jako magazynu danych: **docelowego/obiektu sink**. Jeśli nie masz konta usługi Azure Storage ogólnego przeznaczenia, zobacz [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account), aby uzyskać informacje o jego tworzeniu.

#### <a name="get-storage-account-name-and-account-key"></a>Pobieranie nazwy konta i klucza konta magazynu
W tym samouczku Szybki start używasz nazwy i klucza konta usługi Azure Storage. Poniższa procedura obejmuje kroki pobierania nazwy i konta klucza magazynu. 

1. Otwórz przeglądarkę internetową i przejdź do witryny [Azure Portal](https://portal.azure.com). Zaloguj się za pomocą nazwy użytkownika i hasła platformy Azure. 
2. Kliknij pozycję **Więcej usług >** w menu po lewej stronie, odfiltruj przy użyciu słowa kluczowego **Storage** i wybierz pozycję **Konta usługi Storage**.

    ![Wyszukiwanie konta magazynu](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. Na liście kont magazynu odfiltruj konto magazynu (w razie potrzeby), a następnie wybierz **swoje konto magazynu**. 
4. Na stronie **Konto magazynu** wybierz pozycję **Klucze dostępu**.

    ![Pobieranie nazwy i klucza konta magazynu](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. Skopiuj wartości pól **Nazwa konta magazynu** i **klucz1** do schowka. Wklej je do Notatnika lub innego edytora i zapisz plik. W samouczku będziesz używać klucza i nazwy konta magazynu. 

#### <a name="create-the-adftutorial-container"></a>Tworzenie kontenera adftutorial 
W tej sekcji utworzysz kontener obiektów blob o nazwie adftutorial w magazynie obiektów blob Azure Blob Storage. 

1. Zainstaluj [Eksploratora usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/), jeśli nie ma go na maszynie. 
2. Uruchom **Eksploratora usługi Microsoft Azure Storage** na swojej maszynie.   
3. W oknie **łączenia z usługą Azure Storage** wybierz pozycję **Użyj nazwy i klucza konta magazynu** i kliknij pozycję **Dalej**. Jeśli nie widzisz okna **łączenia z usługą Azure Storage**, kliknij prawym przyciskiem myszy pozycję **Konta magazynu** w widoku drzewa, a następnie kliknij pozycję **Połącz z usługą Azure Storage**. 

    ![Łączenie z usługą Azure Storage](media/tutorial-hybrid-copy-powershell/storage-explorer-connect-azure-storage.png)
4. W oknie **dołączania przy użyciu nazwy i klucza** wklej **nazwę konta** i **klucz konta**, które zostały zapisane w poprzednim kroku. Następnie kliknij przycisk **Dalej**. 
5. W oknie **Podsumowanie połączenia** kliknij pozycję **Połącz**.
6. Sprawdź, czy widzisz swoje konto magazynu w widoku drzewa w obszarze **(Lokalny i dołączony)** -> **Konta magazynu**. 
7. Rozwiń węzeł **Kontenery obiektów blob** i upewnij się, że kontener obiektów blob o nazwie **adftutorial** nie istnieje. Jeśli już istnieje, możesz pominąć kolejne kroki tworzenia kontenera. 
8. Kliknij prawym przyciskiem myszy pozycję **Kontenery obiektów blob** i wybierz pozycję **Utwórz kontener obiektów blob**.

    ![Tworzenie kontenera obiektów blob](media/tutorial-hybrid-copy-powershell/stroage-explorer-create-blob-container-menu.png)
9. Wprowadź ciąg **adftutorial** jako nazwę i naciśnij klawisz **ENTER**. 
10. Upewnij się, że kontener **adftutorial** został wybrany w widoku drzewa. Usługa Data Factory automatycznie tworzy folder wyjściowy w tym kontenerze, więc Ty nie musisz go tworzyć. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Instalowanie programu Azure PowerShell
Zainstaluj najnowszy program Azure PowerShell, jeśli nie masz go jeszcze na swojej maszynie. 

1. W przeglądarce internetowej przejdź do strony [plików do pobrania zestawu Azure SDK i innych zestawów SDK](https://azure.microsoft.com/downloads/). 
2. Kliknij pozycję **Wersja instalacyjna dla systemu Windows** w sekcji **Narzędzia wiersza polecenia** -> **PowerShell**. 
3. Aby zainstalować program Azure PowerShell, uruchom plik **MSI**. 

Aby uzyskać szczegółowe informacje, zobacz [How to install and configure Azure PowerShell (Jak zainstalować i skonfigurować program Azure PowerShell)](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Logowanie do programu Azure PowerShell
Uruchom program **PowerShell** na maszynie. Nie zamykaj programu Azure PowerShell aż do końca tego samouczka Szybki start. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.

1. Uruchom poniższe polecenie i wprowadź nazwę użytkownika platformy Azure oraz hasło, których używasz do logowania się w witrynie Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Jeśli masz wiele subskrypcji platformy Azure, uruchom poniższe polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Uruchom poniższe polecenie, aby wybrać subskrypcję, z którą chcesz pracować. Zastąp parametr **SubscriptionId** identyfikatorem Twojej subskrypcji platformy Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Zdefiniuj zmienną nazwy grupy zasobów, której użyjesz później w poleceniach programu PowerShell. Skopiuj poniższy tekst polecenia do programu PowerShell, podaj nazwę [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) w podwójnych cudzysłowach, a następnie uruchom polecenie. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Zdefiniuj zmienną nazwy fabryki danych, której możesz użyć później w poleceniach programu PowerShell. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Zdefiniuj zmienną lokalizacji fabryki danych: 

    ```powershell
    $location = "East US"
    ```
4. Aby utworzyć grupę zasobów platformy Azure, uruchom następujące polecenie: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Jeśli grupa zasobów już istnieje, możesz zrezygnować z jej zastąpienia. Przypisz inną wartość do zmiennej `$resourceGroupName` i spróbuj ponownie. Jeśli chcesz udostępnić grupę zasobów innym użytkownikom, przejdź do następnego kroku.  
5. Aby utworzyć fabrykę danych, uruchom następujące polecenie cmdlet **Set-AzureRmDataFactoryV2**: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Pamiętaj o następujących kwestiach:

* Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli zostanie wyświetlony następujący błąd, zmień nazwę i spróbuj ponownie.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Aby tworzyć wystąpienia usługi Data Factory, musisz być **współautorem** lub **administratorem** subskrypcji platformy Azure.
* Obecnie usługa Data Factory w wersji 2 umożliwia tworzenie fabryk danych tylko w regionach Wschodnie stany USA, Wschodnie stany USA 2 i Europa Zachodnia. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych regionach.

## <a name="create-a-self-hosted-ir"></a>Tworzenie własnego środowiska IR

W tej sekcji możesz utworzyć własne środowisko Integration Runtime i skojarzyć je z węzłem lokalnym (maszyną).

1. Utwórz zmienną dla nazwy środowiska Integration Runtime. 

    ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
    ```
1. Utwórz własne środowisko Integration Runtime. Użyj unikatowej nazwy w przypadku, gdy istnieje inne środowisko Integration Runtime o takiej samej nazwie.

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Oto przykładowe dane wyjściowe:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Uruchom następujące polecenie, aby pobrać stan utworzonego środowiska Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Oto przykładowe dane wyjściowe:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Uruchom następujące polecenie, aby pobrać klucze uwierzytelniania w celu zarejestrowania własnego środowiska Integration Runtime za pomocą usługi Data Factory w chmurze. Skopiuj jeden z kluczy w celu zarejestrowania środowiska Integration Runtime (Self-hosted).

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Oto przykładowe dane wyjściowe:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

## <a name="install-integration-runtime"></a>Instalowanie środowiska Integration Runtime
1. [Pobierz](https://www.microsoft.com/download/details.aspx?id=39717) środowisko Integration Runtime (Self-hosted) na lokalną maszynę z systemem Windows, a następnie uruchom instalację. 
2. Na **stronie powitalnej Kreatora instalacji środowiska Microsoft Integration Runtime** kliknij przycisk **Dalej**.  
3. Na stronie **postanowień licencyjnych** zaakceptuj warunki i umowę licencyjną, a kliknij pozycję **Dalej**. 
4. Na stronie **Folder docelowy** kliknij pozycję **Dalej**. 
5. Na stronie **Gotowe do zainstalowania środowiska Microsoft Integration Runtime** kliknij pozycję **Zainstaluj**. 
6. Jeśli zostanie wyświetlony komunikat ostrzegawczy informujący o tym, że konfigurowany komputer przejdzie w tryb uśpienia lub hibernacji, jeśli nie będzie używany, kliknij przycisk **OK**. 
7. Na stronie **zakończenia pracy z Kreatorem instalacji środowiska Microsoft Integration Runtime** kliknij pozycję **Zakończ**.
8. Na stronie **rejestracji środowiska Integration Runtime (Self-hosted)** wklej klucz zapisany w poprzedniej sekcji i kliknij pozycję **Zarejestruj**. 

   ![Rejestrowanie środowiska Integration Runtime](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)
2. Gdy własne środowisko Integration Runtime zostanie pomyślnie zarejestrowane, zostanie wyświetlony następujący komunikat:

   ![Pomyślnie zarejestrowano](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

3. Na stronie **nowego węzła środowiska Integration Runtime (Self-hosted)** kliknij pozycję **Dalej**. 

    ![Strona nowego węzła środowiska Integration Runtime (Self-hosted)](media/tutorial-hybrid-copy-powershell/new-integration-runtime-node-page.png)
4. Na stronie **kanału komunikacyjnego Intranet** kliknij pozycję **Pomiń**. Możesz wybrać certyfikację protokołów TLS/SSL w celu zabezpieczenia komunikacji w środowisku Integration Runtime z wieloma węzłami. 

    ![Strona kanału komunikacyjnego Intranet](media/tutorial-hybrid-copy-powershell/intranet-communication-channel-page.png)
5. Na stronie **rejestrowania środowiska Integration Runtime (Self-hosted)** kliknij pozycję **Uruchom program Configuration Manager**. 
6. Gdy węzeł zostanie połączony z usługą w chmurze, zostanie wyświetlona następująca strona:

   ![Węzeł jest połączony](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Tworzenie połączonych usług

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Tworzenie połączonej usługi Azure Storage (obiektu docelowego/ujścia)

1. Utwórz plik JSON o nazwie **AzureStorageLinkedService.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości: (utwórz folder ADFv2Tutorial, jeśli jeszcze nie istnieje).  

    > [!IMPORTANT]
    > Przed zapisaniem pliku zastąp wartości &lt;accountName&gt; i &lt;accountKey&gt; nazwą i kluczem konta usługi Azure Storage.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```
2. W programie **Azure PowerShell** przejdź do folderu **ADFv2Tutorial**.

   Uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2LinkedService**, aby utworzyć połączoną usługę: **AzureStorageLinkedService**. Polecenia cmdlet użyte w tym samouczku pobierają wartości dla parametrów **ResourceGroupName** i **DataFactoryName**. Możesz też przekazać obiekt **DataFactory** zwrócony przez polecenie cmdlet Set-AzureRmDataFactoryV2 bez wpisywania parametrów ResourceGroupName i DataFactoryName za każdym razem, gdy uruchamiasz polecenie cmdlet.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Oto przykładowe dane wyjściowe:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Tworzenie i szyfrowanie połączonej usługi SQL Server (źródło)

1. Utwórz plik JSON o nazwie **SqlServerLinkedService.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości: Zastąp zmienne **&lt;servername >**, **&lt;databasename>**, **&lt;username>**, **&lt;servername >** i **&lt;password>** wartościami Twojego programu SQL Server przed zapisaniem pliku. 

    > [!IMPORTANT]
    > Zastąp zmienną **&lt;integration** **runtime** **name>** nazwą Twojego środowiska Integration Runtime.

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. W celi zaszyfrowania poufnych danych z ładunku w formacie JSON w lokalnym środowisku Integration Runtime (Self-hosted) uruchom polecenie **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** i przekaż powyższy ładunek w formacie JSON. To szyfrowanie zapewnia szyfrowanie poświadczeń za pomocą interfejsu API ochrony danych (DPAPI). Zaszyfrowane poświadczenia są przechowywane lokalnie w węźle środowiska Integration Runtime (Self-hosted) (maszyna lokalna). Ładunek danych wyjściowych może zostać przekierowany do innego pliku JSON (w tym przypadku „encryptedLinkedService.json”), który zawiera zaszyfrowane poświadczenia.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Uruchom następujące polecenie przy użyciu pliku JSON z poprzedniego kroku, aby utworzyć usługę **SqlServerLinkedService**:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Tworzenie zestawów danych
W tym kroku utworzysz zestawy danych wejściowych i wyjściowych, które reprezentują dane wejściowe i wyjściowe operacji kopiowania (lokalna baza danych programu SQL Server => usługa Azure Blob Storage).

### <a name="create-a-dataset-for-source-sql-database"></a>Tworzenie zestawu danych źródłowej bazy danych SQL Database

1. Utwórz plik JSON o nazwie **SqlServerDataset.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. Aby utworzyć zestaw danych **SqlServerDataset**, uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Dataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Tworzenie zestawu danych ujścia usługi Azure Blob Storage

1. Utwórz plik JSON o nazwie **AzureBlobDataset.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości:

    > [!IMPORTANT]
    > W tym przykładzie kodu założono, że masz kontener o nazwie **adftutorial** w usłudze Azure Blob Storage.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. Aby utworzyć zestaw danych **AzureBlobDataset**, uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Dataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>Tworzenie potoków

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>Tworzenie potoku „SqlServerToBlobPipeline”

1. Utwórz plik JSON o nazwie **SqlServerToBlobPipeline.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. W celu utworzenia potoku **SQLServerToBlobPipeline** uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Pipeline**.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>Uruchamianie i monitorowanie działania potoku

1. Uruchom działanie potoku dla potoku „SQLServerToBlobPipeline” i zarejestruj identyfikator działania potoku do monitorowania w przyszłości.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Uruchom następujący skrypt, aby stale sprawdzać stan działania potoku „**SQLServerToBlobPipeline**” i wydrukować wynik końcowy.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Oto dane wyjściowe przykładowego uruchomienia:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. Aby uzyskać identyfikator działania potoku „**SQLServerToBlobPipeline**” i sprawdzić szczegółowe wyniki działania, wykonaj poniższe instrukcje.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Oto dane wyjściowe przykładowego uruchomienia:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 3,
      "throughput": 0.01171875,
      "errors": [],
      "effectiveIntegrationRuntime": "MyIntegrationRuntime",
      "billedDuration": 3
    }
    ```
## <a name="verify-the-output"></a>Sprawdzanie danych wyjściowych
Potok automatycznie tworzy folder wyjściowy o nazwie `fromonprem` w kontenerze obiektów blob `adftutorial`. Upewnij się, że plik **dbo.emp.txt** jest widoczny w folderze wyjściowym. Za pomocą [Eksploratora usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/) sprawdź, czy utworzono dane wyjściowe. 

## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Utwórz własne środowisko Integration Runtime.
> * Tworzenie połączonych zestawów programu SQL Server i usługi Azure Storage. 
> * Tworzenie zestawów danych programu SQL Server i usługi Azure Blob.
> * Tworzenie potoku z działaniem kopiowania do przenoszenia danych.
> * Uruchamianie potoku.
> * Monitorowanie uruchomienia potoku.

Lista magazynów danych obsługiwanych przez usługę Azure Data Factory znajduje się w artykule dotyczącym [obsługiwanych magazynów danych](copy-activity-overview.md#supported-data-stores-and-formats).

Przejdź do poniższego samouczka, aby dowiedzieć się o zbiorczym kopiowaniu danych z lokalizacji źródłowej do docelowej:

> [!div class="nextstepaction"]
>[Zbiorcze kopiowanie danych](tutorial-bulk-copy.md)
