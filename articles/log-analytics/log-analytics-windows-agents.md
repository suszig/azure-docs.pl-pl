---
title: "Łączenia komputerów z systemem Windows Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono kroki dotyczące łączenia komputerów z systemem Windows w infrastrukturze lokalnej usługi analizy dzienników przy użyciu dostosowanej wersji programu Microsoft Monitoring Agent (MMA)."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aaf3e596f8c287c60531a6911c5797b3de26e570
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Łączenie komputerów z systemem Windows z usługą analizy dzienników na platformie Azure

W tym artykule przedstawiono kroki dotyczące łączenia komputerów z systemem Windows w infrastrukturze lokalnej OMS obszarów roboczych przy użyciu dostosowanej wersji programu Microsoft Monitoring Agent (MMA). Należy zainstalować i nawiąż połączenie agentów dla wszystkich komputerów, które chcesz dołączyć, aby je do wysyłania danych z usługą analizy dzienników oraz do wyświetlania i korzystania z tych danych. Każdy agent może raportować do wielu obszarów roboczych.

Można zainstalować agentów przy użyciu wiersza polecenia Instalatora lub z żądanego stanu konfiguracji (DSC) w automatyzacji Azure.  

>[!NOTE]
Dla maszyn wirtualnych działających na platformie Azure, można uprościć instalacji przy użyciu [rozszerzenie maszyny wirtualnej](../virtual-machines/windows/extensions-oms.md).

Na komputerach z połączeniem internetowym agent używa połączenia z Internetem do wysyłania danych z usługą OMS. W przypadku komputerów, które nie ma łączności z Internetem, można użyć serwera proxy lub [bramy OMS](log-analytics-oms-gateway.md).

Łączenie komputerów z systemem Windows z usługą OMS jest proste, przy użyciu trzech prostych kroków:

1. Pobierz plik Instalatora agenta z portalu OMS
2. Zainstaluj agenta przy użyciu wybranej metody
3. Skonfiguruj agenta lub Dodaj dodatkowe obszary robocze, w razie potrzeby

Na poniższym diagramie przedstawiono relację między komputerami z systemem Windows i pakietu OMS po zainstalowaniu i skonfigurować agentów.

![OMS — direct — agent — diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

Jeśli zasady zabezpieczeń IT nie zezwalają na komputerach w sieci, aby nawiązać połączenie z Internetem, można skonfigurować komputery do łączenia się z bramą OMS. Aby uzyskać więcej informacji i kroki dotyczące sposobu konfigurowania serwerów do komunikacji za pośrednictwem bramy OMS z usługą OMS, zobacz [łączenia komputerów przy użyciu bramy OMS OMS](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Wymagania systemowe i wymaganej konfiguracji
Przed zainstalowaniem lub wdrożyć agentów, przejrzyj następujące informacje, aby upewnić się, że spełniają wymagania.

- OMS MMA można zainstalować tylko na komputerach z systemem Windows Server 2008 SP 1 lub nowszym lub Windows 7 z dodatkiem SP1 lub nowszym.
- Potrzebujesz subskrypcji platformy Azure.  Aby uzyskać więcej informacji, zobacz [wprowadzenie do analizy dzienników](log-analytics-get-started.md).
- Każdy komputer z systemem Windows musi mieć możliwość połączenia z Internetem przy użyciu protokołu HTTPS lub bramy OMS. To połączenie może być bezpośrednio, za pośrednictwem serwera proxy lub za pośrednictwem bramy OMS.
- OMS MMA można zainstalować na komputerach autonomicznych, serwerów i maszyn wirtualnych. Jeśli chcesz połączyć się OMS z maszynami wirtualnymi hostowanymi na platformie Azure, zobacz [łączenie maszyn wirtualnych platformy Azure do analizy dzienników](log-analytics-azure-vm-extension.md).
- Agent musi wybrać TCP port 443 dla różnych zasobów.

### <a name="network"></a>Sieć

Dla agentów systemu Windows połączyć się i zarejestruj usługę muszą mieć dostęp do zasobów sieciowych, w tym adresy URL domeny i numery portów.

- W przypadku serwerów proxy konieczne jest zapewnienie, że ich odpowiednie zasoby są skonfigurowane w ustawieniach agenta.
- Dla zapór, które ograniczają dostęp do Internetu należy lub Twojej sieci engineers, należy skonfigurować zaporę tak, aby zezwolić na dostęp do pakietu OMS. W ustawieniach agenta nie jest wymagana żadna akcja.

W poniższej tabeli przedstawiono zasoby wymagane do komunikacji.

>[!NOTE]
>Niektóre z następujących zasobów wspomina Operational Insights, który został poprzednia nazwa protokołu analizy dzienników.

| Zasób agenta | Porty | Obejście inspekcji HTTPS |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Tak |
| *.oms.opinsights.azure.com | 443 | Tak |
| *.blob.core.windows.net | 443 | Tak |
| *.azure-automation.net | 443 | Tak |



## <a name="download-the-agent-setup-file-from-oms"></a>Pobierz plik Instalatora agenta z usługą OMS
1. W [portalu OMS](https://www.mms.microsoft.com)na **omówienie** kliknij przycisk **ustawienia** kafelka.  Kliknij przycisk **połączonych źródeł** u góry.  
    ![Połączone kartę źródła](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Kliknij przycisk **serwerów z systemem Windows** , a następnie kliknij przycisk **Pobierz agenta systemu Windows** zastosowania do typu procesora komputera można pobrać pliku ustawień.
3. Po prawej stronie **identyfikator obszaru roboczego**, kliknij ikonę kopiowania i wklejania identyfikator do Notatnika.
4. Po prawej stronie **klucz podstawowy**, kliknij ikonę kopiowania i wklejania klucza do Notatnika.     

## <a name="install-the-agent-using-setup"></a>Zainstaluj agenta przy użyciu Instalatora
1. Uruchom Instalatora, aby zainstalować agenta na komputerze, na którym chcesz zarządzać.
2. Na stronie powitalnej kliknij **dalej**.
3. Na stronie postanowienia licencyjne przeczytaj licencji, a następnie kliknij przycisk **zgadzam się**.
4. Na stronie folderu docelowego, zmienić lub zachować domyślny folder instalacji, a następnie kliknij przycisk **dalej**.
5. Na stronie Opcje instalacji agenta można połączyć agenta do Analiza dzienników Azure (OMS), Operations Manager lub puste opcji, jeśli chcesz później skonfigurować agenta. Kliknij przycisk **Dalej**.   
    - Jeśli zdecydujesz się na łączenie się Analiza dzienników Azure (OMS), Wklej **identyfikator obszaru roboczego** i **klucz obszaru roboczego (klucz podstawowy)** , który został skopiowany do Notatnika w poprzedniej procedurze, a następnie kliknij przycisk **dalej** .  
        ![Wklej klucz podstawowy i identyfikator obszaru roboczego](./media/log-analytics-windows-agents/connect-workspace.png)
    - Jeśli chcesz nawiązać połączenie programu Operations Manager, wpisz **Nazwa grupy zarządzania**, **serwera zarządzania** nazwa, i **portu serwera zarządzania**, a następnie kliknij przycisk **Dalej**. Na stronie konto akcji agenta, wybierz konto systemu lokalnego lub konta domeny lokalnej, a następnie kliknij przycisk **dalej**.  
        ![Konfiguracja grupy zarządzania](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![konto akcji agenta](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Na stronie gotowy do instalacji, przejrzyj wybrane opcje, a następnie kliknij przycisk **zainstalować**.
7. W konfiguracji została ukończona pomyślnie kliknij pozycję **Zakończ**.
8. Po zakończeniu **programu Microsoft Monitoring Agent** pojawia się w **Panelu sterowania**. Można sprawdzić konfigurację istnieje i sprawdź, czy agent jest podłączony do Operational Insights (OMS). Podczas połączenia z usługą OMS, agent wyświetla komunikat z informacją: **Microsoft Monitoring Agent pomyślnie połączył się z usługą Microsoft Operations Management Suite.**

## <a name="configure-proxy-settings"></a>Skonfiguruj ustawienia serwera proxy

Poniższa procedura pozwala skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą Panelu sterowania. Należy użyć tej procedury dla każdego serwera. Jeśli masz wiele serwerów, które trzeba skonfigurować, łatwiejszym rozwiązaniem może być użycie skryptu automatyzującego ten proces. W takim przypadku zobacz następną procedurę [Aby skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą skryptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Aby skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą Panelu sterowania
1. Otwórz **Panel sterowania**.
2. Otwórz program **Microsoft Monitoring Agent**.
3. Kliknij kartę **Ustawienia serwera proxy**.  
    ![karta Ustawienia serwera proxy](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. Zaznacz pozycję **Użyj serwera proxy**, a następnie wpisz adres URL i numer portu, jeśli jest potrzebny, podobnie jak pokazano na przykładzie. Jeśli Twój serwer proxy wymaga uwierzytelniania, wpisz nazwę użytkownika i hasło, aby uzyskać dostęp do serwera proxy.


### <a name="verify-agent-connectivity-to-oms"></a>Sprawdź łączność agent pakietu OMS

Można łatwo sprawdzić, czy agentów komunikują się z OMS, korzystając z następującej procedury:

1.  Na komputerze z agentem systemu Windows otwórz Panel sterowania.
2.  Otwórz program Microsoft Monitoring Agent.
3.  Kliknij kartę Analiza dzienników Azure (OMS).
4.  W kolumnie Stan powinny być widoczne czy agent pomyślnie połączony z usługą Operations Management Suite.

![Agent połączone](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Aby skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą skryptu
Skopiuj poniższy przykład, zaktualizuj go informacjami specyficznymi dla Twojego środowiska, zapisz go z rozszerzeniem nazwy pliku PS1, a następnie uruchom skrypt na każdym komputerze, który łączy się bezpośrednio z usługą OMS.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-the-agent-using-the-command-line"></a>Zainstaluj agenta przy użyciu wiersza polecenia
- Zmodyfikuj, a następnie skorzystaj z następującego przykładu, aby zainstalować agenta przy użyciu wiersza polecenia. W przykładzie jest wykonywane pełni dyskretnej instalacji.

    >[!NOTE]
    Jeśli chcesz uaktualnić agenta, należy użyć analizy dzienników interfejs API obsługi skryptów. Zobacz następną sekcję, aby uaktualnić agenta.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

Agent używa IExpress jako jego własny ekstraktor przy użyciu `/c` polecenia. Widać przełączniki wiersza polecenia w [przełączniki wiersza polecenia dla IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) , a następnie zaktualizować przykładu do własnych potrzeb.

|Opcje MMA                   |Uwagi         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Konfigurowanie agenta z obszaru roboczego                |
|OPINSIGHTS_WORKSPACE_ID                | Identyfikator obszaru roboczego (guid) dla obszaru roboczego do dodania                    |
|OPINSIGHTS_WORKSPACE_KEY               | Klucz obszaru roboczego używany do uwierzytelniania początkowo z obszaru roboczego |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Określ środowiska chmury, w którym znajduje się obszar roboczy <br> 0 = komercyjnych chmury azure (ustawienie domyślne) <br> 1 = azure dla instytucji rządowych |
|OPINSIGHTS_PROXY_URL               | Identyfikator URI serwera proxy do użycia |
|OPINSIGHTS_PROXY_USERNAME               | Nazwa użytkownika do uzyskania dostępu uwierzytelnionego serwera proxy |
|OPINSIGHTS_PROXY_PASSWORD               | Hasło dostępu uwierzytelnionego serwera proxy |

>[!NOTE]
Aby uniknąć naciśnięcie limit długości wiersza polecenia IExpress, zainstaluj agenta z obszarem roboczym, nie skonfigurowany, a następnie użyj skryptu konfiguracji dla obszaru roboczego.

>[!NOTE]
Jeśli otrzymasz `Command line option syntax error.` przy użyciu `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parametru, można użyć następującego obejścia:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Dodaj obszar roboczy za pomocą skryptu
Dodaj obszar roboczy za pomocą interfejsu API skryptów agent analizy dzienników z następującym przykładzie:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

Aby dodać obszar roboczy na platformie Azure dla instytucji rządowych Stanów Zjednoczonych, skorzystaj z poniższego przykładu skryptu:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Jeśli używano wiersza polecenia lub skryptu wcześniej do zainstalowania i skonfigurowania agenta, `EnableAzureOperationalInsights` została zastąpiona `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Zainstaluj agenta przy użyciu usługi Konfiguracja DSC automatyzacji Azure

Poniższy przykładowy skrypt służy do instalowania agenta przy użyciu usługi Konfiguracja DSC automatyzacji Azure. W przykładzie przedstawiono instalację agenta 64-bitowe, zidentyfikowane przez `URI` wartość. Umożliwia także 32-bitowej wersji zastępując wartość identyfikatora URI. Identyfikatory URI dla obu wersji są:

- Agent 64-bitowego systemu Windows — https://go.microsoft.com/fwlink/?LinkId=828603
- Agent 32-bitowego systemu Windows — https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
W tym przykładzie procedury i skrypt nie spowoduje uaktualnienie istniejącego agenta.

1. Importowanie konfiguracji DSC xPSDesiredStateConfiguration modułu na podstawie [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) w automatyzacji Azure.  
2.  Tworzenie zasobów zmiennej usługi Automatyzacja Azure *OPSINSIGHTS_WS_ID* i *OPSINSIGHTS_WS_KEY*. Ustaw *OPSINSIGHTS_WS_ID* do zestawu i identyfikator obszaru roboczego analizy dzienników OMS *OPSINSIGHTS_WS_KEY* w kluczu podstawowym obszaru roboczego.
3.  Użyj następującego skryptu i zapisać go jako MMAgent.ps1
4.  Zmodyfikuj, a następnie skorzystaj z następującego przykładu, aby zainstalować agenta przy użyciu usługi Konfiguracja DSC automatyzacji Azure. Zaimportuj MMAgent.ps1 do automatyzacji Azure za pomocą usługi Automatyzacja Azure, interfejsu lub apletu polecenia.
5.  Przypisać konfiguracji węzła. W ciągu 15 minut węzeł sprawdza konfigurację i MMA zostanie przypisany do węzła.

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-the-latest-productid-value"></a>Pobierz najnowszą wartość ProductId

`ProductId value` W MMAgent.ps1 skrypt jest unikatowa dla każdej wersji agenta. Po opublikowaniu zaktualizowaną wersję każdego agenta, wartość ProductId zostanie zmieniona. Tak gdy ProductId zmieni się w przyszłości, można znaleźć wersji agenta za pomocą prostego skryptu. Po umieszczeniu najnowszą wersję agenta zainstalowana na serwerze testowym, służy poniższy skrypt można uzyskać wartość ProductId zainstalowane. Ostatnia wartość ProductId można aktualizować wartość w skrypcie MMAgent.ps1.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Ręcznie skonfigurować agenta, lub Dodaj dodatkowe obszary robocze
Jeśli po zainstalowaniu agentów, ale ich nie skonfigurowano lub jeśli Agent ma raportować do wielu obszarów roboczych, można użyć następujące informacje, aby włączyć agenta lub ponownie go skonfigurować. Po skonfigurowaniu agent, będą rejestrować się w usłudze agenta i niezbędnych informacji konfiguracyjnych oraz pakietów administracyjnych, które zawierają informacje o rozwiązaniu.

1. Po zainstalowaniu programu Microsoft Monitoring Agent, otwórz **Panelu sterowania**.
2. Otwórz **programu Microsoft Monitoring Agent** , a następnie kliknij przycisk **Analiza dzienników Azure (OMS)** kartę.   
3. Kliknij przycisk **Dodaj** otworzyć **Dodaj obszar roboczy analizy dzienników** pole.
4. Wklej **identyfikator obszaru roboczego** i **klucz obszaru roboczego (klucz podstawowy)** skopiowanego do Notatnika w poprzedniej procedurze dla obszaru roboczego, który chcesz dodać, a następnie kliknij przycisk **OK**.  
    ![Konfigurowanie usługi Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)

Po zebraniu danych z komputerów monitorowanych przez agenta, liczbę komputerów monitorowanych przez OMS będą wyświetlane w portalu OMS na **połączonych źródeł** karcie **ustawienia** jako **serwerów Połączone**.


## <a name="to-disable-an-agent"></a>Aby wyłączyć agenta
1. Po zainstalowaniu agenta, należy otworzyć **Panelu sterowania**.
2. Otwórz program Microsoft Monitoring Agent, a następnie kliknij przycisk **Analiza dzienników Azure (OMS)** kartę.
3. Wybierz obszar roboczy, a następnie kliknij przycisk **Usuń**. Powtórz ten krok dla wszystkich innych obszarów roboczych.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Opcjonalnie możesz skonfigurować agentów do raportu do grupy zarządzania programu Operations Manager

Jeśli używasz programu Operations Manager w infrastrukturze IT, umożliwia także MMA agent jako agent programu Operations Manager.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>Aby skonfigurować agentów MMA zgłoszenia do grupy zarządzania programu Operations Manager
1.  Na komputerze, na których jest zainstalowany agent, otwórz **Panelu sterowania**.  
2.  Otwórz **programu Microsoft Monitoring Agent** , a następnie kliknij przycisk **programu Operations Manager** kartę.  
    ![Karta Microsoft Monitoring Agent programu Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)
3.  Jeśli serwery programu Operations Manager są zintegrowane z usługą Active Directory, kliknij przycisk **automatycznie Aktualizuj przypisania grupy zarządzania z usług AD DS**.
4.  Kliknij przycisk **Dodaj** otworzyć **Dodaj grupę zarządzania** okno dialogowe.  
    ![Program Microsoft Monitoring Agent Dodaj grupę zarządzania](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  W **Nazwa grupy zarządzania** wpisz nazwę grupy zarządzania.
6.  W **podstawowego serwera zarządzania** wpisz nazwę komputera w podstawowy serwer zarządzania.
7.  W **port serwera zarządzania** wpisz numer portu TCP.
8.  W obszarze **konto akcji agenta**, wybierz konto systemu lokalnego lub konta domeny lokalnej.
9.  Kliknij przycisk **OK** zamknąć **Dodaj grupę zarządzania** okno dialogowe, a następnie kliknij przycisk **OK** zamknąć **właściwości agenta monitorowania firmy Microsoft** okno dialogowe.


## <a name="next-steps"></a>Następne kroki

- [Dodaj rozwiązania Log Analytics z galerii rozwiązań](log-analytics-add-solutions.md), aby dodać funkcje i zebrać dane.
