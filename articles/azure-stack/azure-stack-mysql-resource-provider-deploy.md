---
title: "Użyj bazy danych MySQL jako PaaS w stosie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć MySQL dostawcy zasobów i zapewnić baz danych MySQL jako usługa na stosie Azure"
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
ms.openlocfilehash: 71abceb1afe315a09ea88b593f9806e9e8b31f16
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Użyj bazy danych MySQL na Microsoft Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Dostawca zasobów MySQL na stosie Azure można wdrożyć. Po wdrożeniu dostawcy zasobów można tworzyć MySQL serwerów i baz danych za pośrednictwem usługi Azure Resource Manager deployment szablonów i podaj baz danych MySQL jako usługa. Bazy danych MySQL, które są wspólne w witrynach sieci web, obsługuje wiele platform witryny sieci Web. Na przykład po wdrożeniu dostawcy zasobów, można utworzyć witryny sieci Web WordPress z platformy Azure Web Apps jako dodatek usługa (PaaS) Azure stosu.

Aby wdrożyć dostawcy MySQL na komputerze, który nie ma dostępu do Internetu, należy skopiować plik [mysql — łącznik net-6.9.9.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.9.9.msi) do udziału lokalnego. Następnie podaj nazwę tego udziału, po wyświetleniu monitu. Należy również zainstalować moduły Azure oraz Azure PowerShell stosu.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Architektura karty dostawcy zasobów serwera MySQL

Dostawca zasobów składa się z trzech składników:

- **Karta Dostawca zasobów MySQL maszyny Wirtualnej**, który jest maszyną wirtualną systemu Windows z usługami dostawcy.
- **Dostawca zasobów sam**, który przetwarza żądania alokacji i ujawnia bazy danych zasobów.
- **Serwery obsługujące serwer MySQL**, które zapewniają pojemność baz danych, nazywane Hosting serwerów.

Ta wersja nie jest już tworzy wystąpienie MySQL. Należy je utworzyć i zapewnienia dostępu do zewnętrznych wystąpień programu SQL. Odwiedź stronę [galerii Szybki Start Azure stosu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) przykład szablonu, który można:
- Utwórz serwer MySQL dla Ciebie
- Pobieranie i wdrażanie serwera MySQL z witryny Marketplace.

> [!NOTE]
> Zainstalowana na stosie Azure wielowęzłowego serwerów hosta muszą być tworzone z subskrypcji dzierżawcy. Nie można ich utworzyć z subskrypcji domyślny dostawca. Innymi słowy muszą one zostać utworzone w portalu dzierżawcy lub w sesji programu PowerShell z odpowiednią nazwą logowania. Wszystkie serwery hostingu maszyn wirtualnych mogą być obciążane i musi mieć odpowiednie licencje. Administrator usługi może być właścicielem tej subskrypcji.

### <a name="required-privileges"></a>Wymagane uprawnienia
Konto system musi mieć następujące uprawnienia:

1.  Baza danych: Utwórz, Porzuć
2.  Logowania: Tworzenie, ustawić, porzucić, przydzielić, odwołać

## <a name="deploy-the-resource-provider"></a>Wdrażanie dostawcy zasobów

1. Jeśli jeszcze nie zarejestrować Twojego zestawu programowanie i pobranie obrazu systemu Windows Server 2016 Datacenter Core do pobrania za pośrednictwem zarządzania Marketplace. Należy użyć obrazu systemu Windows Server 2016 Core. Umożliwia także skrypt do utworzenia [obrazu systemu Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) — należy wybrać opcję core. Środowisko uruchomieniowe .NET 3.5 nie jest już wymagane.


2. Zaloguj się do hosta, który można uzyskać dostępu do uprzywilejowanych punktu końcowego maszyny Wirtualnej.

    a. W instalacjach Azure stosu Development Kit (ASDK) Zaloguj się do hosta fizycznego.

    b. W systemach z wieloma węzłami host musi być systemu, w którym można uzyskać dostępu do uprzywilejowanych punktu końcowego.
    
    >[!NOTE]
    > System, w którym skrypt jest uruchamiany *musi* jako system Windows 10 lub Windows Server 2016 z najnowszą wersją środowiska uruchomieniowego .NET zainstalowane. Instalacja zakończy się niepowodzeniem, w przeciwnym razie wartość. ASDK host spełnia te kryteria.
    

3. Pobierz dostawcę zasobów MySQL binarne i wykonaj samorozpakowujący się plik typu wyodrębnienie zawartości do katalogu tymczasowego.

    >[!NOTE] 
    > Kompilacja dostawcy zasobów odnosi się do stosu Azure kompilacji. Należy pobrać poprawne dane binarne dla wersji stosu Azure, w którym jest uruchomiony.

    | Azure stosu kompilacji | Instalator MySQL RP |
    | --- | --- |
    | 1.0.171122.1 | [MySQL RP wersji 1.1.12.0](https://aka.ms/azurestackmysqlrp) |
    | 1.0.171028.1 | [MySQL RP wersji 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |
    | 1.0.170928.3 | [MySQL RP wersji 1.1.3.0](https://aka.ms/azurestackmysqlrp1709) |

4.  Certyfikat główny stos Azure są pobierane z punktu końcowego uprzywilejowanych. ASDK, aby uzyskać certyfikat z podpisem własnym jest tworzony w ramach tego procesu. Wieloma węzłami trzeba podać odpowiedni certyfikat.

    Jeśli musisz dostarczyć własny certyfikat, należy umieścić w pliku PFX **DependencyFilesLocalPath** (patrz poniżej) w następujący sposób:

    - Certyfikat uniwersalny dla \*.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\> lub certyfikat w jednej lokacji z nazwą pospolitą mysqladapter.dbadapter.\< region\>.\< nazwy fqdn zewnętrznej\>
    - Ten certyfikat musi być zaufany, takie jak czy wystawiony przez urząd certyfikacji. Oznacza to łańcuch zaufania musi istnieć bez konieczności używania certyfikatów pośrednich.
    - Istnieje tylko jeden certyfikat plik w DependencyFilesLocalPath.
    - Nazwa pliku nie może zawierać żadnych znaków specjalnych.



5. Otwórz **nowe** z podwyższonym poziomem uprawnień (administracyjnych) konsoli programu PowerShell i zmian w katalogu, w którym zostały wyodrębnione pliki. Użyj nowego okna, aby uniknąć problemów, które mogą wystąpić podczas niepoprawne już załadowanych modułów programu PowerShell w systemie.

6. [Zainstaluj program Azure PowerShell w wersji 1.2.11](azure-stack-powershell-install.md).

7. Uruchom skrypt DeploySqlProvider.ps1.

Skrypt wykonuje następujące czynności:

* Pobierz plik binarny łącznika MySQL (to można podać w trybie offline).
* Przekaż certyfikaty i inne artefaktów na konto magazynu na stosie Azure.
* Publikowanie pakietów galerii, dzięki czemu można wdrożyć bazy danych SQL za pomocą galerii.
* Publikowanie pakietu galerii w celu wdrażania serwerów hostingu
* Wdróż Maszynę wirtualną przy użyciu obrazu systemu Windows Server 2016 utworzonego w kroku 1 i zainstaluj dostawcę zasobów.
* Zarejestruj rekord DNS lokalnego mapy do dostawcy zasobów maszyny Wirtualnej.
* Rejestrowanie dostawcy zasobów z lokalnej usługi Azure Resource Manager (dzierżawcy i administratora).


Możesz:
- Określ co najmniej wymaganych parametrów w wierszu polecenia
- lub, jeśli zostanie uruchomiony bez żadnych parametrów, wprowadź je po wyświetleniu monitu.

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
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privleged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files
# Find the ERCS01 IP address first and make sure the certificate
# file is in the specified directory
. $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```


### <a name="deploysqlproviderps1-parameters"></a>Parametry DeploySqlProvider.ps1
Te parametry można określić w wierszu polecenia. Jeśli nie chcesz lub wszystkich parametrów sprawdzania poprawności zakończy się niepowodzeniem, zostanie wyświetlony monit podaj wymaganych pól.

| Nazwa parametru | Opis | Wartość domyślna lub komentarz |
| --- | --- | --- |
| **CloudAdminCredential** | Poświadczenia dla administratora chmury niezbędne do uzyskiwania dostępu do punktu końcowego Privleged. | _Wymagane_ |
| **AzCredential** | Podaj poświadczenia dla konta administratora usługi Azure stosu. Użyj tych samych poświadczeń używane do wdrażania stosu Azure). | _Wymagane_ |
| **VMLocalCredential** | Zdefiniuj poświadczenia dla konta administratora lokalnego dostawcy zasobów MySQL maszyny Wirtualnej. | _Wymagane_ |
| **PrivilegedEndpoint** | Podaj adres IP lub nazwę DNS uprzywilejowanych punktu końcowego. |  _Wymagane_ |
| **DependencyFilesLocalPath** | Ścieżka do udziału lokalnego nadrzędnym [mysql — łącznik net-6.9.9.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.9.9.msi). Jeśli podasz, plik certyfikatu muszą znajdować się w tym również katalogu. | _opcjonalne_ (_obowiązkowe_ dla wielowęzłowego) |
| **DefaultSSLCertificatePassword** | Hasło dla certyfikatu pfx | _Wymagane_ |
| **Wartość MaxRetryCount** | Zdefiniuj jak często chcesz ponowić próbę każdej operacji w przypadku awarii.| 2 |
| **RetryDuration** | Zdefiniuj limit czasu między kolejnymi próbami w sekundach. | 120 |
| **Dezinstalacja** | Usuń dostawcę zasobów i wszystkie powiązane zasoby (zobacz uwagi poniżej) | Nie |
| **DebugMode** | Uniemożliwia automatyczne oczyszczania po awarii | Nie |
| **AcceptLicense** | Pomija wiersz, aby zaakceptować licencji License (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


W zależności od szybkości wydajności i pobierania systemu instalacja może zająć zaledwie 20 minut lub tak długo, jak kilka godzin. Jeśli blok MySQLAdapter nie jest dostępna, Odśwież z portalu administratora.

> [!NOTE]
> Jeśli instalacja wymaga więcej niż 90 minut, może nie powieść, i zobaczysz komunikat o błędzie na ekranie i w pliku dziennika. Wdrożenie próba zostanie ponowiona z kroku się niepowodzeniem. Systemy, które nie spełnia zalecanych wymagań pamięci i podstawowe nie można wdrożyć MySQL RP.



## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Weryfikacja wdrożenia przy użyciu portalu Azure stosu

> [!NOTE]
>  Po ukończeniu działania skryptu instalacji, należy odświeżyć portalu, zobacz blok administratora.


1. Zaloguj się do portalu administratora jako administratora usługi.

2. Sprawdź, czy wdrożenie zakończyło się pomyślnie. Przeglądaj w poszukiwaniu **grup zasobów** &gt;, kliknij **systemu.\< Lokalizacja\>.mysqladapter** zasobów grupy i sprawdź, czy wszystkie cztery wdrożenia zakończyło się pomyślnie.

      ![Sprawdź wdrożenie MySQL RP](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Zapewniają pojemność, łącząc się z serwerem hostingowym MySQL

1. Zaloguj się do portalu Azure stosu jako administratora usługi.

2. Przejdź do **zasobów administracyjnych** &gt; **MySQL serwerów hosta** &gt; **+ Dodaj**.

    **Hosting serwerów MySQL** bloku jest podłączenia dostawcy zasobów programu MySQL Server do rzeczywistego wystąpień serwera MySQL służące jako wewnętrznej bazy danych dostawcy zasobów.

    ![Serwerów hosta](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Wypełnienie formularza Szczegóły połączenia z wystąpieniem serwera MySQL. Podaj w pełni kwalifikowaną nazwę (FQDN) lub prawidłowy adres IPv4, a nie krótką nazwę maszyny Wirtualnej. Ta instalacja nie jest już zawiera domyślne wystąpienie MySQL. Rozmiar podany pomaga dostawcy zasobów, zarządzanie wydajnością bazy danych. Należy go blisko fizycznych zdolności produkcyjnych serwera bazy danych.

    > [!NOTE]
    > Tak długo, jak dzierżawy i administratora usługi Azure Resource Manager można uzyskać dostępu do wystąpienia MySQL, mogą być umieszczone pod kontrolą dostawcy zasobów. Wystąpienie programu MySQL __musi__ można przydzielić wyłącznie do planu odzyskiwania.

4. W miarę dodawania serwerów, należy je przypisać do nowej lub istniejącej jednostki SKU umożliwiają rozróżnienie ofert usług.
  Na przykład można mieć wystąpieniem przedsiębiorstwa dostarczanie:
    - wydajność bazy danych
    - Automatyczne kopie zapasowe
    - Zarezerwuj serwerów o wysokiej wydajności dla poszczególnych działów
 

Nazwa jednostki SKU powinien odzwierciedlać właściwości, dzięki czemu dzierżaw odpowiednio umieszczać swoje bazy danych. Wszystkie serwery hostingu w jednostce SKU powinny mieć takie same możliwości.

![Utwórz MySQL SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
> Jednostki SKU może potrwać do godziny mają być wyświetlane w portalu. Nie można utworzyć bazy danych, dopóki nie zostanie utworzona jednostka SKU.


## <a name="to-test-your-deployment-create-your-first-mysql-database"></a>Aby przetestować wdrożenie, należy utworzyć pierwszą bazę danych MySQL


1. Zaloguj się do portalu Azure stosu jako administratora usługi.

2. Kliknij przycisk **+ nowy** przycisk &gt; **dane i magazyn** &gt; **baza danych MySQL**.

3. Wypełnij formularz ze szczegółami bazy danych.

    ![Tworzenie nowego testu baza danych MySQL](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Wybierz jednostki SKU.

    ![Wybierz jednostki SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Utwórz ustawienie logowania. Mogą być ponownie używane ustawienia logowania lub utworzyć nowy. To ustawienie zawiera nazwę użytkownika i hasło dla bazy danych.

    ![Utwórz nowe nazwy logowania bazy danych](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Ciąg połączenia zawiera nazwę serwera bazy danych rzeczywistych. Skopiuj go z portalu.

    ![Pobierz ciąg połączenia bazy danych MySQL](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Długość nazwy użytkownika nie może przekraczać 32 znaków i MySQL 5.7 lub 16 znaków w starszych wersjach.


## <a name="add-capacity"></a>Zwiększanie pojemności

Dodaj pojemność przez dodanie dodatkowych serwerów MySQL w portalu Azure stosu. Dodatkowe serwery można dodać do nowej lub istniejącej jednostki SKU. Upewnij się, że właściwości serwera są takie same.


## <a name="make-mysql-databases-available-to-tenants"></a>Udostępnij dzierżawcom baz danych MySQL
Tworzenie planów i oferty, aby udostępnić baz danych MySQL dla dzierżawców. Dodaj usługę Microsoft.MySqlAdapter, dodać przydział itp.

![Tworzenie planów i ofert do dołączenia bazy danych](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="update-the-administrative-password"></a>Zaktualizuj hasło administracyjne
Hasło można modyfikować, zmieniając go pierwszym wystąpieniu serwera MySQL. Przejdź do **zasobów administracyjnych** &gt; **Hosting serwerów MySQL** &gt; i kliknij na serwerze hostingu. W Panelu ustawień kliknij hasła.

![Zaktualizuj hasło administratora](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="update-the-mysql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Zaktualizuj kartę Dostawca zasobów MySQL (wielowęzłowego tylko kompilacje 1710 i nowsze)
Przy każdej aktualizacji kompilacji stosu Azure zostanie wydana nowa karta dostawcy zasobów MySQL. Gdy istniejącej karty mogą nadal działać, zaleca się jak najszybciej zaktualizować do nowszej kompilacji po zaktualizowaniu stosu Azure. Proces aktualizacji jest bardzo podobny do procesu instalacji opisane powyżej. Nowa maszyna wirtualna zostanie utworzona kodem RP najnowsze i ustawienia zostaną zmigrowane do tego nowego wystąpienia, łącznie z bazy danych i obsługi informacji o serwerze, a także niezbędnych rekordów DNS.

Za pomocą skryptu UpdateMySQLProvider.ps1 te same argumenty co powyżej. Należy również podać certyfikatu w tym miejscu.

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
. $tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense
 ```

### <a name="updatemysqlproviderps1-parameters"></a>Parametry UpdateMySQLProvider.ps1
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
| **AcceptLicense** | Pomija wiersz, aby zaakceptować licencji License (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |

## <a name="remove-the-mysql-resource-provider-adapter"></a>Usuń kartę Dostawca zasobów MySQL

Aby usunąć dostawcę zasobów, konieczne jest najpierw usunąć wszelkie zależności.

1. Upewnij się, że oryginalne pakiet wdrożeniowy, który został pobrany dla tej wersji dostawcy zasobów.

2. Należy usunąć wszystkie dzierżawy bazy danych od dostawcy zasobów (nie powoduje usunięcia danych). To zadanie należy wykonać przez dzierżawców, samodzielnie.

3. Dzierżawcy wyrejestrować z przestrzeni nazw.

4. Administrator musi usunąć serwerami hostingu z karty MySQL

5. Administrator, należy usunąć wszystkie plany, które odwołują się do karty MySQL.

6. Administrator musi usunąć wszelkie przydziały skojarzona z kartą MySQL.

7. Uruchom ponownie skrypt wdrożenia z odinstalować parametru, punktów końcowych usługi Azure Resource Manager, DirectoryTenantID i poświadczenia dla konta administratora usługi.




## <a name="next-steps"></a>Kolejne kroki

Spróbuj innych [usług PaaS](azure-stack-tools-paas-services.md) jak [dostawcy zasobów programu SQL Server](azure-stack-sql-resource-provider-deploy.md) i [dostawcy zasobów usługi aplikacji](azure-stack-app-service-overview.md).
