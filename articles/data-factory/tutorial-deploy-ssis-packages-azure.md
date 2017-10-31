---
title: "Wdrażanie pakietów usług SSIS na platformie Azure | Microsoft Docs"
description: "W tym artykule opisano sposób wdrażania pakietów usług SSIS w środowisku Azure SSIS Integration Runtime udostępnianym przez usługę Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 09/06/2017
ms.author: spelluru
ms.openlocfilehash: c7ae13cd07d9f85376b664a0d51564f90c35f97e
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>Wdrażanie pakietów usług SQL Server Integration Services na platformie Azure
Azure Data Factory to oparta na chmurze usługa integracji danych, za pomocą której możesz tworzyć oparte na danych przepływy pracy w chmurze służące do organizowania oraz automatyzowania przenoszenia i przekształcania danych. Za pomocą usługi Azure Data Factory można tworzyć oparte na danych przepływy pracy (nazywane potokami) i ustalać ich harmonogram. Te przepływy mogą pozyskiwać dane z różnych magazynów danych, przetwarzać/przekształcać je za pomocą usług obliczeniowych, takich jak Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics i Azure Machine Learning, a następnie publikować dane wyjściowe w magazynach danych, np. Azure SQL Data Warehouse, do użycia przez aplikacje analizy biznesowej. 

Ten samouczek zawiera instrukcje aprowizacji środowiska Azure SSIS Integration Runtime (IR) w usłudze Azure Data Factory. Następnie możesz użyć programu SQL Server Data Tools (SSDT) lub SQL Server Management Studio (SSMS) do wdrożenia pakietów usług SQL Server Integration Services (SSIS) w tym środowisku uruchomieniowym na platformie Azure. W tym samouczku wykonasz następujące czynności:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie środowiska Azure SSIS Integration Runtime
> * Uruchamianie środowiska Azure SSIS Integration Runtime
> * Wdrażanie pakietów usług SSIS
> * Przeglądanie kompletnego skryptu

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto. Aby uzyskać koncepcyjne informacje dotyczące środowiska Azure SSIS IR, zobacz [Omówienie środowiska Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime).

## <a name="prerequisites"></a>Wymagania wstępne
- **Serwer usługi Azure SQL Database**. Jeśli nie masz jeszcze serwera bazy danych, utwórz go w witrynie Azure Portal, zanim zaczniesz. Ten serwer hostuje bazę danych katalogu usług SSIS (SSISDB). Zaleca się utworzenie serwera bazy danych w tym samym regionie platformy Azure, co środowisko Integration Runtime. Ta konfiguracja pozwala środowisku Integration Runtime zapisywać dzienniki wykonywania SSISDB bez wykraczania poza granice regionów świadczenia usług platformy Azure. 
    - Upewnij się, że ustawienie „**Zezwalaj na dostęp do usług platformy Azure**” jest **WŁĄCZONE** dla serwera bazy danych. Aby uzyskać więcej informacji, zobacz artykuł [Secure your Azure SQL database (Zabezpieczenia bazy danych Azure SQL)](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Aby włączyć to ustawienie za pomocą programu PowerShell, zobacz polecenie [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
    - Do listy adresów IP klienta w ustawieniach zapory dla serwera bazy danych dodaj adres IP maszyny klienta lub zakres adresów IP, który zawiera adres IP maszyny klienta. Aby uzyskać więcej informacji, zobacz [Azure SQL Database server-level and database-level firewall rules (Reguły zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database)](../sql-database/sql-database-firewall-configure.md). 
- Zainstalowanie programu **Azure PowerShell**. Wykonaj instrukcje podane w temacie [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps). Program PowerShell służy do uruchamiania skryptu w celu aprowizacji środowiska Azure SSIS Integration Runtime, które uruchamia pakiety SSIS w chmurze. 

## <a name="launch-windows-powershell-ise"></a>Uruchamianie programu Windows PowerShell ISE
Uruchom program **Windows PowerShell ISE** przy użyciu uprawnień administracyjnych. 

## <a name="create-variables"></a>Tworzenie zmiennych
Skopiuj i wklej poniższy skrypt. Określ wartości zmiennych. Aby zapoznać się z listą obsługiwanych **warstw cenowych** dla usługi Azure SQL Database, zobacz [SQL Database resource limits (Limity zasobów usługi SQL Database)](../sql-database/sql-database-resource-limits.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 
```

## <a name="validate-the-connection-to-database"></a>Weryfikowanie połączenia z bazą danych
Dodaj poniższy skrypt w celu zweryfikowania serwera usługi Azure SQL Database server.database.windows.net. 

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

Aby utworzyć bazę danych Azure SQL w ramach skryptu, zobacz poniższy przykład: 

Ustaw wartości dla zmiennych, które nie zostały jeszcze zdefiniowane. Na przykład: FirewallIPAddress. 

```powershell
New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName `
  -ServerName $SQLServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SQLServerAdmin, $(ConvertTo-SecureString -String $SQLServerPass -AsPlainText -Force))

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SQLServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SQLServerName -AllowAllAzureIPs
```


## <a name="log-in-and-select-subscription"></a>Logowanie i wybieranie subskrypcji
Dodaj następujący kod do skryptu logowania i wybierz subskrypcję platformy Azure: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy. Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup` w lokalizacji `westeurope`.

Jeśli grupa zasobów już istnieje, nie kopiuj tego kodu do skryptu. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych
Uruchom następujące polecenie, aby utworzyć fabrykę danych:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                        -Location $DataFactoryLocation `
                        -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Tworzenie środowiska Integration Runtime
Uruchom następujące polecenie, aby utworzyć środowisko Azure-SSIS Integration Runtime, które uruchamia pakiety usług SSIS na platformie Azure: 

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

## <a name="start-integration-runtime"></a>Uruchamianie środowiska Integration Runtime
Uruchom następujące polecenie, aby uruchomić środowisko Azure-SSIS Integration Runtime: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Wykonanie tego polecenia trwa od **20 do 30 minut**. 

## <a name="deploy-ssis-packages"></a>Wdrażanie pakietów usług SSIS
Teraz użyj programu SQL Server Data Tools (SSDT) lub SQL Server Management Studio (SSMS), aby wdrożyć pakiety usług SSIS na platformie Azure. Połącz się ze swoim serwerem Azure SQL, który hostuje katalog usług SSIS (SSISDB). Nazwa serwera Azure SQL jest w formacie: `<servername>.database.windows.net`(dla usługi Azure SQL Database). 

Zobacz następujące artykuły wchodzące w skład usług SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure (Wdrażanie, uruchamianie i monitorowanie pakietu usług SSIS na platformie Azure)](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Connect to SSIS catalog on Azure (Łączenie z wykazem usług SSIS na platformie Azure)](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule package execution on Azure (Planowanie wykonywania pakietów na platformie Azure)](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows authentication (Łączenie z lokalnymi źródłami danych przy użyciu uwierzytelniania systemu Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="full-script"></a>Pełny skrypt
W tym wydaniu musisz użyć programu PowerShell do aprowizacji wystąpienia środowiska Azure SSIS Integration Runtime, które uruchamia pakiety usług SSIS w chmurze. Obecnie nie jest możliwa aprowizacja tego środowiska uruchomieniowego przy użyciu witryny Azure Portal. 

Skrypt programu PowerShell w tej sekcji konfiguruje wystąpienie środowiska Azure SSIS Integration Runtime w chmurze, w której działają pakiety usług SSIS. Po pomyślnym uruchomieniu tego skryptu możesz wdrażać i uruchamiać pakiety usług SSIS w chmurze Microsoft Azure z bazą danych SSISDB hostowaną w usłudze Azure SQL Database.

1. Uruchom środowisko Windows PowerShell Integrated Scripting Environment (ISE).
2. W środowisku ISE uruchom następujące polecenie w wierszu polecenia.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Skopiuj skrypt programu PowerShell w tej sekcji i wklej go do środowiska ISE.
4. Podaj odpowiednie wartości parametrów skryptu w sekcji „SSIS in Azure specifications” (Usługi SSIS w specyfikacjach platformy Azure) na początku skryptu. Te parametry są opisane w następnej sekcji.
5. Uruchom skrypt. Wykonanie polecenia `Start-AzureRmDataFactoryV2IntegrationRuntime` w pobliżu końca skryptu zajmuje od **20 do 30 minut**.

> [!NOTE]
> Skrypt łączy się z usługą Azure SQL Database w celu przygotowania bazy danych wykazu usług SSIS (SSISDB). Skrypt konfiguruje również uprawnienia i ustawienia Twojej sieci wirtualnej, jeśli zostanie określona, i dołącza nowe wystąpienie środowiska Azure SSIS Integration Runtime do sieci wirtualnej.

Aby zapoznać się z listą obsługiwanych **warstw cenowych** dla usługi Azure SQL Database, zobacz [SQL Database resource limits (Limity zasobów usługi SQL Database)](../sql-database/sql-database-resource-limits.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS (IR)>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 

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

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

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
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-vnet"></a>Dołączanie środowiska Azure SSIS IR do sieci wirtualnej
Jeśli używasz wystąpienia zarządzanego Azure SQL (prywatna wersja zapoznawcza) do hostowania wykazu usług SQL Server Integration Services (SSIS) wewnątrz sieci wirtualnej, musisz również dołączyć środowisko Azure-SSIS Integration Runtime do tej samej sieci wirtualnej. Usługa Azure Data Factory w wersji 2 (wersja zapoznawcza) umożliwia dołączenie środowiska Azure-SSIS Integration Runtime do klasycznej sieci wirtualnej. Aby uzyskać więcej informacji, zobacz [Join Azure-SSIS runtime to a VNet (Dołączanie środowiska uruchomieniowego usług Azure-SSIS do sieci wirtualnej)](join-azure-ssis-integration-runtime-virtual-network.md).

Pełen skrypt tworzenia środowiska usług Azure-SSIS można znaleźć w części [Create an Azure-SSIS integration runtime (Tworzenie środowiska Azure-SSIS Integration Runtime)](create-azure-ssis-integration-runtime.md).

## <a name="monitor-and-manage-azure-ssis-ir"></a>Monitorowanie środowiska Azure-SSIS IR
W poniższych artykułach można znaleźć szczegółowe informacje o monitorowaniu środowiska Azure-SSIS IR i zarządzania nim. 

- [Monitor an Azure-SSIS integration runtime (Monitorowanie środowiska Azure-SSIS Integration Runtime)](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Manage an Azure-SSIS integration runtime (Zarządzanie środowiskiem Azure-SSIS Integration Runtime)](manage-azure-ssis-integration-runtime.md)

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności: 

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie środowiska Azure SSIS Integration Runtime
> * Uruchamianie środowiska Azure SSIS Integration Runtime
> * Wdrażanie pakietów usług SSIS
> * Przeglądanie kompletnego skryptu

Przejdź do następującego samouczka, aby dowiedzieć się więcej o kopiowaniu danych lokalnych do chmury: 

> [!div class="nextstepaction"]
>[Kopiowanie danych w chmurze](tutorial-copy-data-dot-net.md)
