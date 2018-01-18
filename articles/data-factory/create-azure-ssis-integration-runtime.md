---
title: "Tworzenie środowiska uruchomieniowego integracji siebie w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak SQL Server działania dotyczącego procedury składowanej umożliwia wywołanie procedury przechowywanej w bazie danych SQL Azure lub usługi Azure SQL Data Warehouse z potoku fabryki danych."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: a5ed3cbfac0b86cedde5718cef4231a7fcc36f2e
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Tworzenie środowiska uruchomieniowego integracji usług SSIS Azure w fabryce danych Azure
Ten artykuł zawiera kroki do inicjowania obsługi środowiska uruchomieniowego integracji usług SSIS Azure w fabryce danych Azure. Następnie możesz użyć programu SQL Server Data Tools (SSDT) lub SQL Server Management Studio (SSMS) do wdrożenia pakietów usług SQL Server Integration Services (SSIS) w tym środowisku uruchomieniowym na platformie Azure.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [dokumentację dotyczącą usługi Data Factory w wersji 1](v1/data-factory-introduction.md).

Samouczek: [samouczek: Wdrażanie pakietów usług SQL Server Integration Services (SSIS) na platformie Azure](tutorial-deploy-ssis-packages-azure.md) pokazuje, jak utworzyć środowiska uruchomieniowego integracji usług SSIS Azure (IR) przy użyciu bazy danych SQL Azure do przechowywania katalogu usług SSIS. W tym artykule rozszerzenie samouczka i pokazuje, jak wykonać następujące czynności: 

- Używać wystąpienia zarządzane Azure SQL (wersja zapoznawcza prywatne) do obsługi katalogu SSIS (baza danych usług SSIS).
- Dołącz IR Azure SSIS do sieci wirtualnej platformy Azure (VNet). 

Aby uzyskać informacje koncepcyjne na dołączenie IR Azure SSIS do sieci wirtualnej i konfigurowanie sieci wirtualnej w portalu Azure, zobacz [Join IR Azure SSIS do sieci wirtualnej](join-azure-ssis-integration-runtime-virtual-network.md). 

## <a name="prerequisites"></a>Wymagania wstępne

- **Subskrypcja platformy Azure**. Jeśli nie masz subskrypcji, możesz utworzyć konto [bezpłatnej wersji próbnej](http://azure.microsoft.com/pricing/free-trial/).
- **Serwer usługi Azure SQL Database** lub **zarządzane wystąpienie programu SQL Server (prywatna wersja zapoznawcza) (rozszerzona prywatna wersja zapoznawcza)**. Jeśli nie masz jeszcze serwera bazy danych, utwórz go w witrynie Azure Portal, zanim zaczniesz. Ten serwer hostuje bazę danych katalogu usług SSIS (SSISDB). Zaleca się utworzenie serwera bazy danych w tym samym regionie platformy Azure, co środowisko Integration Runtime. Ta konfiguracja pozwala środowisku Integration Runtime zapisywać dzienniki wykonywania SSISDB bez wykraczania poza granice regionów świadczenia usług platformy Azure. Zapamiętaj warstwy cenowej serwera Azure SQL. Aby uzyskać listę obsługiwanych warstw cenowych bazy danych SQL Azure, zobacz [limity zasobów bazy danych SQL](../sql-database/sql-database-resource-limits.md).
- **Klasyczna sieć wirtualna (opcjonalna)**. Jeśli co najmniej jeden z poniższych warunków jest spełniony, musisz mieć sieć Azure Virtual Network:
    - Hostujesz bazę danych katalogu usług SSIS na wystąpieniu zarządzanym programu SQL Server (prywatna wersja zapoznawcza), który jest częścią sieci wirtualnej.
    - Chcesz połączyć się z lokalnymi magazynami danych z pakietów usług SSIS działającymi w środowisku Azure SSIS Integration Runtime.
- Zainstalowanie programu **Azure PowerShell**. Wykonaj instrukcje podane w temacie [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps). Program PowerShell służy do uruchamiania skryptu w celu aprowizacji środowiska Azure SSIS Integration Runtime, które uruchamia pakiety SSIS w chmurze. 

> [!NOTE]
> Aby uzyskać listę regionów obsługiwanych przez usługę Azure Data Factory w wersji 2 i środowisko Azure SSIS Integration Runtime, zobacz [Dostępność produktów według regionów](https://azure.microsoft.com/regions/services/). Rozwiń pozycję **Dane + analiza**, aby wyświetlić usługi **Data Factory V2** i **SSIS Integration Runtime**.

## <a name="azure-portal"></a>Azure Portal

### <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).    
2. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. W **nowa fabryka danych** wprowadź **MyAzureSsisDataFactory** dla **nazwa**. 
      
     ![Nowa strona fabryki danych](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli zostanie wyświetlony następujący błąd, Zmień nazwę fabryki danych (na przykład yournameMyAzureSsisDataFactory), a następnie spróbuj ponownie utworzyć. Zobacz [fabryki danych - reguły nazewnictwa](naming-rules.md) artykułu dla reguł nazewnictwa artefakty fabryki danych.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
4. Dla opcji **Grupa zasobów** wykonaj jedną z następujących czynności:
     
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
      Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
4. Wybierz wartość **V2 (wersja zapoznawcza)** dla **wersji**.
5. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście są wyświetlane tylko te lokalizacje, które są obsługiwane w celu utworzenia fabryki danych.
6. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.     
7. Kliknij przycisk **Utwórz**.
8. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: **Wdrażanie fabryki danych**. 

    ![kafelek Wdrażanie fabryki danych](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Po zakończeniu tworzenia zobacz **fabryki danych** strony, jak pokazano w obrazie.
   
   ![Strona główna fabryki danych](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Kliknij przycisk **autora & Monitor** można uruchomić danych fabryki użytkownika Interfejsu w osobnej karcie. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Zapewnij środowiska uruchomieniowego integracji Azure SSIS

1. W strony wprowadzenie, kliknij przycisk **Konfigurowanie obsługi integracji usług SSIS** kafelka. 

   ![Konfigurowanie środowiska uruchomieniowego integracji usług SSIS kafelka](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. W **ustawienia ogólne** strony **ustawień środowiska uruchomieniowego integracji**, wykonaj następujące czynności: 

   ![Ustawienia ogólne](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Określ **nazwa** integrację środowiska uruchomieniowego.
    2. Wybierz **lokalizacji** integrację środowiska uruchomieniowego. Wyświetlane są tylko obsługiwane lokalizacje.
    3. Wybierz **rozmiar węzła** który ma zostać skonfigurowana ze środowiskiem uruchomieniowym SSIS.
    4. Określ **liczba węzłów** w klastrze.
    5. Kliknij przycisk **Dalej**. 
1. W **ustawienia SQL**, wykonaj następujące czynności: 

    ![Ustawienia SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Określ platformy Azure **subskrypcji** mający serwer Azure SQL. 
    2. Wybierz serwer Azure SQL w celu **punkt końcowy serwera bazy danych katalogu**.
    3. Wprowadź **administratora** nazwy użytkownika.
    4. Wprowadź **hasło** dla administratora.  
    5. Wybierz **warstwy usług** dla bazy danych usług SSIS. Wartość domyślna to Basic.
    6. Kliknij przycisk **Dalej**. 
1.  W **Zaawansowane ustawienia** wybierz wartość dla **maksymalna równoległych wykonaniami każdego węzła**.   

    ![Ustawienia zaawansowane](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Ten krok jest **opcjonalne**. Jeśli masz klasycznego sieć wirtualną (VNet), który chcesz dołączyć, wybierz środowiska uruchomieniowego integracji **wybierz sieć wirtualną dla Twojego środowiska uruchomieniowego integracji usług SSIS Azure do przyłączania a Zezwalaj usługom platformy Azure skonfigurować ustawienia uprawnień sieci wirtualnej** Opcja i wykonaj następujące czynności: 

    ![Zaawansowane ustawienia z sieci wirtualnej](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Określ **subskrypcji** mający klasycznej sieci wirtualnej. 
    2. Wybierz **sieci wirtualnej**. <br/>
    4. Wybierz **podsieci**.<br/> 
1. Kliknij przycisk **Zakończ** zacząć od utworzenia środowiska uruchomieniowego integracji usług SSIS Azure. 

    > [!IMPORTANT]
    > - Ten proces trwa około 20 minut
    > - Usługi fabryka danych łączy się z bazą danych SQL Azure do przygotowania bazy danych usług SSIS katalogu (SSISDB). Skrypt konfiguruje również uprawnienia i ustawienia Twojej sieci wirtualnej, jeśli zostanie określona, i dołącza nowe wystąpienie środowiska Azure SSIS Integration Runtime do sieci wirtualnej.
7. W **połączeń** okna, Przełącz do **środowisk uruchomieniowych integracji** w razie potrzeby. Kliknij przycisk **Odśwież** Aby odświeżyć stan. 

    ![Stan tworzenia](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Użyj łączy w obszarze **akcje** kolumny do monitorowania, zatrzymywać i zacząć, edytować lub usunąć środowiska uruchomieniowego integracji. Umożliwia wyświetlenie kodu JSON dla środowiska uruchomieniowego integracji ostatnie łącze. Edytowanie i usuwanie przycisków są włączone tylko wtedy, gdy IR jest zatrzymana. 

    ![Azure IR SSIS - akcje](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Kliknij przycisk **Monitor** łącze w obszarze **akcje**.  

    ![Azure IR SSIS — szczegóły](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Jeśli było **błąd** skojarzony z Azure SSIS IR, zobacz liczbę błędów na tej stronie, a łącze, aby wyświetlić szczegółowe informacje o błędzie. Na przykład jeśli katalog SSIS już istnieje na serwerze bazy danych, zostanie wyświetlony błąd wskazuje na istnienie bazę danych usług SSIS.  
11. Kliknij przycisk **środowisk uruchomieniowych integracji** u góry, aby przejść do poprzedniej strony, aby wyświetlić wszystkie integracji środowisk uruchomieniowych skojarzona z fabryką danych kopii.  

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure środowisk uruchomieniowych integracji usług SSIS w portalu

1. W Interfejsie użytkownika fabryki danych Azure, przełącz się do **Edytuj** , kliknij pozycję **połączeń**, a następnie przejdź do **środowisk uruchomieniowych integracji** kartę, aby wyświetlić istniejące integracji środowisk uruchomieniowych w sieci Fabryka danych. 
    ![Amerykańskim istniejącego widoku](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Kliknij przycisk **nowy** do utworzenia nowego IR. Azure SSIS 

    ![Środowisko uruchomieniowe integracji za pomocą menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Aby utworzyć środowiska uruchomieniowego integracji usług SSIS Azure, kliknij przycisk **nowy** jak pokazano w obrazie. 
3. W oknie Ustawienia środowiska uruchomieniowego integracji wybierz **przyrostu shift istniejących pakietów SSIS do wykonania na platformie Azure**, a następnie kliknij przycisk **dalej**.

    ![Określ typ środowiska uruchomieniowego integracji](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Zobacz [udostępnienia środowiska uruchomieniowego integracji Azure SSIS](#provision-an-azure-ssis-integration-runtime) sekcji pozostałe kroki, aby skonfigurować podczerwieni Azure SSIS.

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="create-variables"></a>Tworzenie zmiennych
Zdefiniuj zmienne do wykorzystania w skrypcie w tym samouczku:

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

# Remove these the following two OPTIONAL variables if you are using Azure SQL Database. 
# These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# Get the following information from the properties page for your Classic Virtual Network in the Azure portal
# It should be in the format: $VnetId = "/subscriptions/<Azure Subscription ID>/resourceGroups/<Azure Resource Group>/providers/Microsoft.ClassicNetwork/virtualNetworks/<Class Virtual Network Name>"

# OPTIONAL: In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

```
### <a name="log-in-and-select-subscription"></a>Logowanie i wybieranie subskrypcji
Dodaj następujący kod, skrypt logowania i wybierz subskrypcję platformy Azure: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Weryfikowanie połączenia z bazą danych
Dodaj następujący skrypt w celu zweryfikowania Twojej server.database.windows.net serwera bazy danych SQL Azure lub punkt końcowy serwera (podglądzie prywatnym) wystąpienia zarządzane SQL Azure. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

### <a name="configure-virtual-network"></a>Konfigurowanie sieci wirtualnej
Dodaj następujący skrypt, aby automatycznie skonfigurować uprawnienia/ustawienia sieci wirtualnej do przyłączenia do Twojego środowiska Azure SSIS Integration Runtime.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy. Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup` w lokalizacji `westeurope`.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Tworzenie fabryki danych
Uruchom następujące polecenie, aby utworzyć fabrykę danych:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Tworzenie środowiska Integration Runtime
Uruchom następujące polecenie, aby utworzyć środowisko uruchomieniowe integracji usług SSIS Azure, które pakiety usług SSIS działa na platformie Azure: Użyj skryptu z sekcji, na podstawie typu bazy danych (vs bazy danych SQL Azure. Azure SQL wystąpienia zarządzane (podglądzie prywatnym)) jest używany. 

#### <a name="azure-sql-database-to-host-the-ssisdb-database-ssis-catalog"></a>Azure SQL Database do hostowania bazy danych usług SSIS (SSIS katalogu) 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode
```

Nie trzeba przekazać wartości VNetId i podsieć, jeśli nie potrzebujesz dostępu do danych lokalnych, oznacza to, że masz lokalnych źródeł/miejsca docelowe danych w pakietów SSIS. Należy podać wartość parametru CatalogPricingTier. Aby uzyskać listę obsługiwanych warstw cenowych bazy danych SQL Azure, zobacz [limity zasobów bazy danych SQL](../sql-database/sql-database-resource-limits.md).

#### <a name="azure-sql-managed-instance-private-preview-to-host-the-ssisdb-database"></a>Azure zarządzane wystąpienia SQL (podglądzie prywatnym) do obsługi bazy danych usług SSIS

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

Należy przekazać wartości parametrów VnetId i podsieci z Azure zarządzane wystąpienia SQL (podglądzie prywatnym), które łączy sieć wirtualną. Parametr CatalogPricingTier nie ma zastosowania dla wystąpienia zarządzane SQL Azure. 

### <a name="start-integration-runtime"></a>Uruchamianie środowiska Integration Runtime
Uruchom następujące polecenie, aby uruchomić środowisko Azure-SSIS Integration Runtime: 

```powershell
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Wykonanie tego polecenia trwa od **20 do 30 minut**. 


### <a name="full-script"></a>Pełny skrypt
W tym miejscu jest pełna skrypt, który tworzy IR Azure SSIS i dołącza go do sieci wirtualnej. Ten skrypt odnosi się wystąpienia zarządzane SQL Azure (MI) są używane do obsługi katalogu usług SSIS. 

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two OPTIONAL variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName

write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Szablon usługi Azure Resource Manager
Szablon usługi Azure Resource Manager umożliwia utworzenie środowiska uruchomieniowego integracji usług SSIS Azure. Poniżej przedstawiono wskazówki próbki: 

1. Utwórz plik JSON zawierający następujący szablon Menedżera zasobów. Zastąp wartości w nawiasach (symbole zastępcze) własne wartości. 

    ```json
    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2017-09-01-preview",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for the Azure SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D1_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 1
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL server>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL user",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```
2. Aby wdrożyć szablon usługi Resource Manager, uruchom polecenie New-AzureRmResourceGroupDeployment, jak pokazano w następujących exmaple. W tym przykładzie ADFTutorialResourceGroup jest nazwą grupy zasobów. ADFTutorialARM.json jest plikiem, który zawiera definicję JSON dla fabryki danych i podczerwieni Azure SSIS. 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    To polecenie tworzy fabryki danych i tworzy IR Azure SSIS, ale nie uruchamia IR. 
3. Aby uruchomić IR Azure SSIS, uruchom polecenie Start-AzureRmDataFactoryV2IntegrationRuntime: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name> `
                                             -DataFactoryName <Data Factory Name> `
                                             -Name <Azure SSIS IR Name> `
                                             -Force
    ``` 

## <a name="deploy-ssis-packages"></a>Wdrażanie pakietów usług SSIS
Teraz użyj programu SQL Server Data Tools (SSDT) lub SQL Server Management Studio (SSMS), aby wdrożyć pakiety usług SSIS na platformie Azure. Połącz się ze swoim serwerem Azure SQL, który hostuje katalog usług SSIS (SSISDB). Nazwa serwera Azure SQL jest w formacie: &lt;nazwa_serwera&gt;.database.windows.net (dla usługi Azure SQL Database). Zobacz artykuł [Deploy packages (Wdrażanie pakietów)](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server), aby uzyskać instrukcje. 

## <a name="next-steps"></a>Następne kroki
Inne IR Azure SSIS w tematach w tej dokumentacji:

- [Środowisko uruchomieniowe integracji usług SSIS Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime). Ten artykuł zawiera informacje o pojęciach dotyczących środowisk uruchomieniowych integracji ogólnie tym podczerwieni Azure SSIS. 
- [Samouczek: Wdrażanie pakietów usług SSIS na platformie Azure](tutorial-deploy-ssis-packages-azure.md). Ten artykuł zawiera szczegółowe instrukcje dotyczące tworzenia środowiska Azure-SSIS IR i używania bazy danych Azure SQL do hostowania wykazu usług SSIS. 
- [Monitor an Azure-SSIS IR (Monitorowanie środowiska Azure-SSIS IR)](monitor-integration-runtime.md#azure-ssis-integration-runtime). W tym artykule przedstawiono sposób pobierania informacji o środowisku Azure-SSIS IR i opisów stanów w pobranych informacjach. 
- [Manage an Azure-SSIS IR (Zarządzanie środowiskiem Azure-SSIS IR)](manage-azure-ssis-integration-runtime.md). W tym artykule przedstawiono sposób zatrzymywania, uruchamiania lub usuwania środowiska Azure-SSIS IR. Zawiera on również instrukcje skalowania środowiska Azure-SSIS IR do wewnątrz za pomocą dodawania do niego węzłów. 
- [Join an Azure-SSIS IR to a VNet (Dołączanie środowiska Azure-SSIS IR do sieci wirtualnej)](join-azure-ssis-integration-runtime-virtual-network.md). Ten artykuł zawiera podstawowe informacje na temat dołączania środowiska Azure-SSIS IR do sieci wirtualnej platformy Azure. Opisano w nim kroki konfigurowania sieci wirtualnej za pomocą witryny Azure Portal tak, aby umożliwić dołączanie środowiska Azure-SSIS IR do sieci wirtualnej. 