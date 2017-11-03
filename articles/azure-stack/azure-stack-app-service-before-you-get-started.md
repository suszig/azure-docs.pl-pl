---
title: "Przed wdrożeniem usługi aplikacji Azure stosu | Dokumentacja firmy Microsoft"
description: "Kroki, aby zakończyć przed wdrożeniem usługi aplikacji Azure stosu"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: f2e7b5b96b70333ae4ee92d24c354960008c7f00
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Przed rozpoczęciem pracy z usługi aplikacji Azure stosu

Usługa Azure App Service na stosie Azure zawiera szereg wstępnie wymagane kroki, które należy wykonać przed wdrożeniem:

- Pobierz usługi aplikacji Azure dotyczące skryptów pomocnika Azure stosu
- Wysoka dostępność
- Certyfikaty wymagane dla usługi Azure App Service na stosie Azure
- Przygotowywanie serwera plików
- Przygotowanie programu SQL Server
- Tworzenie aplikacji usługi Azure Active Directory
- Tworzenie aplikacji usług federacyjnych Active Directory

## <a name="download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts"></a>Pobierz usługi aplikacji Azure, Azure stosu Instalatora i skryptów pomocnika

1. Pobierz [usługi aplikacji na skryptów pomocnika wdrażania stosu Azure](https://aka.ms/appsvconmashelpers).
2. Pobierz [usługi aplikacji na stos Azure Instalatora](https://aka.ms/appsvconmasinstaller).
3. Wyodrębnij pliki z pliku zip skryptów pomocnika. Są wyświetlane następujące pliki i struktury folderów:
  - Common.ps1
  - Utwórz AADIdentityApp.ps1
  - Utwórz ADFSIdentityApp.ps1
  - Utwórz AppServiceCerts.ps1
  - Get-AzureStackRootCert.ps1
  - Usuń AppService.ps1
  - Moduły
    - GraphAPI.psm1
    
## <a name="high-availability"></a>Wysoka dostępność

Usługa aplikacji Azure na stosie Azure nie jest obecnie w stanie oferują wysoką dostępność, ponieważ stos Azure tylko wdraża obciążeń do jednej domeny awarii jednego.

Aby przygotować usłudze Azure App Service na stosie Azure wysokiej dostępności, należy wdrożyć wymagane serwera plików i programu SQL Server w konfiguracji o wysokiej dostępności. Gdy stos Azure obsługuje wiele domen błędów, firma Microsoft będzie zawierają wskazówki dotyczące włączania usługi Azure App Service na stosie Azure w konfiguracji o wysokiej dostępności.


## <a name="certificates-required-for-azure-app-service-on-azure-stack"></a>Certyfikaty wymagane dla usługi Azure App Service na stosie Azure

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Certyfikaty wymagane dla zestawu SDK usługi Azure stosu

Ten skrypt pierwszy współpracuje z urzędu certyfikacji stosu Azure można utworzyć cztery certyfikaty, które są wymagane przez usługę App Service.

| Nazwa pliku | Użycie |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Certyfikat SSL domyślne usługi aplikacji |
| API.appservice.local.azurestack.external.pfx | Certyfikat SSL interfejsu API usługi aplikacji |
| FTP.appservice.local.azurestack.external.pfx | Certyfikat SSL wydawcy usługi aplikacji |
| Sso.appservice.local.azurestack.external.pfx | Certyfikat aplikacji tożsamości usługi aplikacji |

Uruchom skrypt na hoście Azure stosu Development Kit i upewnij się, że używasz programu PowerShell jako azurestack\CloudAdmin.

1. W sesji programu PowerShell uruchomione jako azurestack\CloudAdmin wykonać skryptu Utwórz AppServiceCerts.ps1 z folderu, w którym została rozpakowana skrypty pomocnika. Skrypt tworzy cztery certyfikatów, w tym samym folderze co Utwórz skrypt certyfikaty wymagające usługi aplikacji.
2. Wprowadź hasło, aby zabezpieczyć pliki PFX i zapisz go. Wprowadź go w usłudze App Service w Instalatorze stosu Azure.

#### <a name="create-appservicecertsps1-parameters"></a>Utwórz AppServiceCerts.ps1 parametrów

| Parametr | Wymagane opcjonalne | Wartość domyślna | Opis |
| --- | --- | --- | --- |
| PfxPassword | Wymagane | Wartość null | Hasło używane do ochrony klucza prywatnego certyfikatu |
| domainName | Wymagane | Local.azurestack.external | Azure sufiks regionu i domeny stosu |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Certyfikaty wymagane w przypadku wdrożenia produkcyjnego usługi Azure App Service na stosie Azure

Działanie dostawcy zasobów w środowisku produkcyjnym, należy podać cztery następujących certyfikatów:

#### <a name="default-domain-certificate"></a>Domyślny certyfikat domeny

Domyślny certyfikat domeny jest umieszczany w roli Serwer sieci Web. Jest on używany przez aplikacje użytkownika dla symbolu wieloznacznego lub wartość domyślną żądania domeny w usłudze Azure App Service. Certyfikat służy także do operacji kontroli źródła (KUDU).

Należy certyfikat uniwersalny dwa podmiotu certyfikatu i musi być w formacie pfx. Dzięki temu zarówno domyślnej domeny i punkt końcowy scm dla operacji kontroli źródła mają być objęte jeden certyfikat.

| Format | Przykład |
| --- | --- |
| \*.appservice. \<region\>.\< DomainName\>.\< rozszerzenia\> | \*. appservice.redmond.azurestack.external |
| \*. scm.appservice. <region>. <DomainName>.<extension> | \*. appservice.scm.redmond.azurestack.external |

#### <a name="api-certificate"></a>Certyfikat interfejsu API

Certyfikat interfejsu API jest umieszczona na rolę zarządzania i jest używany przez dostawcę zasobów do zabezpieczania wywołań interfejsu api. Certyfikat do opublikowania musi zawierać którego podmiot odpowiada wpis DNS interfejsu API:

| Format | Przykład |
| --- | --- |
| API.appservice. \<region\>.\< DomainName\>.\< rozszerzenia\> | API.appservice.Redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Publikowanie certyfikatów

Certyfikat dla roli wydawcy zabezpiecza ruch FTPS dla właścicieli aplikacji, gdy ich przekazać zawartość.  Certyfikat musi zawierać, którego podmiot odpowiada wpis FTPS DNS publikowania.

| Format | Przykład |
| --- | --- |
| FTP.appservice. \<region\>.\< DomainName\>.\< rozszerzenia\> | API.appservice.Redmond.azurestack.external |

#### <a name="identity-certificate"></a>Certyfikat tożsamości

Certyfikat tożsamości aplikacji umożliwia:
- Integracja katalogu usługi AAD/ADFS, stos Azure i usługi aplikacji do obsługi integracji z dostawcy zasobów obliczeniowe
- Rejestracja jednokrotna scenariusze dotyczące narzędzia dla zaawansowanych programistów w usłudze Azure App Service na stosie platformy Azure.
Certyfikat tożsamości musi zawierać którego podmiot odpowiada następujący format:

| Format | Przykład |
| --- | --- |
| sso.appservice. \<region\>.\< DomainName\>.\< rozszerzenia\> | sso.appservice.Redmond.azurestack.external |

#### <a name="extract-the-azure-stack-azure-resource-manager-root-certificate"></a>Wyodrębnij certyfikat głównego menedżera zasobów Azure Azure stosu

W sesji programu PowerShell, uruchomione jako azurestack\CloudAdmin Uruchom skrypt Get AzureStackRootCert.ps1 z folderu, w którym została rozpakowana skrypty pomocnika. Skrypt tworzy cztery certyfikatów, w tym samym folderze co Utwórz skrypt certyfikaty wymagające usługi aplikacji.

| Parametr Get-AzureStackRootCert.ps1 | Wymagane opcjonalne | Wartość domyślna | Opis |
| --- | --- | --- | --- |
| PrivelegedEndpoint | Wymagane | AzS ERCS01 | Punkt końcowy uprzywilejowanych. |
| CloudAdminCredential | Wymagane | AzureStack\CloudAdmin | Azure poświadczenia konta domeny administratora chmury stosu |


## <a name="prepare-the-file-server"></a>Przygotowywanie serwera plików

Usługa aplikacji Azure wymaga użycia serwera plików. Wdrożeń produkcyjnych serwer plików musi być skonfigurowana by zapewnić wysoką dostępność i możliwość obsługi błędów.

Do użytku z Azure stosu Development Kit tylko w przypadku wdrożeń, można w tym przykładzie szablonu wdrażania Menedżera zasobów Azure, aby wdrożyć serwer plików skonfigurowany jeden węzeł: https://aka.ms/appsvconmasdkfstemplate.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Zapewnij grup i kont w usłudze Active Directory

>[!NOTE]
> Wszystkie następujące polecenia, należy wykonać podczas konfigurowania serwera plików, w sesji wiersza polecenia administratora.  **NIE należy używać środowiska PowerShell.**

1. Utwórz następujące grupy zabezpieczeń globalnych usługi Active Directory:
    - FileShareOwners
    - FileShareUsers
2. Utwórz następujące konta usługi Active Directory jako konta usług:
    - Właściciel udziału plików
    - FileShareUser

    Bezpieczeństwa najlepiej użytkowników dla tych kont (i dla wszystkich ról sieci Web) powinny się różnić od siebie i mieć użytkownika silne nazwy i hasła.
    Ustaw hasła z następujących warunków:
     - Włącz **hasło nigdy nie wygasa**.
     - Włącz **użytkownik nie może zmienić hasła**.
     - Wyłącz **użytkownik musi zmienić hasło przy następnym logowaniu**.
3. Dodaj konta do członkostwa w grupach w następujący sposób:
    - Dodaj **właściciel udziału plików** do **FileShareOwners** grupy.
    - Dodaj **FileShareUser** do **FileShareUsers** grupy.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Zapewnij grup i kont w grupie roboczej

W grupie roboczej uruchom net i WMIC poleceń do obsługi administracyjnej kont i grup.

1. Uruchom następujące polecenia, aby utworzyć konta Właściciel udziału plików i FileShareUser. Zastąp <password> z własne wartości.
``` DOS
net user FileShareOwner <password> /add /expires:never /passwordchg:no
net user FileShareUser <password> /add /expires:never /passwordchg:no
```
2. Ustaw hasła dla konta, które nigdy nie wygasa, uruchamiając następujące polecenia WMIC:
``` DOS
WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
```
3. Tworzenie grup lokalnych FileShareUsers i FileShareOwners i dodać konta w pierwszym kroku do nich.
``` DOS
net localgroup FileShareUsers /add
net localgroup FileShareUsers FileShareUser /add
net localgroup FileShareOwners /add
net localgroup FileShareOwners FileShareOwner /add
```

### <a name="provision-the-content-share"></a>Udostępnianie zawartości udostępnionej

Udział zawartości zawiera zawartość witryny sieci web dzierżawy. Procedura udostępniania zawartości udostępnionej na jednym serwerze plików jest taka sama dla środowisk usługi Active Directory, jak i grupy roboczej, ale o różnych klastra pracy awaryjnej w usłudze Active Directory.

#### <a name="provision-the-content-share-on-a-single-file-server-ad-or-workgroup"></a>Udostępnianie zawartości udostępnionej na jednym serwerze plików (usługi AD lub grupy roboczej)

Na jednym serwerze plików, uruchom następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień. Zamień wartość dla < C:\WebSites > odpowiednich ścieżkach w danym środowisku.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=<C:\WebSites>
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="to-enable-winrm-add-the-fileshareowners-group-to-the-local-administrators-group"></a>Aby włączyć usługę WinRM, Dodaj grupę FileShareOwners do lokalnej grupy administratorów

Aby zdalne zarządzanie systemem Windows działała poprawnie należy dodać grupę FileShareOwners do lokalnej grupy Administratorzy.

#### <a name="active-directory"></a>Usługa Active Directory

Uruchom następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień na serwerze plików lub na każdym węźle klastra trybu Failover serwera plików. Zastąp wartość <DOMAIN> z nazwą domeny, którego chcesz użyć.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Grupa robocza

Uruchom następujące polecenie w wierszu polecenia z podwyższonym poziomem uprawnień na serwerze plików.

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Konfigurowanie kontroli dostępu do udziałów

Uruchom następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień na serwerze plików lub węzła klastra trybu Failover serwera plików, który jest bieżącym właścicielem zasobu klastra. Zastąp wartości kursywą wartości określonej dla danego środowiska.

#### <a name="active-directory"></a>Usługa Active Directory
```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=<C:\WebSites>
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Grupa robocza
```DOS
set WEBSITES_FOLDER=<C:\WebSites>
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server"></a>Przygotowanie programu SQL Server

Usługi aplikacji Azure na zliczania baz danych i hostingu Azure stosu należy przygotować programu SQL Server do przechowywania bazy danych usługi aplikacji Azure.

Do użytku z Development Kit stosu Azure, można użyć programu SQL Express 2014 z dodatkiem SP2 lub nowszym.

Produkcji i wysoką dostępność, należy użyć pełną wersję programu SQL 2014 SP2 lub później, Włącz uwierzytelnianie w trybie mieszanym oraz wdrożyć w [konfiguracji wysokiej dostępności](https://docs.microsoft.com/en-us/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

Usługi Azure App Service na serwerze SQL Azure stos musi być dostępny ze wszystkich ról usługi aplikacji. SQL Server można wdrożyć w ramach subskrypcji domyślny dostawca w stosie Azure. Można też użyć istniejącej infrastruktury, w ramach organizacji (o ile istnieje łączność stos Azure).

Dla każdej z ról serwera SQL można użyć wystąpienie domyślne lub nazwane wystąpienie. Jednak użycie nazwanego wystąpienia, należy ręcznie uruchomić usługę programu SQL Server Browser i otworzyć port 1434.

## <a name="create-an-azure-active-directory-application"></a>Tworzenie aplikacji usługi Azure Active Directory

Skonfiguruj nazwę główną usługi Azure AD do obsługi następujących czynności:
- Zestawu skalowania maszyn wirtualnych integracji, w warstwach procesu roboczego.
- Usługa rejestracji Jednokrotnej dla narzędzi deweloperskich portalu i zaawansowane funkcje platformy Azure.

Te kroki dotyczą usługi Azure AD zabezpieczone tylko w środowiskach stosu Azure.

Administratorzy, należy skonfigurować logowanie Jednokrotne:
- Włącz narzędzia dla zaawansowanych programistów w ramach usługi App Service (Kudu).
- Korzystanie z obsługi portalu usługi Azure Functions.

Wykonaj następujące kroki:

1. Otwórz wystąpienie programu PowerShell jako azurestack\cloudadmin.
2. Przejdź do lokalizacji skrypty pobrane i wyodrębnione w [wymagań wstępnych krok](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Instalowanie programu PowerShell Azure stosu](azure-stack-powershell-install.md).
4. Uruchom **AADIdentityApp.ps1 Utwórz** skryptu. Gdy zostanie wyświetlony monit o Identyfikatora dzierżawy usługi Azure AD, wprowadź identyfikator dzierżawy usługi Azure AD używaną na potrzeby wdrożenia stosu Azure, na przykład myazurestack.onmicrosoft.com.
5. W **poświadczeń** okna, wprowadź konto administratora usługi Azure AD i hasło. Kliknij przycisk **OK**.
6. Wprowadź hasło certyfikatu i ścieżka do pliku certyfikatu [wcześniej utworzony certyfikat](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Certyfikat utworzony w tym kroku domyślnie jest sso.appservice.local.azurestack.external.pfx.
7. Skrypt tworzy nową aplikację w dzierżawie usługi Azure AD. Zanotuj identyfikator aplikacji, który jest zwracany w danych wyjściowych programu PowerShell. To potrzebne podczas instalacji.
8. Otwórz nowe okno przeglądarki i zaloguj się do portalu Azure (portal.azure.com) jako **administratora usługi Active Directory platformy Azure**.
9. Otwórz dostawcy zasobów usługi Azure AD.
10. Kliknij przycisk **rejestracji aplikacji**.
11. Wyszukaj **identyfikator aplikacji** zwracane w ramach kroku 7. Aplikacja usługi aplikacji znajduje się.
12. Kliknij przycisk **aplikacji** na liście
13. Kliknij przycisk **wymagane uprawnienia** > **udzielić uprawnień** > **tak**.

| Utwórz AADIdentityApp.ps1 parametru | Wymagane opcjonalne | Wartość domyślna | Opis |
| --- | --- | --- | --- |
| DirectoryTenantName | Wymagane | Wartość null | Identyfikator dzierżawy usługi Azure AD Podaj identyfikator GUID lub ciąg, na przykład myazureaaddirectory.onmicrosoft.com |
| AdminArmEndpoint | Wymagane | Wartość null | Azure Resource Manager punktu końcowego administratora, na przykład adminmanagement.local.azurestack.external |
| TenantARMEndpoint | Wymagane | Wartość null | Dzierżawy usługi Azure Resource Manager punktu końcowego, na przykład: management.local.azurestack.external |
| AzureStackAdminCredential | Wymagane | Wartość null | Poświadczenie administratora usługi Azure AD |
| CertificateFilePath | Wymagane | Wartość null | Ścieżka do pliku certyfikatu tożsamości aplikacji, wcześniej wygenerowany. |
| CertificatePassword | Wymagane | Wartość null | Hasło używane do ochrony klucza prywatnego certyfikatu. |

## <a name="create-an-active-directory-federation-services-application"></a>Tworzenie aplikacji usług federacyjnych Active Directory

W przypadku środowisk Azure stosu zabezpieczonej przez usługi AD FS należy skonfigurować nazwy głównej usługi AD FS do obsługi następujących czynności:
- Zestawu skalowania maszyn wirtualnych integracji, w warstwach procesu roboczego.
- Usługa rejestracji Jednokrotnej dla narzędzi deweloperskich portalu i zaawansowane funkcje platformy Azure.

Administratorzy, należy skonfigurować logowanie Jednokrotne:
- Konfigurowanie nazwy głównej usługi integracji zestawu skali maszyny wirtualnej w warstwach procesu roboczego.
- Włącz narzędzia dla zaawansowanych programistów w ramach usługi App Service (Kudu).
- Korzystanie z obsługi portalu usługi Azure Functions.

Wykonaj następujące kroki:

1. Otwórz wystąpienie programu PowerShell jako azurestack\azurestackadmin.
2. Przejdź do lokalizacji skrypty pobrane i wyodrębnione w [wymagań wstępnych krok](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Instalowanie programu PowerShell Azure stosu](azure-stack-powershell-install.md).
4.  Uruchom **ADFSIdentityApp.ps1 Utwórz** skryptu.
5.  W **poświadczeń** okna, wprowadź konto administratora usług AD FS chmury i hasło. Kliknij przycisk **OK**.
6.  Ścieżka do pliku certyfikatu i hasła certyfikatu dla [wcześniej utworzony certyfikat](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Certyfikat utworzony w tym kroku domyślnie jest sso.appservice.local.azurestack.external.pfx.

| Utwórz ADFSIdentityApp.ps1 parametru | Wymagane opcjonalne | Wartość domyślna | Opis |
| --- | --- | --- | --- |
| AdminArmEndpoint | Wymagane | Wartość null | Punkt końcowy usługi Azure Resource Manager Administrator. Na przykład adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Wymagane | Wartość null | Punkt końcowy uprzywilejowanych. Na przykład AzS ERCS01. |
| CloudAdminCredential | Wymagane | Wartość null | Azure stosu cloudadmin domeny poświadczeń dla konta. Na przykład Azurestack\CloudAdmin. |
| CertificateFilePath | Wymagane | Wartość null | Ścieżka do pliku PFX certyfikatu tożsamości aplikacji. |
| CertificatePassword | Wymagane | Wartość null | Hasło używane do ochrony klucza prywatnego certyfikatu. |


## <a name="next-steps"></a>Następne kroki

[Zainstaluj dostawcę zasobów usługi aplikacji](azure-stack-app-service-deploy.md).
