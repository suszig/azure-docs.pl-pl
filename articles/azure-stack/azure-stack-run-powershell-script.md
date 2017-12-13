---
title: "Wdrażanie Azure stosu Development Kit | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przygotować Azure stosu Development Kit i uruchom skrypt programu PowerShell do wdrożenia."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 7c320c6ba51ae0800407aab7aee92c42b2b441a7
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Wdrażanie Azure stosu Development Kit

*Dotyczy: Azure stosu Development Kit*

Aby wdrożyć [Azure stosu Development Kit](azure-stack-poc.md), należy wykonać następujące czynności:

1. [Pobierz pakiet wdrożeniowy](https://azure.microsoft.com/overview/azure-stack/try/?v=try) uzyskanie Cloudbuilder.vhdx.
2. Przygotuj cloudbuilder.vhdx do skonfigurowania komputera (Programowanie hosta kit), na którym chcesz zainstalować zestawu SDK. Po wykonaniu tego kroku hosta zestawu programowanie uruchomi się do Cloudbuilder.vhdx.
3. Wdróż zestawie na hoście development kit.

> [!NOTE]
> Aby uzyskać najlepsze wyniki nawet jeśli ma być używany w środowisku bez połączenia stosu Azure najlepiej do wdrożenia podczas połączenia z Internetem. W ten sposób można aktywować wersję ewaluacyjną systemu Windows Server 2016 dołączone do rozwoju Instalacja zestawu w czasie wdrażania.

## <a name="download-and-extract-the-development-kit"></a>Pobierać i wyodrębniać z zestawem deweloperskim
1. Aby rozpocząć pobieranie, upewnij się, że komputer spełnia następujące wymagania wstępne:

  - Komputer musi mieć co najmniej 60 GB wolnego miejsca na dysku na czterech oddzielnych, identyczne logiczne dyski twarde dodatkowo do dysku systemu operacyjnego.
  - [.NET framework 4.6 (lub nowszym)](https://aka.ms/r6mkiy) musi być zainstalowany.

2. [Przejdź do strony wprowadzenie](https://azure.microsoft.com/overview/azure-stack/try/?v=try) gdy można pobrać Azure stosu Development Kit, podać swoje szczegóły dotyczące i kliknięcie **przesyłania**.
3. Pobierz i uruchom [sprawdzanie wdrożenia dla zestawu SDK usługi Azure stosu](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) skryptu narzędzia sprawdzania wymagań wstępnych. Ten skrypt autonomiczny przechodzi przez kontrole wstępne wykonywane przez Instalatora dla zestawu SDK usługi Azure stosu. Zapewnia sposób upewnij się, że są spełniane wymagania dotyczące sprzętu i oprogramowania przed pobraniem większych pakietu dla zestawu SDK usługi Azure stosu.
4. W obszarze **Pobierz oprogramowanie**, kliknij przycisk **Azure stosu Development Kit**.

  > [!NOTE]
  > Pobieranie ASDK (AzureStackDevelopmentKit.exe) jest approximiately 10GB przez samego siebie. Jeśli użytkownik chce również pobrać plik ISO wersji ewaluacyjnej systemu Windows Server 2016, rozmiar pobierania zwiększa się do około 17GB. Aby utworzyć i dodać obraz maszyny wirtualnej systemu Windows Server 2016 w portalu Azure Marketplace stosu, po zakończeniu instalacji ASDK, można użyć tego pliku ISO. Należy pamiętać, że ten obraz oceny systemu Windows Server 2016 można używać tylko z ASDK i jest z zastrzeżeniem postanowień licencyjnych ASDK.

5. Po zakończeniu pobierania kliknij **Uruchom** można uruchomić ASDK samorozpakowujący się plik typu (AzureStackDevelopmentKit.exe).
6. Przejrzyj i zaakceptuj umowę licencyjną wyświetlanych z **umowy licencyjnej** strony kreatora samorozpakowujący się plik typu, a następnie kliknij przycisk **dalej**.
7. Zapoznaj się z informacjami instrukcji prywatności wyświetlany na **ważna Uwaga** strony kreatora samorozpakowujący się plik typu, a następnie kliknij przycisk **dalej**.
8. Wybierz lokalizację plików instalacyjnych stosu Azure do wyodrębnienia za **wybierz lokalizację docelową** strony kreatora samorozpakowujący się plik typu, a następnie kliknij przycisk **dalej**. Domyślna lokalizacja to *bieżący folder*\Azure stosu Development Kit. 
9. Przejrzyj podsumowanie w lokalizacji docelowej **gotowy do wyodrębniania** strony kreatora samorozpakowujący się plik typu, a następnie kliknij przycisk **wyodrębnić** wyodrębnić CloudBuilder.vhdx (około 25 GB) i Pliki ThirdPartyLicenses.rtf. Ten proces trwa trochę czasu, aby zakończyć.
10. Skopiować lub przenieść pliku CloudBuilder.vhdx w katalogu głównym dysku C:\ (C:\CloudBuilder.vhdx) na komputerze hosta ASDK.

> [!NOTE]
> Po wyodrębnieniu plików, można usunąć. EXE i. BIN plików, aby odzyskać miejsce na dysku twardym. Alternatywnie można tworzyć kopie zapasowe zapasowej tych plików, dzięki czemu nie trzeba ponownie pobrać pliki, jeśli chcesz ponownie wdrożyć ASDK.

## <a name="deploy-the-asdk-using-a-guided-experience"></a>Wdrażanie ASDK przy użyciu użytkownik otrzymuje niezbędne instrukcje
ASDK można wdrożyć przy użyciu graficznego interfejsu użytkownika (GUI) podane przez pobranie i uruchomienie skryptu programu PowerShell asdk installer.ps1.

> [!NOTE]
> Instalator interfejsu użytkownika dla zestawu SDK usługi Azure stosu jest Otwórz skrypt pochodzące z wyszukiwania na podstawie WCF i programu PowerShell.

### <a name="prepare-the-development-kit-host-using-a-guided-user-experience"></a>Przygotowanie hosta zestawu Programowanie przy użyciu z przewodnikiem użytkowników
Przed zainstalowaniem ASDK na komputerze-hoście, należy przygotować środowisko ASDK.
1. Zaloguj się jako Administrator lokalny na komputerze hosta ASDK.
2. Upewnij się, że plik CloudBuilder.vhdx został przeniesiony do katalogu głównego dysku C:\ (C:\CloudBuilder.vhdx).
3. Uruchom następujący skrypt, aby pobrać plik Instalatora development kit (asdk installer.ps1) z [repozytorium narzędzia Azure stosu GitHub](https://github.com/Azure/AzureStack-Tools) do **C:\AzureStack_Installer** folderu na użytkownika Development kit hosta komputera:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Z konsoli programu PowerShell z podwyższonym poziomem uprawnień uruchom **C:\AzureStack_Installer\asdk-installer.ps1** skrypt, a następnie kliknij przycisk **przygotować środowisko**.
5. Na **vhdx wybierz Cloudbuilder** strony Instalatora, Znajdź i wybierz **cloudbuilder.vhdx** pliku, który pobrane i wyodrębnione w poprzednich krokach. Na tej stronie można również, opcjonalnie, Włącz **dodać sterowniki** pole wyboru, aby dodać sterowniki do komputera hosta development kit.  
6. Na **opcjonalne ustawienia** Podaj konto administratora lokalnego komputera hosta development kit. 

  > [!IMPORTANT]
  > Jeśli nie podasz tych poświadczeń, należy bezpośrednio lub KVM dostęp do hosta po ponownym uruchomieniu komputera w ramach konfigurowania zestaw deweloperski.

7. Można też podać tych opcjonalnych ustawień na **opcjonalne ustawienia** strony:
    - **ComputerName**: Ta opcja umożliwia ustawienie nazwy hosta development kit. Nazwa musi spełniać wymagania dotyczące w pełni kwalifikowaną nazwę domeny i musi być 15 znaków lub mniej długości. Wartość domyślna to losową nazwę komputera wygenerowany przez system Windows.
    - **Strefa czasowa**: ustawia strefę czasową dla hosta development kit. Wartość domyślna to (UTC-8:00) czas pacyficzny (USA i Kanada).
    - **Konfiguracji statycznych adresów IP**: ustawia danego wdrożenia, aby użyć statycznego adresu IP. W przeciwnym razie gdy Instalator wykonuje ponowny rozruch w cloudbuilder.vhx, interfejsów sieciowych są skonfigurowane przy użyciu protokołu DHCP.
11. Kliknij przycisk **Dalej**.
12. Jeśli w poprzednim kroku wybrano konfiguracji statycznych adresów IP, należy teraz:
    - Wybierz kartę sieciową. Upewnij się, że możesz nawiązać połączenie karty przed kliknięciem przycisku **dalej**.
    - Upewnij się, że **adres IP**, **bramy**, i **DNS** wartości są poprawne, a następnie kliknij przycisk **dalej**.
13. Kliknij przycisk **dalej** można uruchomić proces przygotowywania.
14. Po przygotowaniu wskazuje **Ukończono**, kliknij przycisk **dalej**.
15. Kliknij przycisk **ponowny rozruch teraz** rozruchu cloudbuilder.vhdx i kontynuować proces wdrażania.


### <a name="deploy-the-development-kit-using-a-guided-experience"></a>Wdrażanie zestaw deweloperski, korzystanie z przewodnikiem obsługi
Po przygotowaniu komputera-hosta ASDK, ASDK można wdrożyć w obrazie CloudBuilder.vhdx, wykonując następujące kroki. 
1. Po uruchomieniu pomyślnie komputera hosta w obrazie CloudBuilder.vhdx, zaloguj się przy użyciu poświadczeń administratora określone w poprzednich krokach. 
2. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień i uruchom **\AzureStack_Installer\asdk-installer.ps1** skryptu (która może znajdować się na dysku w obrazie CloudBuilder.vhdx). Kliknij pozycję **Zainstaluj**.
3. W **typu** listy rozwijanej wybierz pozycję **chmury Azure** lub **usług AD FS**.
    - **Chmury Azure**: umożliwia skonfigurowanie usługi Azure Active Directory (Azure AD) jako dostawcy tożsamości. Aby użyć tej opcji, będzie konieczne połączenie internetowe, pełna nazwa usługi Azure AD directory dzierżawy w formie *domainname*. onmicrosoft.com, a poświadczenia administratora globalnego dla określonego katalogu. 
    - **Usługi AD FS**: sygnatura domyślna usługa katalogowa będzie używany jako dostawca tożsamości. Zaloguj się przy użyciu domyślnego konta jest azurestackadmin@azurestack.local, i hasło używane jest dostarczana jako część instalacji.
4. W obszarze **hasło administratora lokalnego**w **hasło** wpisz hasło administratora lokalnego (która musi odpowiadać bieżące hasło administratora lokalnego skonfigurowany), a następnie kliknij przycisk **Dalej**.
5. Wybierz kartę sieciową do użycia dla zestawu SDK, a następnie kliknij przycisk **dalej**.
6. Wybierz DHCP lub konfiguracji statycznej sieci dla maszyny wirtualnej BGPNAT01.
    - **DHCP** (domyślnie): maszyny wirtualnej pobiera konfigurację sieci IP od serwera DHCP.
    - **Statyczne**: tej opcji należy używać tylko wtedy, gdy DHCP nie można przypisać prawidłowy adres IP dla stosu Azure na dostęp do Internetu. Należy określić statyczny adres IP o długości maska podsieci w formacie CIDR (na przykład 10.0.0.5/24).
7. Opcjonalnie ustaw następujące wartości:
    - **Identyfikator sieci VLAN**: Określa identyfikator sieci VLAN. Tej opcji należy używać tylko wtedy, gdy host i AzS BGPNAT01 należy skonfigurować identyfikator sieci VLAN umożliwiający dostęp do sieci fizycznej (i Internetu). 
    - **Usługa przesyłania dalej DNS**: serwer DNS jest tworzony jako część wdrożenia stosu Azure. Aby umożliwić komputerom wewnątrz rozwiązania do rozpoznawania nazw poza sygnatury, podaj istniejącej infrastruktury serwera DNS. Serwer DNS w sygnatury przekazuje żądania rozpoznania nieznanej nazwy do tego serwera.
    - **Serwer czasu**: to wymagane pole ustawia czas server używanego przez zestaw deweloperski. Ten parametr musi być dostarczona jako adres IP serwera czas ważności. Nazwy serwerów nie są obsługiwane.
  
  > [!TIP]
  > Aby znaleźć adres IP serwera czasu, odwiedź stronę [pool.ntp.org](http:\\pool.ntp.org) lub polecenie ping time.windows.com. 
  
8. Kliknij przycisk **Dalej**. 
9. Na **Weryfikowanie właściwości karty interfejsu sieciowego** strony, zostanie wyświetlony pasek postępu. 
    - Informacja **nie można pobrać aktualizacji**, postępuj zgodnie z instrukcjami na stronie.
    - Gdy mówi **Ukończono**, kliknij przycisk **dalej**.
10. Na **Podsumowanie** kliknij przycisk **Wdróż**. W tym miejscu można także skopiować ustawienia poleceń programu PowerShell, które będą używane do zainstalowania z zestawem deweloperskim.
11. Jeśli używasz wdrożenia usługi Azure AD, możesz wyświetlony monit o wprowadzenie poświadczeń konta administratora globalnego usługi Azure AD za kilka minut po uruchomieniu Instalatora.
12. Proces wdrażania może potrwać kilka godzin, w których automatycznie ponownego raz. Gdy wdrożenie zakończy się powodzeniem, konsoli programu PowerShell Wyświetla: **COMPLETE: Akcja "Wdrożenie"**. Jeśli wdrożenie nie powiedzie się, można możesz [ponownie wdrożyć](azure-stack-redeploy.md) od podstaw lub użyj programu PowerShell następujących poleceń, z tego samego okna programu PowerShell z podwyższonym poziomem uprawnień, uruchom ponownie wdrażanie w ostatnim kroku powiodło się:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

   > [!IMPORTANT]
   > Jeśli chcesz monitorować postęp wdrażania, zaloguj się jako azurestack\AzureStackAdmin po uruchomieniu hosta zestawu programowanie podczas instalacji. Jeśli zarejestrujesz się jako administrator lokalny po komputer jest przyłączony do domeny, nie będzie wyświetlany jest postęp wdrażania. Ponownie uruchom wdrożenie, nie zamiast tego zaloguj się jako azurestack\AzureStackAdmin można sprawdzić, czy jest uruchomiona.
   

## <a name="deploy-the-asdk-using-powershell"></a>Wdrażanie ASDK przy użyciu programu PowerShell
Poprzednie kroki udał kroków wdrażania ASDK przy użyciu środowiska użytkownika z przewodnikiem. Za pomocą programu PowerShell można również wdrożyć ASDK na hoście development kit, wykonując kroki opisane w tej sekcji.

### <a name="configure-the-asdk-host-computer-to-boot-from-cloudbuildervhdx"></a>Skonfiguruj komputer-host ASDK rozruchu z CloudBuilder.vhdx
Te polecenia skonfiguruje komputer-host ASDK rozruchu z pobrane i wyodrębnione stosu Azure wirtualnego dysku twardego (CloudBuilder.vhdx). Po wykonaniu tych kroków, uruchom ponownie komputer-host ASDK.

1. Uruchom wiersz polecenia jako administrator.
2. Uruchom polecenie `bcdedit /copy {current} /d "Azure Stack"`
3. Kopiowania (CTRL + C) wartość identyfikatora CLSID zwrócone, łącznie z wymagane {} "s. Ta wartość jest określana jako {CLSID} i należy wkleić (CTRL + V lub kliknij prawym przyciskiem myszy) w pozostałych kroków.
4. Uruchom polecenie `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` 
5. Uruchom polecenie `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` 
6. Uruchom polecenie `bcdedit /set {CLSID} detecthal on` 
7. Uruchom polecenie `bcdedit /default {CLSID}`
8. Aby sprawdzić ustawienia rozruchu, uruchom `bcdedit`. 
9. Upewnij się, że CloudBuilder.vhdx plik został przeniesiony do katalogu głównego dysku C:\ (C:\CloudBuilder.vhdx) i uruchom ponownie na komputerze deweloperskim zestaw hosta. Po ponownym uruchomieniu hosta ASDK powinny teraz domyślnie uruchamianie z CloudBuilder.vhdx maszyny Wirtualnej. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>Przygotowanie hosta zestawu Programowanie przy użyciu programu PowerShell 
Po development kit hosta pomyślnie uruchomił obraz CloudBuilder.vhdx, można otworzyć konsoli programu PowerShell z podwyższonym poziomem uprawnień i Uruchom polecenia w tej sekcji, aby wdrożyć ASDK na hoście development kit.

> [!IMPORTANT] 
> Instalacja ASDK obsługuje dokładnie jednej karty interfejsu sieciowego (NIC) do sieci. Jeśli masz wiele kart sieciowych, upewnij się, że tylko jeden z nich jest włączona (i wszystkich innych są wyłączone) przed uruchomieniem skryptu wdrażania.

Stos Azure z usługi Azure AD lub AD FS można wdrożyć jako dostawcy tożsamości. Azure stosu, dostawców zasobów i inne aplikacje działają tak samo zarówno. Aby dowiedzieć się więcej na temat co to jest obsługiwana z usługami AD FS w stosie usługi Azure, zobacz [klucza pojęć i funkcji](.\azure-stack-key-features.md) artykułu.

> [!TIP]
> Jeśli nie podawaj parametrów instalacji (zobacz InstallAzureStackPOC.ps1 następujące parametry opcjonalne i przykłady poniżej), zostanie wyświetlony monit dla wymaganych parametrów.

### <a name="deploy-azure-stack-using-azure-ad"></a>Wdrażanie stos Azure za pomocą usługi Azure AD 
Aby wdrożyć stosu Azure **przy użyciu usługi Azure AD jako dostawcy tożsamości**, musi mieć połączenie z Internetem bezpośrednio lub za pośrednictwem przezroczystego obiektu pośredniczącego. Uruchom następujące polecenia programu PowerShell, aby wdrożyć zestaw deweloperski, za pomocą usługi Azure AD:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

  Kilka minut w ASDK instalacji zostanie wyświetlony monit o poświadczenia usługi Azure AD. Musisz podać poświadczenia administratora globalnego dla dzierżawy usługi Azure AD. 

### <a name="deploy-azure-stack-using-ad-fs"></a>Wdrażanie stos Azure za pomocą usług AD FS 
Aby wdrożyć zestaw deweloperski **za pomocą usług AD FS jako dostawca tożsamości**, uruchom następujące polecenia środowiska PowerShell (wystarczy dodać parametr - UseADFS): 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

W przypadku wdrożeń usług AD FS sygnatury domyślna usługa katalogowa jest używany jako dostawca tożsamości. Zaloguj się przy użyciu domyślnego konta jest azurestackadmin@azurestack.local, i hasło zostaną ustawione na podany jako część polecenia Instalatora programu PowerShell.

Proces wdrażania może potrwać kilka godzin, w tym czasie automatycznie ponownego raz. Gdy wdrożenie zakończy się powodzeniem, konsoli programu PowerShell Wyświetla: **COMPLETE: Akcja "Wdrożenie"**. Jeśli wdrożenie nie powiedzie się, można spróbować uruchomić ponownie przy użyciu skryptu parametr ponownego uruchamiania. Można też [ponownie wdrożyć ASDK](.\azure-stack-redeploy.md) od początku.
> [!IMPORTANT]
> Jeśli chcesz monitorować postęp wdrażania, po ponownym uruchomieniu hosta ASDK, musisz się zalogować jako AzureStack\AzureStackAdmin. Jeśli zarejestrujesz się jako administrator lokalny po komputer-host ponownego uruchomienia (i przyłączone do domeny azurestack.local), nie będzie wyświetlany jest postęp wdrażania. Ponownie uruchom wdrożenie, nie zamiast tego zaloguj się jako azurestack można sprawdzić, czy jest uruchomiona.
>

#### <a name="azure-ad-deployment-script-examples"></a>Przykłady skryptów wdrażania w usłudze Azure AD
Można utworzyć skrypty całego wdrożenia usługi Azure AD. Oto kilka przykładów komentarze, obejmujące niektóre parametry opcjonalne.

Jeśli tożsamości usługi Azure AD tylko jest skojarzony z **jeden** katalog usługi Azure AD:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Jeśli tożsamości usługi Azure AD jest skojarzony z **więcej niż jeden** katalog usługi Azure AD:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<specific Azure AD directory in the form of domainname.onmicrosoft.com>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Jeśli środowisko **nie** ma DHCP włączone, należy uwzględnić następujące dodatkowe parametry do jednej z opcji powyżej (przykład użycia podano): 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1 -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>Parametry opcjonalne ASDK InstallAzureStackPOC.ps1
|Parametr|Wymagane opcjonalne|Opis|
|-----|-----|-----|
|AdminPassword|Wymagane|Ustawia konta administratora lokalnego i innymi kontami użytkowników na wszystkie maszyny wirtualne utworzone jako część development kit wdrożenia. To hasło musi być zgodna bieżące hasło administratora lokalnego na hoście.|
|InfraAzureDirectoryTenantName|Wymagane|Ustawia katalog dzierżawy. Ten parametr umożliwia określenie określonego katalogu gdzie konta usługi AAD ma uprawnienia do zarządzania wielu katalogów. Pełna nazwa dzierżawy katalogu usługi AAD w formacie. onmicrosoft.com.|
|TimeServer|Wymagane|Ten parametr umożliwia określenie serwera określony czas. Ten parametr musi być dostarczona jako adres IP serwera czas ważności. Nazwy serwerów nie są obsługiwane.|
|InfraAzureDirectoryTenantAdminCredential|Optional (Opcjonalność)|Ustawia nazwę użytkownika usługi Azure Active Directory i hasło. Te poświadczenia platformy Azure musi być identyfikatorem organizacji.|
|InfraAzureEnvironment|Optional (Opcjonalność)|Wybierz środowisko Azure, z którym chcesz zarejestrować tego wdrożenia stosu Azure. Obejmują one publicznej Azure, Azure - Chinach, Azure - instytucji rządowych Stanów Zjednoczonych.|
|DNSForwarder|Optional (Opcjonalność)|Serwer DNS jest tworzony jako część wdrożenia stosu Azure. Aby umożliwić komputerom wewnątrz rozwiązania do rozpoznawania nazw poza sygnatury, podaj istniejącej infrastruktury serwera DNS. Serwer DNS w sygnatury przekazuje żądania rozpoznania nieznanej nazwy do tego serwera.|
|NatIPv4Address|Wymagane do obsługi protokołu DHCP translatora adresów Sieciowych|Ustawia MAS BGPNAT01 statycznego adresu IP. Tego parametru należy używać tylko wówczas, gdy usługa DHCP nie może przypisać prawidłowego adresu IP w celu uzyskania dostępu do Internetu.|
|NatIPv4Subnet|Wymagane do obsługi protokołu DHCP translatora adresów Sieciowych|Prefiks podsieci IP używanego w DHCP za pośrednictwem obsługi translatora adresów Sieciowych. Tego parametru należy używać tylko wówczas, gdy usługa DHCP nie może przypisać prawidłowego adresu IP w celu uzyskania dostępu do Internetu.|
|PublicVlanId|Optional (Opcjonalność)|Ustawia identyfikator sieci VLAN. Tego parametru należy użyć tylko, jeśli host i MAS BGPNAT01 należy skonfigurować identyfikator sieci VLAN umożliwiający dostęp do sieci fizycznej (i Internetu). Na przykład.\InstallAzureStackPOC.ps1-Verbose - PublicVLan 305|
|Uruchom ponownie|Optional (Opcjonalność)|Użyj tej flagi, aby ponownie uruchomić wdrażania. Wszystkie poprzednie dane wejściowe są używane. Ponownego wprowadzania danych podać wcześniej nie jest obsługiwana, ponieważ wiele unikatowych wartości były generowane i użyć do wdrożenia.|

## <a name="activate-the-administrator-and-tenant-portals"></a>Aktywować administratora i dzierżawcy portali
Po wdrożeń, które używają usługi Azure AD musisz aktywować zarówno stosu Azure administratora i dzierżawcy portali. Aktywacja zgadza się na zapewnieniu stosu Azure portalu i usługi Azure Resource Manager odpowiednie uprawnienia (wyświetlane na stronie zgoda) dla wszystkich użytkowników z katalogu.

- Portalu administratora, przejdź do https://adminportal.local.azurestack.external/guest/signup, przeczytaj informacje, a następnie kliknij przycisk **Akceptuj**. Po zaakceptowaniu, możesz dodać Administratorzy usług, którzy nie są również administratorami dzierżawy katalogu.

- Portalu dzierżawcy, przejdź do https://portal.local.azurestack.external/guest/signup, przeczytaj informacje, a następnie kliknij przycisk **Akceptuj**. Po zaakceptowaniu, w tym katalogu może logowania użytkownika do portalu dzierżawcy. 

> [!NOTE] 
> Jeśli nie są uaktywnione portali, tylko administrator katalogu można zalogować i korzystać portali. Jeśli inny użytkownik się zaloguje, zostanie wyświetlony błąd, który informuje, czy administrator nie ma przyznane uprawnienia do innych użytkowników. Administrator nie natywnie należą do katalogu, który stosu Azure jest zarejestrowany, katalog Azure stos musi dołączany do adresu URL aktywacji. Na przykład jeśli stosu Azure jest zarejestrowany do fabrikam.onmicrosoft.com i użytkownika administratora jest admin@contoso.com, przejdź do https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com aktywować portalu. 

## <a name="reset-the-password-expiration-policy"></a>Resetuj zasady wygasania haseł 
Po wdrożeniu ASDK, aby upewnić się, że hasło dla hosta development kit nie wygasa przed zakończeniem okresu oceny, wykonaj następujące kroki.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Aby zmienić zasady wygasania haseł z programu Powershell:
W konsoli programu Powershell z podwyższonym poziomem uprawnień uruchom polecenie:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Aby ręcznie zmienić zasady wygasania haseł:
1. Na hoście development kit Otwórz **Zarządzanie zasadami grupy** i przejdź do **Zarządzanie zasadami grupy** — **lasu: azurestack.local** — **domen** — **azurestack.local**.
2. Kliknij prawym przyciskiem myszy **domyślne zasady domeny** i kliknij przycisk **Edytuj**.
3. W edytorze zasad grupy zarządzania, przejdź do **Konfiguracja komputera** — **zasady** — **ustawienia systemu Windows** — **ustawienia zabezpieczeń**— **Zasady konta** — **zasady haseł**.
4. W prawym okienku kliknij dwukrotnie **maksymalny okres ważności hasła**.
5. W **maksymalny okres ważności hasła właściwości** okno dialogowe, zmień **hasło wygaśnie za** do 180 wartości, a następnie kliknij przycisk **OK**.


## <a name="next-steps"></a>Następne kroki

[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)

[Instalator programu PowerShell na potrzeby środowisk Azure stosu](azure-stack-powershell-configure-quickstart.md)

[Zarejestruj stosu Azure z subskrypcją platformy Azure](azure-stack-register.md)



