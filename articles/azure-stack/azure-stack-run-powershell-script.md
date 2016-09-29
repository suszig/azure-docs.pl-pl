<properties
    pageTitle="Wdrażanie usługi Azure Stack POC | Microsoft Azure"
    description="Dowiedz się, jak przygotować usługę Azure Stack POC i uruchomić skrypt programu PowerShell w celu wdrożenia usługi Azure Stack POC."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/29/2016"
    ms.author="erikje"/>


# Wdrażanie usługi Azure Stack POC
Aby wdrożyć usługę Azure Stack POC, należy najpierw [przygotować komputer do wdrożenia](#prepare-the-deployment-machine), a następnie [uruchomić skrypt wdrażania programu PowerShell](#run-the-powershell-deployment-script).

## Przygotowanie komputera do wdrożenia

1. Upewnij się, że można fizycznie połączyć się z komputerem do wdrożenia lub uzyskać dostęp do konsoli fizycznej (na przykład KVM). Dostęp tego typu będzie potrzebny po ponownym rozruchu komputera do wdrożenia w kroku 9.
 
2. Upewnij się, że komputer do wdrożenia spełnia [minimalne wymagania](azure-stack-deploy.md). Korzystając z [Narzędzia do sprawdzania wdrożenia usługi Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1), można potwierdzić zgodność z wymaganiami.

3.  [Pobierz](http://aka.ms/ReqOSforAzureStack) i zainstaluj system Windows Server 2016 Datacenter Edition Technical Preview 4 EN-US (pełna wersja).

4.  [Pobierz](https://azure.microsoft.com/overview/azure-stack/try/?v=try) pakiet wdrożeniowy usługi Azure Stack POC do folderu na dysku C (na przykład c:\\AzureStack).

5.  Uruchom plik **Microsoft Azure Stack POC.exe**.

    Powoduje to utworzenie folderu \\Microsoft Azure Stack POC\\ zawierającego następujące elementy:

    -   DeployAzureStack.ps1: skrypt programu PowerShell do instalacji usługi Azure Stack POC

    -   MicrosoftAzureStackPOC.vhdx: pakiet danych usługi Azure Stack

    -   SQLServer2014.vhdx: plik VHD programu SQL Server

    -   WindowsServer2012R2DatacenterEval.vhd

    -   WindowsServer2016Datacenter.vhdx: plik VHD systemu Windows Server 2016 Datacenter (zawiera KB 3124262)

    > [AZURE.IMPORTANT] Musisz mieć co najmniej 128 GB wolnego miejsca na fizycznym woluminie rozruchowym.

6. Skopiuj plik WindowsServer2016Datacenter.vhdx na dysk C:\ i zmień jego nazwę na MicrosoftAzureStackPOCBoot.vhdx.

7. W Eksploratorze plików kliknij prawym przyciskiem myszy plik MicrosoftAzureStackPOCBoot.vhdx, a następnie kliknij pozycję **Zainstaluj**.

8. Otwórz wiersz polecenia jako administrator i uruchom poniższe polecenie bcdboot. To polecenie tworzy środowisko rozruchowe jednego z kilku systemów operacyjnych. Od tego punktu należy wykonywać rozruch przy użyciu górnej opcji rozruchu.

        bcdboot <mounted drive letter>:\windows

9. Uruchom ponownie komputer. Spowoduje to automatyczne uruchomienie Instalatora systemu Windows podczas przygotowywania systemu VHD. Po tym punkcie fizyczne połączenie z komputerem do wdrożenia lub uzyskanie dostępu do konsoli fizycznej będzie konieczne do wykonania następujących kroków.

10. Jeśli system BIOS zawiera odpowiednią opcję, należy skonfigurować go do użycia czasu lokalnego zamiast czasu UTC.

11. Gdy zostanie wyświetlony odpowiedni monit, określ kraj, język, klawiaturę i inne preferencje. Jeśli zostanie wyświetlony monit o podanie klucza produktu, możesz go znaleźć w artykule [Wymagania systemowe i instalacja](https://technet.microsoft.com/library/mt126134.aspx).

12. Gdy zostanie wyświetlony odpowiedni monit, ustaw hasło administratora, a następnie zaloguj się przy użyciu tej nazwy konta i hasła.

13. Jeśli po ponownym uruchomieniu i zalogowaniu się nie masz obsługi protokołu DHCP, skonfiguruj statycznie kartę sieciową.

14. Włącz obsługę połączeń pulpitu zdalnego, przechodząc do okna **Właściwości systemu** i wybierając opcję **Zezwalaj na połączenia zdalne z tym komputerem**.

15. Zaloguj się przy użyciu konta lokalnego z uprawnieniami administratora.

16. Sprawdź, czy **dokładnie** cztery dyski dla usługi Azure Stack POC:
  - są widoczne w oknie zarządzania dyskami,
  - nie są używane,
  - są wyświetlane jako Online, Nieprzydzielone.

17. Zweryfikuj, że host nie jest przyłączony do domeny.

18. Korzystając z programu Internet Explorer, zweryfikuj łączność sieciową z witryną Azure.com.

> [AZURE.IMPORTANT] Wdrożenie usługi TP1 POC obsługuje dokładnie cztery dyski funkcji magazynu i tylko jedną kartę sieciową dla sieci.
>
> - **W przypadku magazynu** wyłącz wszystkie inne dyski przy użyciu menedżera urządzeń lub usługi WMI (nie wystarczy przełączyć dysków do trybu offline za pomocą menedżera dysków).
>
> - **W przypadku sieci**, jeśli masz wiele kart sieciowych, upewnij się, że tylko jedna z nich jest włączona (a wszystkie pozostałe karty są wyłączone) przed uruchomieniem poniższego skryptu wdrażania.
>
> Jeśli używasz opisanych powyżej kroków rozruchu systemu VHD, konieczne będzie zainstalowanie tych aktualizacji po rozruchu w systemie VHD i przed uruchomieniem skryptu wdrażania.

## Uruchamianie skryptu programu PowerShell

1. Przenieś następujące pliki wdrożenia z folderu D:\Microsoft Azure Stack POC\ do folderu C:\Microsoft Azure Stack POC.\.
    - DeployAzureStack.ps1
    - MicrosoftAzureStackPOC.vhdx
    - SQLServer14.vhdx
    - WindowsServer2012R2DatacenterEval.vhd
    - WindowsServer2016Datacenter.vhdx

2.  Otwórz program PowerShell jako administrator.

3.  W programie PowerShell przejdź do lokalizacji folderu usługi Azure Stack (\\Microsoft Azure Stack POC\\, jeśli użyto ustawienia domyślnego).

4.  Uruchom polecenie wdrożenia:

        .\DeployAzureStack.ps1 –Verbose

    W Chinach użyj następującego polecenia:

        .\DeployAzureStack.ps1 –Verbose -UseAADChina $true

    Wdrożenie zostanie rozpoczęte, a nazwa domeny usługi Azure Stack POC zostanie umieszczona w kodzie jako azurestack.local.

5.  Gdy zostanie wyświetlony monit **Wprowadź hasło dla wbudowanego konta administratora**, wprowadź hasło, a następnie potwierdź je. Jest to hasło dla wszystkich maszyn wirtualnych. Koniecznie zapisz to hasło administratora usługi.

6.  Na stronie **Zaloguj się do konta platformy Azure na wyskakującej stronie uwierzytelniania Azure** naciśnij dowolny klawisz, aby otworzyć okno dialogowe logowania do platformy Microsoft Azure.

7.  Wprowadź poświadczenia dla swojego konta usługi Azure Active Directory. Ten użytkownik musi być administratorem globalnym w dzierżawie katalogu.

8.  Wróć do programu PowerShell i na stronie monitu o potwierdzenie wyboru konta wprowadź *y*. Spowoduje to utworzenie dwóch użytkowników i trzech aplikacji dla usługi Azure Stack w tej dzierżawie katalogu: administratora usługi Azure Stack, użytkownika dzierżawy dla testów TiP (Testowanie w produkcji) i po jednej aplikacji dla portalu, interfejsu API i dostawców zasobów monitorowania. Oprócz tego instalator dodaje zgody dla programu Azure PowerShell, interfejsu wiersza polecenia XPlat i programu Visual Studio do tej dzierżawy katalogu.

9.  Na stronie monitu **Usługa Microsoft Azure Stack POC jest gotowa do wdrożenia. Kontynuować?** wprowadź *y*.

10.  Proces wdrażania potrwa kilka godzin. W tym czasie system będzie automatycznie ponownie uruchamiany kilka razy. Logowanie podczas wdrażania będzie powodować automatyczne uruchomienie okna programu PowerShell, w którym będzie wyświetlany postęp wdrożenia. Okno programu PowerShell jest zamykane po ukończeniu wdrażania.

11. Na komputerze usługi Azure Stack POC zaloguj się jako użytkownik AzureStack\administrator, otwórz **Menedżera serwera**, a następnie wyłącz opcję **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer** zarówno dla administratorów, jak i dla użytkowników.

Jeśli wdrażanie nie powiedzie się i zostanie zgłoszony błąd godziny lub daty, skonfiguruj system BIOS do użycia czasu lokalnego zamiast czasu UTC. Następnie ponów wdrożenie.

Jeśli wykonanie skryptu nie powiedzie się, uruchom skrypt ponownie. Jeśli wykonanie skryptu ponownie nie powiedzie się, wyczyść dane i uruchom ponownie.

Dzienniki skryptów można znaleźć w folderze `C:\ProgramData\microsoft\azurestack` usługi POC.

### Opcjonalne parametry DeployAzureStack.ps1

**AADCredential** (PSCredential) — określa nazwę użytkownika i hasło usługi Azure Active Directory. Jeśli ten parametr nie zostanie podany, skrypt monituje o podanie nazwy użytkownika i hasła.

**AADTenant** (ciąg) — określa katalog dzierżawy. Ten parametr umożliwia określenie katalogu, gdy konto usługi AAD ma uprawnienia do zarządzania wieloma katalogami. Jeśli ten parametr nie zostanie podany, skrypt monituje o podanie katalogu.

**AdminPassword** (SecureString) — określa domyślne hasło administratora. Jeśli ten parametr nie zostanie podany, skrypt monituje o podanie hasła.

**Force** (przełącznik) — określa polecenie cmdlet do uruchomienia bez potwierdzeń.

**NATVMStaticGateway** (ciąg) — określa domyślną bramę używaną w statycznym adresie IP dla maszyny wirtualnej NATVM. Tego parametru należy używać tylko wówczas, gdy usługa DHCP nie może przypisać prawidłowego adresu IP w celu uzyskania dostępu do Internetu. Jeśli ten parametr jest używany, należy również użyć parametru NATVMStaticIP.
Na przykład: `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24 – NATVMStaticGateway 10.10.10.1`

**NATVMStaticIP** (ciąg) — określa dodatkowy statyczny adres IP dla maszyny wirtualnej NATVM. Tego parametru należy używać tylko wówczas, gdy usługa DHCP nie może przypisać prawidłowego adresu IP w celu uzyskania dostępu do Internetu.
Na przykład: `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24`

**NoAutoReboot** (przełącznik) — określa skrypt do uruchomienia bez automatycznego ponownego uruchamiania.

**ProxyServer** (ciąg) — określa informacje dotyczące serwera proxy. Tego parametru należy używać tylko wówczas, gdy środowisko musi używać serwera proxy w celu uzyskania dostępu do Internetu. Serwery proxy wymagające poświadczeń nie są obsługiwane.
Na przykład: `.\DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080`

**PublicVLan** (ciąg) — określa identyfikator sieci VLAN. Tego parametru należy używać tylko wówczas, gdy host i maszyna wirtualna NATVM muszą skonfigurować identyfikator sieci VLAN w celu uzyskania dostępu do sieci fizycznej (i Internetu).
Na przykład: `.\DeployAzureStack.ps1 –Verbose –PublicVLan 305`

**TIPServiceAdminCredential** (PSCredential) — określa poświadczenia istniejącego administratora usługi dla konta usługi Azure Active Directory. To konto jest używane przez test TiP (Testowanie w produkcji). Jeśli ten parametr nie zostanie podany, konto jest tworzone automatycznie.

**TIPTenantAdminCredential** (PSCredential) — określa poświadczenia istniejącego administratora dzierżawy dla konta usługi Azure Active Directory. To konto jest używane przez test TiP (Testowanie w produkcji). Jeśli ten parametr nie zostanie podany, konto jest tworzone automatycznie.

**UseAADChina** (wartość logiczna) — ustaw ten parametr logiczny z wartością $true, jeśli chcesz wdrożyć usługę Microsoft Azure Stack POC z chińską wersją platformy Azure (Mooncake).

## Wyłącz automatyczne testy TiP (opcjonalnie)

Usługa Microsoft Azure Stack Technical Preview 1 zawiera zestaw testów weryfikacyjnych używanych podczas procesu wdrażania i zgodnie z cyklicznym harmonogramem codziennym. Symulują one akcje wykonywane przez dzierżawcę usługi Azure Stack, a konta użytkowników Test-in-POC (TiP) są tworzone w usłudze Azure Active Directory w celu uruchamiania testów. Po pomyślnym wdrożeniu można wyłączyć te testy TiP. 

**Aby wyłączyć automatyczne testy TiP**

  - Na klienckiej maszynie wirtualnej uruchom następujące polecenie cmdlet:

  `Disable-ScheduledTask -TaskName AzureStackSystemvalidationTask`

**Aby wyświetlić wyniki testów**

  - Na klienckiej maszynie wirtualnej uruchom następujące polecenie cmdlet:

  `Get-AzureStackTiPTestsResult`



## Wyłączanie obsługi telemetrii dla usługi Microsoft Azure Stack POC (opcjonalnie)


Przed wdrożeniem usługi Microsoft Azure Stack POC można wyłączyć obsługę telemetrii dla usługi Microsoft Azure Stack na komputerze, z którego wdrożenie jest wykonywane. Aby wyłączyć tę funkcję na jednym komputerze, skorzystaj z informacji na stronie [http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq) i zmień wartość ustawienia **Dane diagnostyczne i dane użycia** na **Podstawowe**.



Po wdrożeniu usługi Microsoft Azure Stack POC można wyłączyć obsługę telemetrii na wszystkich maszynach wirtualnych przyłączonych do domeny usługi Azure Stack. Aby utworzyć zasady grupy i zarządzać ustawieniami telemetrii na tych maszynach wirtualnych, skorzystaj z informacji na stronie [https://technet.microsoft.com/library/mt577208(v=vs.85).aspx\#BKMK\_UTC](https://technet.microsoft.com/library/mt577208%28v=vs.85%29.aspx#BKMK_UTC) i wybierz **0** lub **1** dla zasad grupy **Zezwalaj na telemetrię**. Istnieją dwie maszyny wirtualne (bgpvm i natvm), które nie są przyłączane do domeny usługi Azure Stack. Aby oddzielnie zmienić ustawienia przekazywania opinii i diagnostyki na tych maszynach wirtualnych, skorzystaj z informacji na stronie [http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq).

## Następne kroki

[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)



<!--HONumber=Sep16_HO3-->


