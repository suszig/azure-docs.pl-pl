---
title: "Przy użyciu bazy danych SQL Azure stosu | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można wdrożyć baz danych jako usługa na stosie Azure i Szybkie kroki wdrażania karty dostawcy zasobów programu SQL Server"
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
ms.date: 12/15/2017
ms.author: JeffGo
ms.openlocfilehash: 80b693420768d574b2371211298562ba35e7ed97
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Użyj bazy danych SQL Microsoft Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Użyj karty dostawcy zasobów programu SQL Server do udostępnienia baz danych jako usługa [stosu Azure](azure-stack-poc.md). Po zainstalowaniu dostawcy zasobów i połącz go z co najmniej jedno wystąpienie programu SQL Server, można utworzyć i użytkowników:
- bazy danych dla chmury natywne aplikacje
- witryny sieci Web, które są oparte na SQL
- obciążenia, które są oparte na SQL można nie trzeba udostępnić maszynę wirtualną (VM) obsługującego program SQL Server zawsze.

Dostawca zasobów nie obsługuje wszystkie funkcje zarządzania bazy danych z [bazy danych SQL Azure](https://azure.microsoft.com/services/sql-database/). Na przykład elastyczne pule baz danych oraz możliwość automatycznego wybierania numeru wydajność bazy danych w górę i w dół nie są dostępne. Jednak podobnie Obsługa ma dostawcy zasobów tworzenia, odczytu, aktualizacji i usuwania operacji (CRUD). Interfejs API nie jest zgodny z bazy danych SQL.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektura karty dostawcy zasobów SQL
Dostawca zasobów składa się z trzech składników:

- **Karta Dostawca zasobów SQL maszyny Wirtualnej**, który jest maszyną wirtualną systemu Windows z usługami dostawcy.
- **Dostawca zasobów sam**, który przetwarza żądania alokacji i ujawnia bazy danych zasobów.
- **Serwery obsługujące program SQL Server**, które zapewniają pojemność baz danych, nazywane Hosting serwerów.

Należy utworzyć jeden (lub więcej) serwerów SQL lub zapewniają dostęp do zewnętrznych wystąpień programu SQL.

## <a name="deploy-the-resource-provider"></a>Wdrażanie dostawcy zasobów

1. Jeśli jeszcze nie zarejestrować Twojego zestawu programowanie i pobranie obrazu systemu Windows Server 2016 Datacenter Core do pobrania za pośrednictwem zarządzania Marketplace. Należy użyć obrazu systemu Windows Server 2016 Core. Umożliwia także skrypt do utworzenia [obrazu systemu Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) — należy wybrać opcję core. Środowisko uruchomieniowe .NET 3.5 nie jest już wymagane.

2. Zaloguj się do hosta, który można uzyskać dostępu do uprzywilejowanych punktu końcowego maszyny Wirtualnej.

    a. W instalacjach Azure stosu Development Kit (ASDK) Zaloguj się do hosta fizycznego.

    b. W systemach z wieloma węzłami host musi być systemu, w którym można uzyskać dostępu do uprzywilejowanych punktu końcowego. 
    
    >[!NOTE]
    > System, w którym skrypt jest uruchamiany *musi* jako system Windows 10 lub Windows Server 2016 z najnowszą wersją środowiska uruchomieniowego .NET zainstalowane. Instalacja zakończy się niepowodzeniem, w przeciwnym razie wartość. ASDK host spełnia te kryteria.


3. Pobierz dostawcę zasobów SQL binarne i wykonaj samorozpakowujący się plik typu wyodrębnienie zawartości do katalogu tymczasowego.

    >[!NOTE] 
    > Kompilacja dostawcy zasobów odnosi się do stosu Azure kompilacji. Należy pobrać poprawne dane binarne dla wersji stosu Azure, w którym jest uruchomiony.

    | Azure stosu kompilacji | Instalator SQL RP |
    | --- | --- |
    | 1.0.171122.1 | [RP SQL w wersji 1.1.12.0](https://aka.ms/azurestacksqlrp) |
    | 1.0.171028.1 | [RP SQL w wersji 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
    | 1.0.170928.3 | [RP SQL w wersji 1.1.3.0](https://aka.ms/azurestacksqlrp1709) |
   

4. Certyfikat główny stos Azure są pobierane z punktu końcowego uprzywilejowanych. ASDK, aby uzyskać certyfikat z podpisem własnym jest tworzony w ramach tego procesu. Wieloma węzłami trzeba podać odpowiedni certyfikat.

    Jeśli musisz dostarczyć własny certyfikat, należy umieścić w pliku PFX **DependencyFilesLocalPath** (patrz poniżej) w następujący sposób:

    - Certyfikat uniwersalny dla \*.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\> lub certyfikat w jednej lokacji z nazwą pospolitą sqladapter.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\>
    - Ten certyfikat musi być zaufany, takie jak czy wystawiony przez urząd certyfikacji. Oznacza to łańcuch zaufania musi istnieć bez konieczności używania certyfikatów pośrednich.
    - Istnieje tylko jeden certyfikat plik w DependencyFilesLocalPath.
    - Nazwa pliku nie może zawierać żadnych znaków specjalnych.


5. Otwórz **nowe** z podwyższonym poziomem uprawnień (administracyjnych) konsoli programu PowerShell i zmian w katalogu, w którym zostały wyodrębnione pliki. Użyj nowego okna, aby uniknąć problemów, które mogą wystąpić podczas niepoprawne już załadowanych modułów programu PowerShell w systemie.

6. [Zainstaluj program Azure PowerShell w wersji 1.2.11](azure-stack-powershell-install.md).

7. Uruchom skrypt DeploySqlProvider.ps1 z parametrami wymienionych poniżej.

Skrypt wykonuje następujące czynności:

- Przekaż certyfikaty i inne artefaktów na konto magazynu na stosie Azure.
- Publikowanie pakietów galerii, dzięki czemu można wdrożyć bazy danych SQL za pomocą galerii.
- Publikowanie pakietu galerii w celu wdrażania serwerów hostingu
- Wdróż Maszynę wirtualną przy użyciu obrazu systemu Windows Server 2016 utworzonego w kroku 1 i zainstaluj dostawcę zasobów.
- Zarejestruj rekord DNS lokalnego mapy do dostawcy zasobów maszyny Wirtualnej.
- Rejestrowanie dostawcy zasobów z lokalnej usługi Azure Resource Manager (administratora i użytkowników).

> [!NOTE]
> Jeśli instalacja wymaga więcej niż 90 minut, może nie powieść zobaczysz komunikat o błędzie na ekranie i w pliku dziennika, a próba zostanie ponowiona z kroku niepowodzenie wdrożenia. Systemy, które nie spełnia zalecanych wymagań pamięci i vCPU nie można wdrożyć SQL RP.
>

Oto przykład można uruchomić z programu PowerShell Monituj (ale zmienić informacje o koncie i hasła w razie potrzeby):

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parametry DeploySqlProvider.ps1
Te parametry można określić w wierszu polecenia. Jeśli nie chcesz lub wszystkich parametrów sprawdzania poprawności zakończy się niepowodzeniem, zostanie wyświetlony monit podaj wymaganych pól.

| Nazwa parametru | Opis | Wartość domyślna lub komentarz |
| --- | --- | --- |
| **CloudAdminCredential** | Poświadczenia dla administratora chmury niezbędne do uzyskiwania dostępu do uprzywilejowanych punktu końcowego. | _Wymagane_ |
| **AzCredential** | Podaj poświadczenia dla konta administratora usługi Azure stosu. Użyj tych samych poświadczeń używane do wdrażania stosu Azure). | _Wymagane_ |
| **VMLocalCredential** | Zdefiniuj poświadczenia dla konta administratora lokalnego dostawcy zasobów SQL maszyny Wirtualnej. | _Wymagane_ |
| **PrivilegedEndpoint** | Podaj adres IP lub nazwę DNS Privleged punktu końcowego. |  _Wymagane_ |
| **DependencyFilesLocalPath** | Plik PFX certyfikatu muszą znajdować się w tym również katalogu. | _opcjonalne_ (_obowiązkowe_ dla wielowęzłowego) |
| **DefaultSSLCertificatePassword** | Hasło dla certyfikatu pfx | _Wymagane_ |
| **Wartość MaxRetryCount** | Zdefiniuj jak często chcesz ponowić próbę każdej operacji w przypadku awarii.| 2 |
| **RetryDuration** | Zdefiniuj limit czasu między kolejnymi próbami w sekundach. | 120 |
| **Dezinstalacja** | Usuń dostawcę zasobów i wszystkie powiązane zasoby (zobacz uwagi poniżej) | Nie |
| **DebugMode** | Uniemożliwia automatyczne oczyszczania po awarii | Nie |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Weryfikacja wdrożenia przy użyciu portalu Azure stosu

> [!NOTE]
>  Po ukończeniu działania skryptu instalacji, należy odświeżyć portalu, zobacz blok administratora.


1. Zaloguj się do portalu administratora jako administratora usługi.

2. Sprawdź, czy wdrożenie zakończyło się pomyślnie. Przeglądaj w poszukiwaniu **grup zasobów** &gt;, kliknij **systemu.\< Lokalizacja\>.sqladapter** zasobów grupy i sprawdź, czy wszystkie cztery wdrożenia zakończyło się pomyślnie.

      ![Sprawdź wdrożenie SQL RP](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Zaktualizuj kartę Dostawca zasobów SQL (wielowęzłowego tylko kompilacje 1710 i nowsze)
Przy każdej aktualizacji kompilacji stosu Azure zostanie wydana nowa karta dostawcy zasobów programu SQL. Gdy istniejącej karty mogą nadal działać, zaleca się jak najszybciej zaktualizować do nowszej kompilacji po zaktualizowaniu stosu Azure. Proces aktualizacji jest bardzo podobny do procesu instalacji opisane powyżej. Nowa maszyna wirtualna zostanie utworzona kodem RP najnowsze i ustawienia zostaną zmigrowane do tego nowego wystąpienia, łącznie z bazy danych i obsługi informacji o serwerze, a także niezbędnych rekordów DNS.

Za pomocą skryptu UpdateSQLProvider.ps1 te same argumenty co powyżej. Należy również podać certyfikatu w tym miejscu.

> [!NOTE]
> Aktualizacja jest obsługiwana tylko na komputerach z wieloma węzłami.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parametry UpdateSQLProvider.ps1
Te parametry można określić w wierszu polecenia. Jeśli nie chcesz lub wszystkich parametrów sprawdzania poprawności zakończy się niepowodzeniem, zostanie wyświetlony monit podaj wymaganych pól.

| Nazwa parametru | Opis | Wartość domyślna lub komentarz |
| --- | --- | --- |
| **CloudAdminCredential** | Poświadczenia dla administratora chmury niezbędne do uzyskiwania dostępu do uprzywilejowanych punktu końcowego. | _Wymagane_ |
| **AzCredential** | Podaj poświadczenia dla konta administratora usługi Azure stosu. Użyj tych samych poświadczeń używane do wdrażania stosu Azure). | _Wymagane_ |
| **VMLocalCredential** | Zdefiniuj poświadczenia dla konta administratora lokalnego dostawcy zasobów SQL maszyny Wirtualnej. | _Wymagane_ |
| **PrivilegedEndpoint** | Podaj adres IP lub nazwę DNS Privleged punktu końcowego. |  _Wymagane_ |
| **DependencyFilesLocalPath** | Plik PFX certyfikatu muszą znajdować się w tym również katalogu. | _opcjonalne_ (_obowiązkowe_ dla wielowęzłowego) |
| **DefaultSSLCertificatePassword** | Hasło dla certyfikatu pfx | _Wymagane_ |
| **Wartość MaxRetryCount** | Zdefiniuj jak często chcesz ponowić próbę każdej operacji w przypadku awarii.| 2 |
| **RetryDuration** | Zdefiniuj limit czasu między kolejnymi próbami w sekundach. | 120 |
| **Dezinstalacja** | Usuń dostawcę zasobów i wszystkie powiązane zasoby (zobacz uwagi poniżej) | Nie |
| **DebugMode** | Uniemożliwia automatyczne oczyszczania po awarii | Nie |



## <a name="remove-the-sql-resource-provider-adapter"></a>Usuń kartę Dostawca zasobów SQL

Aby usunąć dostawcę zasobów, konieczne jest najpierw usunąć wszelkie zależności.

1. Upewnij się, że oryginalne pakiet wdrożeniowy, który został pobrany dla tej wersji karty dostawcy zasobów SQL.

2. Od dostawcy zasobów (nie powoduje usunięcia danych), należy usunąć wszystkie bazy danych użytkownika. To zadanie należy wykonać przez siebie użytkowników.

3. Administrator, należy usunąć serwerami hostingu z karty dostawcy zasobów SQL

4. Administrator, należy usunąć wszystkie plany odwołujące się do karty dostawcy zasobów SQL.

5. Administrator musi usunąć wszystkie jednostki SKU i przydziały skojarzonego z karty dostawcy zasobów SQL.

6. Uruchom ponownie skrypt wdrożenia z odinstalować parametru, punktów końcowych usługi Azure Resource Manager, DirectoryTenantID i poświadczenia dla konta administratora usługi.


## <a name="next-steps"></a>Kolejne kroki

[Dodaj serwery Hosting](azure-stack-sql-resource-provider-hosting-servers.md) i [Tworzenie baz danych](azure-stack-sql-resource-provider-databases.md).

Spróbuj innych [usług PaaS](azure-stack-tools-paas-services.md) jak [dostawcy zasobów serwer MySQL](azure-stack-mysql-resource-provider-deploy.md) i [dostawcy zasobów usługi aplikacji](azure-stack-app-service-overview.md).
