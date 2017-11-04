---
title: "Wdrażanie Azure stosu Development Kit | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przygotować Azure stosu Development Kit i uruchom skrypt programu PowerShell do wdrożenia."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/17/2017
ms.author: erikje
ms.openlocfilehash: b67cabf0ecdb48f137bfcfbce95eee568a1c298d
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Wdrażanie Azure stosu Development Kit

*Dotyczy: Azure stosu Development Kit*

Aby wdrożyć [Azure stosu Development Kit](azure-stack-poc.md), należy wykonać następujące czynności:

1. [Pobierz pakiet wdrożeniowy](https://azure.microsoft.com/overview/azure-stack/try/?v=try) uzyskanie Cloudbuilder.vhdx.
2. [Przygotowanie cloudbuilder.vhdx](#prepare-the-development-kit-host) przez uruchomienie skryptu asdk installer.ps1 do skonfigurowania komputera (Programowanie hosta kit), na którym chcesz zainstalować zestawu SDK. Po wykonaniu tego kroku hosta zestawu programowanie uruchomi się do Cloudbuilder.vhdx.
3. [Wdrażanie zestaw deweloperski](#deploy-the-development-kit) na hoście development kit.

> [!NOTE]
> Aby uzyskać najlepsze wyniki nawet jeśli ma być używany w środowisku bez połączenia stosu Azure najlepiej do wdrożenia podczas połączenia z Internetem. W ten sposób można aktywować wersję ewaluacyjną systemu Windows Server 2016 w czasie wdrażania.
> 
> 

## <a name="download-and-extract-the-development-kit"></a>Pobierać i wyodrębniać z zestawem deweloperskim
1. Aby rozpocząć pobieranie, upewnij się, że komputer spełnia następujące wymagania wstępne:

   * Komputer musi mieć co najmniej 60 GB wolnego miejsca na dysku.
   * [.NET framework 4.6 (lub nowszym)](https://aka.ms/r6mkiy) musi być zainstalowany.

2. [Przejdź do strony wprowadzenie](https://azure.microsoft.com/overview/azure-stack/try/?v=try), należy podać swoje szczegóły dotyczące i kliknąć **przesyłania**.
3. W obszarze **Pobierz oprogramowanie**, kliknij przycisk **Azure stosu Development Kit**.
4. Uruchom pobrany plik AzureStackDownloader.exe.
5. W **Azure stosu Development Kit Downloader** okna, wykonaj kroki z zakresu od 1 do 5.
6. Po zakończeniu pobierania kliknij **Uruchom** można uruchomić MicrosoftAzureStackPOC.exe.
7. Przejrzyj umowę licencyjną ekranu i informacji o Kreatorze samorozpakowujący się plik typu, a następnie kliknij przycisk **dalej**.
8. Przejrzyj zasady zachowania poufności informacji ekranu i informacji o Kreatorze samorozpakowujący się plik typu, a następnie kliknij przycisk **dalej**.
9. Wybierz lokalizację docelową dla plików do wyodrębnienia, kliknij przycisk **dalej**.
   * Wartość domyślna to: <drive letter>:\<bieżący folder > \Microsoft Azure stosu
10. Przegląd docelowej lokalizacji ekranu i informacji o Kreatorze samorozpakowujący się plik typu, a następnie kliknij przycisk **wyodrębnić** wyodrębnić CloudBuilder.vhdx (~ 25 GB) i pliki ThirdPartyLicenses.rtf. Ten proces trwa trochę czasu, aby zakończyć.

> [!NOTE]
> Po wyodrębnieniu plików można usunąć plików exe i bin, aby odzyskać miejsce na tym komputerze. Alternatywnie można przenieść te pliki do innej lokalizacji, więc jeśli należy ponownie wdrożyć możesz nie trzeba ponownie pobrać pliki.
> 
> 

## <a name="prepare-the-development-kit-host"></a>Przygotowanie hosta zestawu programowanie
1. Upewnij się, że można fizycznie połączyć się z hostem zestawu rozwoju lub mają dostęp fizyczny konsoli (na przykład KVM). Musi mieć dostęp do takich po ponownym uruchomieniu hosta zestawu Programowanie w kroku 13 poniżej.
2. Upewnij się, że spełnia hosta zestawu programowanie [minimalne wymagania](azure-stack-deploy.md). Można użyć [wdrożenia sprawdzanie stosu Azure](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) o potwierdzenie wymagań.
3. Zaloguj się jako Administrator lokalny do hosta development kit.
4. Skopiować lub przenieść pliku CloudBuilder.vhdx w katalogu głównym dysku C:\ (C:\CloudBuilder.vhdx).
5. Uruchom następujący skrypt, aby pobrać plik Instalatora development kit (asdk installer.ps1) do folderu c:\AzureStack_Installer na hoście development kit.
    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
    $LocalPath = 'c:\AzureStack_Installer'

    # Create folder
    New-Item $LocalPath -Type directory

    # Download file
    Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
    ```
6. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień > Uruchom skrypt C:\AzureStack_Installer\asdk-installer.ps1 > kliknij **przygotować środowisko**.
7. Na **vhdx wybierz Cloudbuilder** strony Instalatora, Znajdź i wybierz plik cloudbuilder.vhdx pobranego w poprzedniej procedurze.
8. Opcjonalnie: Sprawdź **dodać sterowniki** pole, aby określić folder zawierający dodatkowe sterowniki, które mają na hoście.
9. Na **opcjonalne ustawienia** Podaj konto administratora lokalnego dla hosta development kit. Jeśli nie podasz tych poświadczeń, konieczne będzie KVM dostęp do hosta podczas procesu instalacji poniżej.
10. Ponadto na **opcjonalne ustawienia** strony, masz opcję, aby określić następujące ustawienia:
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

## <a name="deploy-the-development-kit"></a>Wdrażanie z zestawem deweloperskim
1. Zaloguj się jako Administrator lokalny do rozwoju zestawu hosta. Użyj poświadczeń określonych w poprzednich krokach.

    > [!IMPORTANT]
    > W przypadku wdrożeń usługi Azure Active Directory stosu Azure wymaga dostępu do Internetu, bezpośrednio lub za pośrednictwem przezroczystego obiektu pośredniczącego. Wdrożenie obsługuje dokładnie jedną kartę Sieciową do sieci. Jeśli masz wiele kart sieciowych, upewnij się, że tylko jeden z nich jest włączona (i wszystkie inne są wyłączone) przed uruchomieniem skryptu wdrażania w następnej sekcji.
    
2. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień > Uruchom skrypt \AzureStack_Installer\asdk-installer.ps1 (która może znajdować się na inny dysk w Cloudbuilder.vhdx) > kliknij **zainstalować**.
3. W **typu** wybierz opcję **chmury Azure** lub **usług AD FS**.
    - **Chmury Azure**: Azure Active Directory jest to dostawca tożsamości. Ten parametr umożliwia określenie określonego katalogu gdzie konta usługi AAD ma uprawnienia administratora globalnego. Pełna nazwa dzierżawy katalogu usługi AAD. Na przykład. onmicrosoft.com. 
    - **Usługi AD FS**: sygnatura domyślna usługa katalogowa jest dostawcy tożsamości, zaloguj się przy użyciu domyślnego konta azurestackadmin@azurestack.local, i hasło używane jest dostarczana jako część instalacji.
4. W obszarze **hasło administratora lokalnego**w **hasło** wpisz hasło administratora lokalnego (która musi odpowiadać bieżące hasło administratora lokalnego skonfigurowany), a następnie kliknij przycisk **Dalej**.
5. Wybierz kartę sieciową do użycia dla zestawu SDK, a następnie kliknij przycisk **dalej**.
6. Wybierz DHCP lub konfiguracji statycznej sieci dla maszyny wirtualnej BGPNAT01.
    - **DHCP** (domyślnie): maszyny wirtualnej pobiera konfigurację sieci IP od serwera DHCP.
    - **Statyczne**: tej opcji należy używać tylko wtedy, gdy DHCP nie można przypisać prawidłowy adres IP dla stosu Azure na dostęp do Internetu. Należy określić statyczny adres IP o długości maska podsieci (na przykład 10.0.0.5/24).
7. Opcjonalnie ustaw następujące wartości:
    - **Identyfikator sieci VLAN**: Określa identyfikator sieci VLAN. Tej opcji należy używać tylko wtedy, gdy host i AzS BGPNAT01 należy skonfigurować identyfikator sieci VLAN umożliwiający dostęp do sieci fizycznej (i Internetu). 
    - **Usługa przesyłania dalej DNS**: serwer DNS jest tworzony jako część wdrożenia stosu Azure. Aby umożliwić komputerom wewnątrz rozwiązania do rozpoznawania nazw poza sygnatury, podaj istniejącej infrastruktury serwera DNS. Serwer DNS w sygnatury przekazuje żądania rozpoznania nieznanej nazwy do tego serwera.
    - **Serwer czasu**: to wymagane pole ustawia czas serwera i musi zawierać adres IP. Aby znaleźć adres IP serwera czasu, odwiedź stronę [pool.ntp.org](http:\\pool.ntp.org) lub polecenie ping time.windows.com. 
8. Kliknij przycisk **Dalej**. 
9. Na **Weryfikowanie właściwości karty interfejsu sieciowego** strony, zostanie wyświetlony pasek postępu. 
    - Informacja **nie można pobrać aktualizacji**, postępuj zgodnie z instrukcjami na stronie.
    - Gdy mówi **Ukończono**, kliknij przycisk **dalej**.
10. Na **Podsumowanie** kliknij przycisk **Wdróż**.
11. Jeśli używasz wdrożenia usługi Azure Active Directory, zostanie wyświetlony monit o podanie poświadczeń konta administratora globalnego usługi Azure Active Directory.
12. Proces wdrażania może potrwać kilka godzin, w których automatycznie ponownego raz.
   
   > [!IMPORTANT]
   > Jeśli chcesz monitorować postęp wdrażania, zaloguj się jako azurestack\AzureStackAdmin. Jeśli zarejestrujesz się jako administrator lokalny po komputer jest przyłączony do domeny, nie będzie wyświetlany jest postęp wdrażania. Ponownie uruchom wdrożenie, nie zamiast tego zaloguj się jako azurestack\AzureStackAdmin można sprawdzić, czy jest uruchomiona.
   > 
   > 
   
    Gdy wdrożenie zakończy się powodzeniem, konsoli programu PowerShell Wyświetla: **COMPLETE: Akcja "Wdrożenie"**.
   
Jeśli wdrożenie nie powiedzie się, można następujący skrypt programu PowerShell, uruchom ponownie z tym samym oknie programu PowerShell z podwyższonym poziomem uprawnień:

```powershell
cd c:\CloudDeployment\Setup
.\InstallAzureStackPOC.ps1 -Rerun
```

Ten skrypt zostanie uruchomiony ponownie wdrożenie w ostatnim kroku, które zakończyło się pomyślnie.

Można też [ponownie wdrożyć](azure-stack-redeploy.md) od początku.


## <a name="reset-the-password-expiration-to-180-days"></a>Resetuj na wartość 180 dni wygaśnięcia hasła

Po wdrożeniu, aby upewnić się, że hasło dla hosta development kit nie wygasa zbyt wcześnie, wykonaj następujące kroki:

Aby zmienić zasady wygasania haseł z programu Powershell:
1. W oknie programu Powershell, uruchom polecenie. Zestaw ADDefaultDomainPasswordPolicy - MaxPasswordAge 180.00:00:00-azurestack.local tożsamości

Aby ręcznie zmienić zasady wygasania haseł:
1. Na hoście development kit Otwórz **Zarządzanie zasadami grupy** i przejdź do **Zarządzanie zasadami grupy** — **lasu: azurestack.local** — **domen** — **azurestack.local**.
2. Kliknij prawym przyciskiem myszy **domyślne zasady domeny** i kliknij przycisk **Edytuj**.
3. W edytorze zasad grupy zarządzania, przejdź do **Konfiguracja komputera** — **zasady** — **ustawienia systemu Windows** — **ustawienia zabezpieczeń**— **Zasady konta** — **zasady haseł**.
4. W prawym okienku kliknij dwukrotnie **maksymalny okres ważności hasła**.
5. W **maksymalny okres ważności hasła właściwości** okno dialogowe, zmień **hasło wygaśnie za** do 180 wartości, a następnie kliknij przycisk **OK**.


## <a name="next-steps"></a>Następne kroki

[Instalowanie programu PowerShell](azure-stack-powershell-configure-quickstart.md)

[Zarejestruj stosu Azure z subskrypcją platformy Azure](azure-stack-register.md)

[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)

