---
title: "Przy użyciu bazy danych SQL Azure stosu | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można wdrożyć baz danych jako usługa na stosie Azure i Szybkie kroki wdrażania karty dostawcy zasobów programu SQL Server."
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: JeffGo
ms.openlocfilehash: bf52ed4986b4e0930b57721c0e38bbf748045a36
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Użyj bazy danych SQL Microsoft Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Użyj karty dostawcy zasobów programu SQL Server do udostępnienia baz danych jako usługa [stosu Azure](azure-stack-poc.md). Po zainstalowaniu dostawcy zasobów i połącz go z co najmniej jedno wystąpienie programu SQL Server, można utworzyć i użytkowników:
- Bazy danych dla chmury natywnych aplikacji.
- Witryny sieci Web, które są oparte na SQL.
- Obciążenia, które są oparte na SQL.
Nie trzeba udostępnić maszynę wirtualną (VM) obsługującego program SQL Server zawsze.

Dostawca zasobów nie obsługuje wszystkie funkcje zarządzania bazy danych z [bazy danych SQL Azure](https://azure.microsoft.com/services/sql-database/). Na przykład elastyczne pule baz danych i możliwości wybierania wydajność bazy danych w górę i w dół automatycznie nie są dostępne. Jednak podobnie Obsługa ma dostawcy zasobów tworzenia, odczytu, aktualizacji i usuwania operacji (CRUD). Interfejs API nie jest zgodny z bazy danych SQL.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektura karty dostawcy zasobów SQL
Dostawca zasobów składa się z trzech składników:

- **Karta Dostawca zasobów SQL maszyny Wirtualnej**, która jest maszyny wirtualnej systemu Windows, która uruchamia dostawcy usług.
- **Dostawca zasobów sam**, który przetwarza żądania alokacji i ujawnia bazy danych zasobów.
- **Serwery obsługujące program SQL Server**, które zapewniają pojemność baz danych o nazwie serwerami hostingu.

Należy utworzyć intances jednej (lub więcej) programu SQL Server i zapewniają dostęp do zewnętrznych wystąpień programu SQL Server.

## <a name="deploy-the-resource-provider"></a>Wdrażanie dostawcy zasobów

1. Jeśli jeszcze nie zarejestrować Twojego zestawu programowanie i pobranie obrazu systemu Windows Server 2016 Datacenter Core do pobrania za pośrednictwem zarządzania Marketplace. Należy użyć obrazu systemu Windows Server 2016 Core. Umożliwia także skrypt do utworzenia [obrazu systemu Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Pamiętaj wybrać opcję core). Środowisko uruchomieniowe .NET 3.5 nie jest już wymagane.

2. Zaloguj się do hosta, który można uzyskać dostępu do uprzywilejowanych punktu końcowego maszyny Wirtualnej.

    - W instalacjach Azure stosu Development Kit Zaloguj się do hosta fizycznego.

    - W systemach z wieloma węzłami host musi być systemu, w którym można uzyskać dostępu do uprzywilejowanych punktu końcowego.
    
    >[!NOTE]
    > System, w którym skrypt jest uruchamiany *musi* jako system Windows 10 lub Windows Server 2016 z najnowszą wersją środowiska uruchomieniowego .NET zainstalowane. W przeciwnym razie niepowodzenia instalacji. Host zestawu SDK platformy Azure stosu spełnia tego kryterium.


3. Pobierz dostawcę zasobów SQL binarnego. Następnie uruchom samorozpakowujący się plik typu wyodrębnienie zawartości do katalogu tymczasowego.

    >[!NOTE] 
    > Kompilacja dostawcy zasobów odnosi się do stosu Azure kompilacji. Pamiętaj pobrać poprawne dane binarne dla wersji stosu Azure, w którym jest uruchomiony.

    | Azure kompilacji stosu | Instalator dostawcy zasobów SQL |
    | --- | --- |
    |1.0.180102.3, 1.0.180103.2 lub 1.0.180106.1 (wielowęzłowego) | [RP SQL w wersji 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1.0.171122.1 | [RP SQL w wersji 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1.0.171028.1 | [RP SQL w wersji 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Certyfikat główny stos Azure są pobierane z punktu końcowego uprzywilejowanych. Dla tego zestawu SDK stosu Azure certyfikatu z podpisem własnym jest tworzony w ramach tego procesu. Wieloma węzłami trzeba podać odpowiedni certyfikat.

   Aby dostarczyć własny certyfikat, należy umieścić plik pfx w **DependencyFilesLocalPath** w następujący sposób:

    - Certyfikat uniwersalny dla \*.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\> lub certyfikat w jednej lokacji z nazwą pospolitą sqladapter.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\>.

    - Ten certyfikat musi być zaufany. Oznacza to łańcuch zaufania musi istnieć bez konieczności używania certyfikatów pośrednich.

    - Istnieje tylko jeden certyfikat plik w DependencyFilesLocalPath.

    - Nazwa pliku nie może zawierać żadnych znaków specjalnych.


5. Otwórz **nowe** z podwyższonym poziomem uprawnień (administracyjnych) konsoli programu PowerShell i zmian w katalogu, w którym zostały wyodrębnione pliki. Użyj nowego okna, aby uniknąć problemów, które mogą wystąpić z niepoprawne moduły programu PowerShell, które zostały już załadowane w systemie.

6. [Zainstaluj program Azure PowerShell w wersji 1.2.11](azure-stack-powershell-install.md).

7. Uruchom skrypt DeploySqlProvider.ps1, która wykonuje następujące czynności:

    - Przekazuje certyfikaty i inne artefaktów na konto magazynu Azure stosu.
    - Publikowanie pakietów galerii tak, aby umożliwić wdrożenie bazy danych SQL za pomocą galerii.
    - Publikuje pakietu galerii wdrażania serwerami hostingu.
    - Wdraża maszynę Wirtualną za pomocą obrazu systemu Windows Server 2016, który został utworzony w kroku 1, a następnie instaluje dostawcę zasobów.
    - Rejestruje rekord DNS lokalnego mapy do dostawcy zasobów maszyny Wirtualnej.
    - Rejestruje dostawcę zasobów z lokalnej usługi Azure Resource Manager (administratora i użytkowników).

> [!NOTE]
> Jeśli instalacja wymaga więcej niż 90 minut, może się nie udać. Jeśli nie powiedzie się, zobaczysz komunikat o błędzie na ekranie i w pliku dziennika, ale próba zostanie ponowiona z kroku niepowodzenie wdrożenia. Systemy, które nie spełnia zalecanych wymagań pamięci i vCPU nie można wdrożyć dostawca identyczny SQL.
>

Oto przykład można uruchamiać w wierszu polecenia programu PowerShell. (Pamiętaj zmienić informacje o koncie i hasła zgodnie z potrzebami.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parametry DeploySqlProvider.ps1
Te parametry można określić w wierszu polecenia. Jeśli nie chcesz, lub jeśli wszystkich parametrów sprawdzania poprawności zakończy się niepowodzeniem, zostanie wyświetlony monit o zapewnić wymagane parametry.

| Nazwa parametru | Opis | Wartość domyślna lub komentarz |
| --- | --- | --- |
| **CloudAdminCredential** | Poświadczenia dla administratora chmury niezbędne do uzyskiwania dostępu do uprzywilejowanych punktu końcowego. | _Wymagane_ |
| **AzCredential** | Poświadczenia dla konta administratora usługi Azure stosu. Użyj tych samych poświadczeń, używane do wdrażania usługi Azure stosu. | _Wymagane_ |
| **VMLocalCredential** | Poświadczenia dla konta administratora lokalnego dostawcy zasobów SQL maszyny Wirtualnej. | _Wymagane_ |
| **PrivilegedEndpoint** | Adres IP lub nazwa DNS uprzywilejowanych punktu końcowego. |  _Wymagane_ |
| **DependencyFilesLocalPath** | Plik .pfx certyfikatu muszą znajdować się w tym również katalogu. | _Opcjonalne_ (_obowiązkowe_ dla wielowęzłowego) |
| **DefaultSSLCertificatePassword** | Hasło dla certyfikatu pfx. | _Wymagane_ |
| **MaxRetryCount** | Ile razy chcesz ponowić próbę każdej operacji w przypadku awarii.| 2 |
| **RetryDuration** | Interwał limitu czasu między kolejnymi próbami w sekundach. | 120 |
| **Dezinstalacja** | Usuwa dostawcy zasobów i wszystkie powiązane zasoby (zobacz uwagi następujące). | Nie |
| **DebugMode** | Uniemożliwia automatyczne czyszczenie w przypadku awarii. | Nie |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Weryfikacja wdrożenia przy użyciu portalu Azure stosu

> [!NOTE]
>  Po zakończeniu skrypt instalacji należy odświeżyć portalu, zobacz blok administratora.


1. Zaloguj się do portalu administratora jako administratora usługi.

2. Sprawdź, czy wdrożenie zakończyło się pomyślnie. Przejdź do **grup zasobów**. Następnie wybierz **systemu.\< Lokalizacja\>.sqladapter** grupy zasobów. Sprawdź, czy wszystkie cztery wdrożenia zakończyło się pomyślnie.

      ![Sprawdź wdrożenie dostawcy zasobów SQL](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Zaktualizuj kartę Dostawca zasobów SQL (wielowęzłowego tylko kompilacje 1710 i nowsze)
Przy każdej aktualizacji kompilacji stosu Azure wydaniu nowej karty dostawcy zasobów SQL. Istniejące karty mogą nadal działać. Firma Microsoft zaleca jednak aktualizacja do nowszej kompilacji jak najszybciej po zaktualizowaniu stosu Azure. 

Proces aktualizacji jest podobny do procesu instalacji, które jest opisane wcześniej. Utworzeniu nowej maszyny Wirtualnej za pomocą najnowszej kod dostawcy zasobów. Ponadto migracji ustawień dla tego nowego wystąpienia, w tym bazy danych i informacji o serwerze hosting. Można również migrację niezbędnych rekordów DNS.

Za pomocą skryptu UpdateSQLProvider.ps1 te same argumenty, które firma Microsoft opisane wcześniej. Należy również podać certyfikatu w tym miejscu.

> [!NOTE]
> Ten proces aktualizacji jest obsługiwana tylko na komputerach z wieloma węzłami.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parametry UpdateSQLProvider.ps1
Te parametry można określić w wierszu polecenia. Jeśli nie chcesz, lub jeśli wszystkich parametrów sprawdzania poprawności zakończy się niepowodzeniem, zostanie wyświetlony monit o zapewnić wymagane parametry.

| Nazwa parametru | Opis | Wartość domyślna lub komentarz |
| --- | --- | --- |
| **CloudAdminCredential** | Poświadczenia dla administratora chmury niezbędne do uzyskiwania dostępu do uprzywilejowanych punktu końcowego. | _Wymagane_ |
| **AzCredential** | Poświadczenia dla konta administratora usługi Azure stosu. Użyj tych samych poświadczeń, które używane do wdrożenia usługi Azure stosu. | _Wymagane_ |
| **VMLocalCredential** | Poświadczenia dla konta administratora lokalnego dostawcy zasobów SQL maszyny Wirtualnej. | _Wymagane_ |
| **PrivilegedEndpoint** | Adres IP lub nazwa DNS uprzywilejowanych punktu końcowego. |  _Wymagane_ |
| **DependencyFilesLocalPath** | Plik .pfx certyfikatu muszą znajdować się w tym również katalogu. | _Opcjonalne_ (_obowiązkowe_ dla wielowęzłowego) |
| **DefaultSSLCertificatePassword** | Hasło dla certyfikatu pfx. | _Wymagane_ |
| **MaxRetryCount** | Ile razy chcesz ponowić próbę każdej operacji w przypadku awarii.| 2 |
| **RetryDuration** |Interwał limitu czasu między kolejnymi próbami w sekundach. | 120 |
| **Dezinstalacja** | Usuwa dostawcy zasobów i wszystkie powiązane zasoby (zobacz uwagi następujące). | Nie |
| **DebugMode** | Uniemożliwia automatyczne czyszczenie w przypadku awarii. | Nie |



## <a name="remove-the-sql-resource-provider-adapter"></a>Usuń kartę Dostawca zasobów SQL

Aby usunąć dostawcę zasobów, konieczne jest najpierw usunąć wszelkie zależności.

1. Upewnij się, że masz oryginalny pakiet wdrożeniowy, który został pobrany dla tej wersji karty dostawcy zasobów SQL.

2. Należy usunąć wszystkie bazy danych użytkownika od dostawcy zasobów. (Usunięcie bazy danych użytkownika nie powoduje usunięcia danych.) To zadanie powinno być wykonywane przez użytkowników, samodzielnie.

3. Administrator musi usunąć serwerami hostingu z karty dostawcy zasobów SQL.

4. Administrator, należy usunąć wszystkie plany odwołujące się do karty dostawcy zasobów SQL.

5. Administrator musi usunąć wszystkie jednostki SKU i przydziałów, które są skojarzone z kartą dostawcy zasobów programu SQL.

6. Uruchom ponownie skrypt wdrożenia wraz z następującymi elementami:
    - Odinstalować parametru
    - Punkty końcowe usługi Azure Resource Manager
    - DirectoryTenantID
    - Poświadczenia dla konta administratora usługi


## <a name="next-steps"></a>Kolejne kroki

[Dodaj serwery hostingu](azure-stack-sql-resource-provider-hosting-servers.md) i [Tworzenie baz danych](azure-stack-sql-resource-provider-databases.md).

Spróbuj innych [usług PaaS](azure-stack-tools-paas-services.md) jak [dostawcy zasobów serwer MySQL](azure-stack-mysql-resource-provider-deploy.md) i [dostawcy zasobów usługi aplikacji](azure-stack-app-service-overview.md).
