---
title: "Użyj bazy danych MySQL jako PaaS w stosie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć MySQL dostawcy zasobów i zapewnić baz danych MySQL jako usługa na stosie Azure."
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: mabrigg
ms.openlocfilehash: db7daf61fa80854c17b58252d7d6cb30c329dfb1
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Użyj bazy danych MySQL na Microsoft Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Dostawca zasobów MySQL na stosie Azure można wdrożyć. Po wdrożeniu dostawcy zasobów, można utworzyć MySQL serwerów i baz danych za pośrednictwem usługi Azure Resource Manager deployment szablonów. Można też podać baz danych MySQL jako usługa. 

Bazy danych MySQL, które są wspólne w witrynach sieci web, obsługuje wiele platform witryny sieci Web. Na przykład po wdrożeniu dostawcy zasobów można tworzenia witryn sieci Web WordPress z platformy aplikacji sieci Web jako dodatek usługa (PaaS) Azure stosu.

Aby wdrożyć dostawcy MySQL na komputerze, który nie ma dostępu do Internetu, skopiuj plik [mysql — łącznik net-6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) do udziału lokalnego. Następnie podaj nazwę tego udziału po wyświetleniu monitu dla niego. Należy zainstalować moduły Azure oraz Azure PowerShell stosu.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Architektura karty dostawcy zasobów serwer MySQL

Dostawca zasobów składa się z trzech składników:

- **Karta Dostawca zasobów MySQL maszyny Wirtualnej**, która jest maszyny wirtualnej systemu Windows, w którym są uruchomione usługi dostawcy.

- **Dostawca zasobów sam**, który przetwarza żądania alokacji i ujawnia bazy danych zasobów.

- **Serwery obsługujące serwer MySQL**, które zapewniają pojemność baz danych, które są nazywane serwerów hosta.

Ta wersja nie tworzy już wystąpień MySQL. Oznacza to, że trzeba je utworzyć samodzielnie i/lub zapewniają dostęp do zewnętrznych wystąpień programu SQL. Odwiedź stronę [galerii Szybki Start Azure stosu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) przykład szablonu, który można:
- Tworzy serwer MySQL.
- Pobieranie i wdrażanie serwera MySQL z portalu Azure Marketplace.

> [!NOTE]
> Hosting serwerów, które są zainstalowane na implementacji stosu Azure wielowęzłowego musi zostać utworzona z subskrypcji dzierżawcy. Nie można ich utworzyć z subskrypcji dostawcy domyślnego. Muszą one zostać utworzone w portalu dzierżawcy lub w sesji programu PowerShell z odpowiednią logowania. Wszystkie serwery hostingu maszyn wirtualnych mogą być obciążane i musi mieć odpowiednie licencje. Administrator usługi może być właścicielem subskrypcji dzierżawcy.

### <a name="required-privileges"></a>Wymagane uprawnienia
Konto system musi mieć następujące uprawnienia:

1.  Baza danych: Utwórz, Porzuć
2.  Logowania: Tworzenie, ustawić, porzucić, przydzielić, odwołać

## <a name="deploy-the-resource-provider"></a>Wdrażanie dostawcy zasobów

1. Jeśli jeszcze nie zarejestrować Twojego zestawu programowanie i pobranie obrazu systemu Windows Server 2016 Datacenter Core do pobrania za pośrednictwem zarządzania Marketplace. Należy użyć obrazu systemu Windows Server 2016 Core. Umożliwia także skrypt do utworzenia [obrazu systemu Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Pamiętaj wybrać opcję core.) Środowisko uruchomieniowe .NET 3.5 nie jest już wymagane.


2. Zaloguj się do hosta, który można uzyskać dostępu do uprzywilejowanych punktu końcowego maszyny Wirtualnej.

    - W przypadku instalacji zestawu Azure SDK Zaloguj się do hosta fizycznego. 
    - W systemach z wieloma węzłami host musi być systemu, w którym można uzyskać dostępu do uprzywilejowanych punktu końcowego.
    
    >[!NOTE]
    > System, na którym skrypt jest uruchamiany *musi* jako system Windows 10 lub Windows Server 2016 z najnowszą wersją środowiska uruchomieniowego .NET zainstalowane. W przeciwnym razie niepowodzenia instalacji. Host zestawu Azure SDK spełnia te kryteria.
    

3. Pobierz dostawcę zasobów MySQL binarnego. Następnie uruchom samorozpakowujący się plik typu wyodrębnienie zawartości do katalogu tymczasowego.

    >[!NOTE] 
    > Kompilacja dostawcy zasobów odpowiada kompilacje stosu Azure. Pamiętaj pobrać poprawne dane binarne dla wersji stosu Azure, w którym jest uruchomiony.

    | Azure kompilacji stosu | Instalator MySQL RP |
    | --- | --- |
    | 1.0.180102.3 lub 1.0.180106.1 (wielowęzłowego) | [MySQL RP wersji 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |
    | 1.0.171122.1 | [MySQL RP wersji 1.1.12.0](https://aka.ms/azurestackmysqlrp1711) |
    | 1.0.171028.1 | [MySQL RP wersji 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |

4.  Certyfikat główny stos Azure są pobierane z punktu końcowego uprzywilejowanych. Dla zestawu Azure SDK certyfikatu z podpisem własnym jest tworzony w ramach tego procesu. Wieloma węzłami trzeba podać odpowiedni certyfikat.

    Jeśli musisz dostarczyć własny certyfikat, należy umieścić plik pfx w **DependencyFilesLocalPath** który spełnia następujące kryteria:

    - Certyfikat uniwersalny dla \*.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\> lub certyfikat w jednej lokacji z nazwą pospolitą mysqladapter.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\>.

    - Ten certyfikat musi być zaufany. Oznacza to łańcuch zaufania musi istnieć bez konieczności używania certyfikatów pośrednich.

    - Istnieje tylko jeden certyfikat plik w DependencyFilesLocalPath.
    
    - Nazwa pliku nie może zawierać żadnych znaków specjalnych.


5. Otwórz **nowe** z podwyższonym poziomem uprawnień (administracyjnych) konsoli programu PowerShell. Następnie przejdź do katalogu, w którym zostały wyodrębnione pliki. Użyj nowego okna, aby uniknąć problemów, które mogą wystąpić z niepoprawne moduły programu PowerShell, które zostały już załadowane w systemie.

6. [Zainstaluj program Azure PowerShell w wersji 1.2.11](azure-stack-powershell-install.md).

7. Uruchom skrypt `DeployMySqlProvider.ps1`.

Skrypt wykonuje następujące czynności:

* Pobiera dane binarne MySQL łącznika (to można podać w trybie offline).
* Przekazuje certyfikaty i inne artefaktów na konto magazynu Azure stosu.
* Publikowanie pakietów galerii tak, aby umożliwić wdrożenie bazy danych SQL za pomocą galerii.
* Publikuje pakietu galerii wdrażania serwerami hostingu.
* Wdraża maszynę Wirtualną za pomocą obrazu systemu Windows Server 2016, który został utworzony w kroku 1. Instaluje dostawcę zasobów.
* Rejestruje rekord DNS lokalnego mapy do dostawcy zasobów maszyny Wirtualnej.
* Rejestruje dostawcę zasobów z lokalnej usługi Azure Resource Manager (dzierżawcy i administratora).


Możesz:
- Określ wymagane parametry w wierszu polecenia.
- Uruchom bez żadnych parametrów, a następnie wprowadź je po wyświetleniu monitu.

Oto przykład można uruchamiać w wierszu polecenia programu PowerShell. Pamiętaj zmienić informacje o koncie i hasła w razie potrzeby:


```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack, and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files.
# Find the ERCS01 IP address first, and make sure the certificate
# file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```


### <a name="deploysqlproviderps1-parameters"></a>Parametry DeploySqlProvider.ps1
Te parametry można określić w wierszu polecenia. Jeśli nie chcesz, lub jeśli wszystkich parametrów sprawdzania poprawności zakończy się niepowodzeniem, zostanie wyświetlony monit o zapewnić wymagane parametry.

| Nazwa parametru | Opis | Wartość domyślna lub komentarz |
| --- | --- | --- |
| **CloudAdminCredential** | Poświadczenia dla administratora chmury niezbędne do uzyskiwania dostępu do uprzywilejowanych punktu końcowego. | _Wymagane_ |
| **AzCredential** | Poświadczenia dla konta administratora usługi Azure stosu. Użyj tych samych poświadczeń, które używane do wdrożenia usługi Azure stosu. | _Wymagane_ |
| **VMLocalCredential** | Poświadczenia dla konta administratora lokalnego dostawcy zasobów MySQL maszyny Wirtualnej. | _Wymagane_ |
| **PrivilegedEndpoint** | Adres IP lub nazwa DNS uprzywilejowanych punktu końcowego. |  _Wymagane_ |
| **DependencyFilesLocalPath** | Ścieżka do udziału lokalnego, który zawiera [mysql — łącznik net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Jeśli podasz jednej z tych ścieżek pliku certyfikatu muszą znajdować się w tym również katalogu. | _Opcjonalne_ (_obowiązkowe_ dla wielowęzłowego) |
| **DefaultSSLCertificatePassword** | Hasło dla certyfikatu pfx. | _Wymagane_ |
| **MaxRetryCount** | Ile razy chcesz ponowić próbę każdej operacji w przypadku awarii.| 2 |
| **RetryDuration** | Interwał limitu czasu między kolejnymi próbami w sekundach. | 120 |
| **Dezinstalacja** | Usuwa dostawcy zasobów i wszystkie powiązane zasoby (zobacz uwagi następujące). | Nie |
| **DebugMode** | Uniemożliwia automatyczne czyszczenie w przypadku awarii. | Nie |
| **AcceptLicense** | Pomija wiersz, aby zaakceptować licencji License.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |



W zależności od szybkości wydajności i pobierania systemu instalacja może trwać zaledwie 20 minut lub tak długo, jak kilka godzin. Jeśli **MySQLAdapter** bloku nie jest dostępna, Odśwież z portalu administratora.

> [!NOTE]
> Jeśli instalacja wymaga więcej niż 90 minut, może się nie udać. Jeśli tak, zobaczysz komunikat o błędzie, na ekranie i w pliku dziennika. Wdrożenie próba zostanie ponowiona z kroku się niepowodzeniem. Systemy, które nie spełnia zalecanych wymagań pamięci i podstawowe nie można wdrożyć MySQL RP.



## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Weryfikacja wdrożenia przy użyciu portalu Azure stosu

> [!NOTE]
>  Po zakończeniu skrypt instalacji należy odświeżyć portalu, zobacz blok administratora.


1. Zaloguj się do portalu administratora jako administratora usługi.

2. Sprawdź, czy wdrożenie zakończyło się pomyślnie. Przejdź do **grup zasobów**, a następnie wybierz **systemu.\< Lokalizacja\>.mysqladapter** grupy zasobów. Sprawdź, czy wszystkie cztery wdrożenia zakończyło się pomyślnie.

      ![Sprawdź wdrożenie MySQL RP](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Zapewniają pojemność, łącząc się z serwerem hostingowym MySQL

1. Zaloguj się do portalu Azure stosu jako administratora usługi.

2. Wybierz **zasobów administracyjnych** > **MySQL serwerów hosta** > **+ Dodaj**.

    Na **Hosting serwerów MySQL** bloku można nawiązać dostawcy zasobów serwer MySQL bieżące wystąpienia serwera MySQL służące jako wewnętrzna dostawcy zasobów.

    ![Serwerów hosta](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Podaj szczegóły połączenia z wystąpieniem serwera MySQL. Należy podać pełną nazwę domeny (FQDN) lub prawidłowy adres IPv4, a nie krótką nazwę maszyny Wirtualnej. Ta instalacja nie jest już zawiera domyślne wystąpienie MySQL. Rozmiar, który został dostarczony ułatwia zarządzanie wydajnością bazy danych dostawcy zasobów. Należy go blisko fizycznych zdolności produkcyjnych serwera bazy danych.

    > [!NOTE]
    >Jeśli dzierżawcy i administratora usługi Azure Resource Manager można uzyskać dostępu do wystąpienia MySQL, mogą być umieszczone pod kontrolą dostawcy zasobów. Wystąpienie programu MySQL *musi* można przydzielić wyłącznie do dostawcy zasobów.

4. W miarę dodawania serwerów, należy je przypisać do nowej lub istniejącej jednostki SKU umożliwiają rozróżnienie ofert usług.
  Na przykład można mieć wystąpieniem przedsiębiorstwa dostarczanie:
    - wydajność bazy danych
    - Automatyczne kopie zapasowe
    - Zarezerwuj serwerów o wysokiej wydajności dla poszczególnych działów
 

Nazwa jednostki SKU powinien odzwierciedlać właściwości, dzięki czemu dzierżaw odpowiednio umieszczać swoje bazy danych. Wszystkie serwery hostingu w jednostce SKU powinny mieć takie same możliwości.

![Utwórz MySQL SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
> Jednostki SKU może potrwać do godziny mają być wyświetlane w portalu. Nie można utworzyć bazy danych, dopóki nie zostanie utworzona jednostka SKU.


## <a name="test-your-deployment-by-creating-your-first-mysql-database"></a>Przetestować wdrożenie, tworząc pierwszą bazę danych MySQL


1. Zaloguj się do portalu Azure stosu jako administratora usługi.

2. Wybierz **+ nowy** > **dane i magazyn** > **baza danych MySQL**.

3. Podaj szczegóły bazy danych.

    ![Tworzenie nowego testu baza danych MySQL](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Wybierz jednostki SKU.

    ![Wybierz jednostki SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Utwórz ustawienie logowania. Można ponownie użyć istniejącego ustawienia logowania lub Utwórz nową. To ustawienie zawiera nazwę użytkownika i hasło dla bazy danych.

    ![Utwórz nowe nazwy logowania bazy danych](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Ciąg połączenia zawiera nazwę serwera bazy danych rzeczywistych. Skopiuj go z portalu.

    ![Pobierz ciąg połączenia bazy danych MySQL](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Długość nazwy użytkownika nie może przekraczać 32 znaków MySQL 5.7. We wcześniejszych wersjach go nie może przekraczać 16 znaków.


## <a name="add-capacity"></a>Zwiększanie pojemności

Dodaj pojemność przez dodanie dodatkowych serwerów MySQL w portalu Azure stosu. Dodatkowe serwery można dodać do nowej lub istniejącej jednostki SKU. Upewnij się, że właściwości serwera są takie same.


## <a name="make-mysql-databases-available-to-tenants"></a>Udostępnij dzierżawcom baz danych MySQL
Tworzenie planów i oferty, aby udostępnić baz danych MySQL dla dzierżawców. Na przykład Dodaj usługę Microsoft.MySqlAdapter, Dodaj limit przydziału i tak dalej.

![Tworzenie planów i ofert do dołączenia bazy danych](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="update-the-administrative-password"></a>Zaktualizuj hasło administracyjne
Hasło można modyfikować, zmieniając go pierwszym wystąpieniu serwera MySQL. Wybierz **zasobów administracyjnych** > **MySQL serwerów hosta**. Następnie wybierz serwer hostingu. W **ustawienia** panelu, wybierz opcję **hasło**.

![Zaktualizuj hasło administratora](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="update-the-mysql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Zaktualizuj kartę Dostawca zasobów MySQL (wielowęzłowego tylko kompilacje 1710 i nowsze)
Przy każdej aktualizacji kompilacji stosu Azure wydaniu nowej karty dostawcy zasobów MySQL. Istniejące karty mogą nadal działać. Firma Microsoft zaleca jednak aktualizacja do nowszej kompilacji jak najszybciej po zaktualizowaniu stosu Azure. 

Proces aktualizacji jest podobny do procesu instalacji, który został opisany wcześniej. Utworzeniu nowej maszyny Wirtualnej za pomocą najnowszej kod dostawcy zasobów. Następnie przeprowadzana jest migracja ustawień dla tego nowego wystąpienia, łącznie z bazy danych i obsługi informacji o serwerze. Można również migrację niezbędnych rekordów DNS.

Za pomocą skryptu UpdateMySQLProvider.ps1 te same argumenty, które zostały opisane wcześniej. Podaj certyfikat tutaj również.

> [!NOTE]
> Aktualizacja jest obsługiwana tylko na komputerach z wieloma węzłami.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack and the default prefix is AzS.
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

# Set credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense
 ```

### <a name="updatemysqlproviderps1-parameters"></a>Parametry UpdateMySQLProvider.ps1
Te parametry można określić w wierszu polecenia. Jeśli nie istnieje lub wszystkich parametrów sprawdzania poprawności zakończy się niepowodzeniem, zostanie wyświetlony monit zapewnić wymagane parametry.

| Nazwa parametru | Opis | Wartość domyślna lub komentarz |
| --- | --- | --- |
| **CloudAdminCredential** | Poświadczenia dla administratora chmury niezbędne do uzyskiwania dostępu do uprzywilejowanych punktu końcowego. | _Wymagane_ |
| **AzCredential** | Poświadczenia dla konta administratora usługi Azure stosu. Użyj tych samych poświadczeń, używane do wdrażania usługi Azure stosu. | _Wymagane_ |
| **VMLocalCredential** |Poświadczenia dla konta administratora lokalnego dostawcy zasobów SQL maszyny Wirtualnej. | _Wymagane_ |
| **PrivilegedEndpoint** | Adres IP lub nazwa DNS uprzywilejowanych punktu końcowego. |  _Wymagane_ |
| **DependencyFilesLocalPath** | Plik .pfx certyfikatu muszą znajdować się w tym również katalogu. | _Opcjonalne_ (_obowiązkowe_ dla wielowęzłowego) |
| **DefaultSSLCertificatePassword** | Hasło dla certyfikatu pfx | _Wymagane_ |
| **MaxRetryCount** | Ile razy chcesz ponowić próbę każdej operacji w przypadku awarii.| 2 |
| **RetryDuration** | Interwał limitu czasu między kolejnymi próbami w sekundach. | 120 |
| **Dezinstalacja** | Usuń dostawcę zasobów i wszystkie powiązane zasoby (zobacz uwagi następujące). | Nie |
| **DebugMode** | Uniemożliwia automatyczne czyszczenie w przypadku awarii. | Nie |
| **AcceptLicense** | Pomija wiersz, aby zaakceptować licencji License.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |

## <a name="remove-the-mysql-resource-provider-adapter"></a>Usuń kartę Dostawca zasobów MySQL

Aby usunąć dostawcę zasobów, konieczne jest najpierw usunąć wszelkie zależności.

1. Upewnij się, że masz oryginalny pakiet wdrożeniowy, który został pobrany dla tej wersji dostawcy zasobów.

2. Należy usunąć wszystkie dzierżawy bazy danych od dostawcy zasobów. (Usuwanie baz danych dzierżawy nie powoduje usunięcia danych.) To zadanie należy wykonać przez dzierżawców, samodzielnie.

3. Dzierżawcy wyrejestrować z przestrzeni nazw.

4. Administrator musi usunąć serwerami hostingu z karty MySQL.

5. Administrator, należy usunąć wszystkie plany, które odwołują się do karty MySQL.

6. Administrator musi usunąć wszelkie przydziały, które są skojarzone z kartą MySQL.

7. Uruchom ponownie skrypt wdrożenia wraz z następującymi elementami:
    - Odinstalować parametru
    - Punkty końcowe usługi Azure Resource Manager
    - DirectoryTenantID
    - Poświadczenia dla konta administratora usługi
